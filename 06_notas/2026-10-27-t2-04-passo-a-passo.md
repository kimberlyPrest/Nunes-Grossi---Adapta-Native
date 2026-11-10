# T2.04 — Passo a passo para iniciar

**Task:** T2.04 — Menus e rotas por perfil + tela "sem acesso" + regressão
**SPEC:** SPEC-1-002 (acesso e segurança) · **Critério:** CA-1-05
**Última task da Leva 2** · prazo original 26/10 · dono: Maestro
**Estado:** `aguardando_autorizacao` — nada implementado

---

## 0. Antes de começar: o que a análise encontrou

A task parecia maior do que é. **Metade do CA-1-05 já está implementada:**

| Item do CA-1-05 | Situação real | Evidência |
|---|---|---|
| Usuários e Configurações não aparecem no menu do colaborador | **Já funciona** | `Layout.tsx`: `NAV_ITEMS` com `adminOnly` e filtro `isAdmin`; o mesmo `navItems` alimenta menu desktop e mobile, então não há divergência entre eles |
| Acesso por endereço direto é bloqueado | **Já funciona** | `App.tsx` envolve `/usuarios`, `/configuracoes/*` em `AdminRoute`; `AdminRoute` devolve `<Navigate to="/minha-fila" replace />` |
| Ao ser bloqueado, mostra "sem acesso" | **NÃO** | O colaborador é redirecionado sem nenhuma explicação |
| Mensagem coerente | **NÃO em 4 telas** | ver abaixo |

### O furo 1 — bloqueio silencioso

Colaborador digita `/usuarios` e cai em `/minha-fila` sem saber por quê. Pode achar que errou o
endereço, que perdeu acesso ou que o sistema quebrou. Isso não é "tela de sem acesso": é um
redirecionamento mudo.

### O furo 2 — mensagem contraditória em 4 telas

`PageStateLayout` responde ao estado `error` com **"Houve uma instabilidade temporária"** e botão
**"Tentar novamente"**. Quatro telas usam esse estado para casos de **permissão**, não de falha:

| Tela | O que mostra hoje | Contradição |
|---|---|---|
| `OportunidadeDetalhes` | "Oportunidade não encontrada / você não tem acesso" + card "instabilidade temporária" | convida a tentar de novo o que nunca vai funcionar |
| `ClienteDetalhes` | "Cliente não encontrado / não tem permissão" + card "instabilidade temporária" | idem |
| `MinhaFila` | título "Minha fila" + "falha na comunicação com o banco de dados" | atribui a falha técnica um problema que pode ser de tela |
| `NotFound` (404) | já é uma tela própria e correta | **não precisa mexer** |

**Correção do que eu disse na análise anterior:** falei em "7 telas com o defeito". Ao conferir uma a
uma, são **4** (`OportunidadeDetalhes`, `ClienteDetalhes`, `MinhaFila` e a percepção de falha em
`Clientes`/`Fluxos`/`Catalogo`/`Usuarios`, que usam `error` só para falha de rede — nelas o estado
está correto). Registro a correção porque a diferença muda o escopo: **não vou reescrever 7 telas.**

---

## 1. Ordem de execução

### Passo 1 — Criar a tela de sem acesso

Arquivo novo: `src/pages/SemAcesso.tsx`.

Modelo: seguir o padrão visual de `NotFound.tsx` (logo, bloco central, botão), porque já existe e é
consistente com a marca.

Conteúdo obrigatório:
- título: **"Você não tem acesso a esta área"**
- explicação em uma frase: a área é restrita a administradores; se precisar de acesso, falar com um
  administrador;
- **um** botão: "Ir para minha fila";
- **nenhum** botão de "tentar novamente" — não há nada a tentar.

Registrar a rota em `App.tsx`, **dentro** de `ProtectedRoute` e **fora** de `AdminRoute`
(o colaborador precisa poder ver a tela; se ficasse dentro de `AdminRoute`, entraria em laço).

### Passo 2 — AdminRoute mostra a tela em vez de redirecionar

Em `src/components/AdminRoute.tsx`, trocar:

```tsx
if (!isAdmin) {
  return <Navigate to="/minha-fila" replace />
}
```

por renderizar o `SemAcesso`. Manter o `Navigate` **apenas** se houver motivo para a rota `/sem-acesso`
não existir. O bloqueio continua idêntico — muda só o que a pessoa vê.

Cuidado: `AdminRoute` também envolve `/configuracoes` (que redireciona para `/configuracoes/fluxos`).
Testar as duas entradas.

### Passo 3 — Separar os dois significados de `status="error"`

