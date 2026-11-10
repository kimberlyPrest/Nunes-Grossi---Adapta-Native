# AP-20261027-1430 — Medir permissão pelo lado que a política usa

**Data:** 27/10/2026 · **Task:** T2.03 · **Evidência:** `06_notas/2026-10-27-t2-03-prova-green.md`

## Sinal reutilizável

Ao provar que um recurso está protegido, **teste a função que a política chama**, não o efeito
indireto. Na T2.03 a política do bucket é:

```sql
USING (... AND public.pode_ver_oportunidade(public.oportunidade_do_caminho(name), auth.uid()))
```

Provar `pode_ver_oportunidade(oportunidade, usuario) = false` para o colaborador de Seguros conclui
o critério com uma consulta, sem precisar reproduzir no navegador o cenário de alguém com o caminho
do arquivo em mãos. É a mesma lógica da T2.02, onde a view `verificacao_regras_de_acesso` prova a
configuração e o teste `authenticated` prova o comportamento.

## Como aplicar

1. Leia a expressão da política e identifique a função de decisão.
2. Chame essa função diretamente com os dois sujeitos do teste (quem deve e quem não deve).
3. Só depois, se o critério falar de tela, confirme a tela.

## Cuidado que custou tempo na T2.03

Enviar script com placeholder (`UUID-DO-COLABORADOR`) para o cliente colar faz o teste falhar por
erro de digitação e queima uma rodada. Preferir consulta que **descobre** o sujeito sozinha
(`FROM public.profiles WHERE situacao = 'ativo'`), ou entregar o valor já preenchido.

## Quando não aplicar

Quando a política depende de estado que não está em coluna (ex.: token expirado, sessão revogada) —
aí só o caminho real prova.
