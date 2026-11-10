# Debug — T2.01 (2ª rodada): desativar usuário falha com "Não foi possível alterar a situação do usuário."

**Data:** 26/10/2026 · **Task:** T2.01 · **SPEC:** SPEC-1-002 · **Relatado por:** Pedro (print do modal "Desativar usuário?")

## Contexto positivo (evidências de que a publicação funcionou)

- A mensagem de erro mudou: antes o genérico "Edge Function returned a non-2xx status code", agora a
  mensagem real da edge function — prova de que o frontend do commit `64850af` está publicado.
- A lista mostra **4 usuários** (incluindo viviane@corretorang.com.br, Administrador) — o caminho de
  convite funcionou após a publicação.
- **Sintoma**

Administrador clica em Desativar no usuário "colaborador seguros" (louise.corretorang@gmail.com) →
banner vermelho "Erro ao alterar situação — Não foi possível alterar a situação do usuário."

## Cadeia causal rastreada

A mensagem exibida é `body.error` da edge function (`toggle_status`), que só é devolvida quando o
`UPDATE public.profiles SET situacao = ...` (com service_role, que ignora RLS) **falha**. Com
service_role e a linha existindo, um UPDATE simples não falha sozinho — sobra o que roda DENTRO dele:

**Hipótese 1 (principal):** a migração `20261020110000_t201_bloqueio_inativo.sql` foi aplicada e o
trigger `trg_profiles_situacao_login_ban` erro durante o UPDATE (provável na escrita em
`auth.users`/`auth.refresh_tokens` — permissão do role postgres ou estrutura da tabela). Erro no
trigger desfaz o UPDATE inteiro → toggleErr → 400 com essa mensagem.
- Teste de descarte: `BEGIN; UPDATE public.profiles SET situacao='inativo' WHERE email='louise.corretorang@gmail.com'; ROLLBACK;` no SQL editor — se estourar erro, é esta hipótese; o texto do erro nomeia a instrução exata.

**Hipótese 2:** migração NÃO aplicada e falha no adminClient da função (env/permissão). Parcialmente
contradita pelo convite funcionar (mesmo client service_role). O mesmo teste descarta: se o UPDATE
passa sem erro, o trigger não existe (ou não é ele) → investigar logs da edge function.

**Hipótese 3 (fraca):** edge function antiga no ar — indiferente ao sintoma (a mensagem de erro é
idêntica nas duas versões); conferir versão publicada no Skip.

## Correção

Ainda não aplicada — aguardando o texto exato do erro (uma hipótese e uma correção por vez).
Correções candidatas conforme a evidência:
- Se falhar em `auth.refresh_tokens`: mover a revogação de sessões para a edge function (que já faz
  via `auth.admin.signOut`) e deixar no trigger só o ban em `auth.users` (ou proteger com EXCEPTION
  sem esconder the ban).
- Se falhar em `auth.users`: reavaliar o caminho do ban (manter apenas via API de admin na edge
  function, que é o caminho suportado) e simplificar o trigger.
- Se o trigger não existir: investigar logs da edge function e a aplicação da migração no Skip.

## Verificação automática

Código atual: tsc OK · lint 0 erros · build OK (26,3s). Falha é de runtime no banco real, não
reproduzível sem acesso ao Supabase da N&G.

## Gate

`em_correcao` → aguardando evidência (consulta de triggers + teste SQL) → volta a
`aguardando_teste_humano` após a correção.
