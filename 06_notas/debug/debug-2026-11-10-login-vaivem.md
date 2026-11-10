# Debug — vaivém na tela de login (tela "pisca" ~3x antes de entrar)

- **Data:** 10/11/2026
- **Relatado por:** Pedro (antes do teste da T6.01)
- **Sintoma:** ao entrar com e-mail e senha, a tela pisca várias vezes e volta para o formulário
  umas 3 vezes antes de finalmente abrir o sistema.
- **Status:** corrigido, aguardando teste humano.

## Reprodução

Não foi possível reproduzir em navegador (sem sessão válida no sandbox), então a cadeia foi
reproduzida por **simulação da máquina de estados** do login, com o comportamento antes e depois
da correção. A simulação mostra 2 navegações antes (com o vaivém) e 1 depois, nos três tempos de
carregamento de perfil testados (20 ms, 120 ms, 300 ms).

## Causa raiz

Três peças se combinam. Nenhuma delas erra sozinha — o defeito é a interação:

1. `useAuth` só marcava `loading = true` no primeiro mount. Ao entrar, `loading` já era `false`,
   então o app se considerava pronto **antes** de o perfil chegar.
2. `situacao = profile?.situacao || 'inativo'` (proteção da T2.01). Com `profile` ainda `null`,
   o usuário é classificado como **inativo**.
3. `ProtectedRoute` fazia `if (situacao === 'inativo') return <Navigate to="/" replace />`, e o
   `Index` em `/` faz `if (session) navigate('/minha-fila')`.

Sequência real da falha:

```
login OK -> sessão existe, perfil ainda carregando
  -> Index vê session e manda para /minha-fila
  -> ProtectedRoute vê profile=null -> situacao='inativo' -> devolve para /
  -> Index vê session de novo -> manda para /minha-fila
  -> (repete enquanto o perfil não chega)
  -> perfil chega -> entra de fato
```

O "piscar" é o React montando e desmontando a área logada nesse ciclo. O número de voltas depende
da latência da consulta de perfil — por isso "umas 3 vezes".

**Por que não apareceu antes:** qualquer lentidão maior no carregamento do perfil (rede, banco)
aumenta o número de voltas. Deve ter piorado com o crescimento dos dados de perfil/equipes.

## Correção

1. `use-auth.tsx`: `loading` passou a ser **derivado** —
   `carregando || (session && !perfilConsultado)`. Enquanto há sessão e o perfil não foi
   consultado, o app continua em carregamento e não navega. A marca `perfilConsultado` é
   necessária para não travar para sempre um usuário sem registro em `profiles`; ela é zerada no
   `signOut` para o próximo login recarregar.
2. `ProtectedRoute.tsx`: sem `profile` carregado, mostra a tela de "Carregando seu acesso..."
   em vez de tratar o usuário como inativo. A expulsão de conta **inativa de verdade** continua
   funcionando — agora ela exige `profile` carregado.

## Verificação automática

- Simulação antes × depois: 3 cenários, ANTES 2 navegações, DEPOIS 1 → PASS.
- `tsc` 0 erros · `oxlint` 71 warnings / 0 errors (= baseline) · `vite build` OK a partir do
  commit `25be730` → `index-DMKkV_Ia.js`.
- Os quatro arquivos conferidos byte a byte contra o remoto (20 bytes de diferença tolerados não).

## Riscos residuais

- Não reproduzi em navegador real: a prova é a simulação da máquina de estados, não o clique.
- A T2.01 continua valendo: quem **não tem** perfil em `profiles` não entra (agora cai no
  formulário sem vaivém). Se algum usuário legítimo estiver sem registro, o sintoma muda de
  "piscar" para "não entra" — vale um olhar no teste.