Em `src/components/PageStateLayout.tsx`, hoje o estado `error` tem texto fixo de instabilidade. Duas
opções; escolho a **B**:

- **A)** adicionar um estado novo `denied`; ou
- **B)** permitir que o texto do estado `error` seja parametrizado (`errorMessage`, `retryLabel`) e
  deixar as telas decidirem o que dizer.

B é menor e não mexe na API de todas as chamadas. O padrão de "instabilidade temporária" continua como
**default**; as telas de permissão passam o texto certo e **escondem** o botão de repetir.

### Passo 4 — Ajustar as 4 telas

| Arquivo | Mudança |
|---|---|
| `OportunidadeDetalhes.tsx` | caso "não encontrada / sem acesso": mensagem de permissão, sem "tentar novamente"; manter repetir só para falha de rede |
| `ClienteDetalhes.tsx` | idem |
| `MinhaFila.tsx` | mensagem neutra (não atribuir a falha de banco) e repetir apenas quando for erro real |
| `NotFound.tsx` | **não mexer** |

Regra de decisão em cada tela: se a consulta voltou **vazia por permissão** (0 linhas) → permissão; se
veio **erro de rede/banco** → falha técnica. Onde as duas coisas caem no mesmo `catch`, separar.

### Passo 5 — Regressão obrigatória

Este é o passo que fecha a task e o que mais pode quebrar. Ver seção 3.

---

## 2. Verificação automática antes de pedir teste

```bash
npx tsc --noEmit     # 0 erros
npx vite build       # sucesso
npx oxlint src       # sem erro novo (hoje: 0 erros, warnings preexistentes)
```

Se a migração não muda, **não há migração nesta task** — é só frontend. Consequência prática: o teste
no Skip é mais simples (não precisa aplicar SQL), e o deploy pode ser conferido pelo hash do bundle
(AP-20261027-1340).

---

## 3. Teste humano — precisa de 3 perfis

O de maior risco. Reserve tempo; não é um teste de 2 minutos como o da T2.03.

### Colaborador
1. Entrar como colaborador Saúde ou Seguros.
2. Conferir o menu: **não** deve haver "Usuários" nem "Configurações".
3. Digitar na barra de endereço `.../usuarios` → deve aparecer **"Você não tem acesso a esta área"**,
   com o botão "Ir para minha fila"; **não** pode cair na fila sem explicação.
4. Repetir com `.../configuracoes/fluxos` e `.../configuracoes/catalogo`.
5. Abrir uma oportunidade de outra equipe pelo endereço direto → "sem acesso" **sem** a palavra
   "instabilidade" e **sem** botão "tentar novamente".

### Administrador
6. Menu deve mostrar Usuários, Fluxos e Catálogo.
7. Abrir `/usuarios`, `/configuracoes/fluxos` e `/configuracoes/catalogo` normalmente.
8. Arquivar um cliente (regressão da T2.02).

### Gestor
9. Menu **não** deve mostrar Usuários nem Configurações.
10. Digitar `/usuarios` → tela de sem acesso.
11. Ver as oportunidades da sua equipe normalmente.

### Regressão das tasks anteriores
12. T2.01: usuário inativo não entra (ou confirme que segue desativado).
13. T2.02: não existe botão de excluir; arquivar só admin/gestor.
14. T2.03: baixar documento de uma oportunidade em que o usuário participa continua funcionando.

**Reconhecer FALHA:** colaborador vendo "Usuários" no menu; acesso direto caindo na fila sem
explicação; qualquer tela de permissão mostrando "instabilidade temporária" ou "tentar novamente";
admin perdendo acesso a qualquer tela.

---

## 4. O que NÃO entra nesta task

- Reescrever as 7 telas (só 4 têm o defeito).
- Mudar a política de quem vê o quê (é T2.02, já fechada).
- Criar tela de "sem acesso" dentro do layout com menu — a tela nova é de página inteira, como o 404.
- As dúvidas D-19 e D-20 (aguardam a Kimberly).

---

## 5. Riscos

| Risco | Mitigação |
|---|---|
| Quebrar estado de carregamento ou vazio legítimo em 4 telas | Comparar antes/depois da mesma tela; testar carregando e vazio, não só o caso de permissão |
| `SemAcesso` dentro de `AdminRoute` criar laço | Registrar a rota em `ProtectedRoute`, fora de `AdminRoute` |
| Mensagem de permissão esconder erro real de banco | Separar os dois caminhos no `catch`; erro de rede continua mostrando "tentar novamente" |
| Regressão das 3 tasks anteriores | Seção 3, itens 12–14 |
