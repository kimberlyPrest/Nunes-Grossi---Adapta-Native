# Debug — T2.01: "Edge Function returned a non-2xx status code" ao convidar usuário

**Data:** 20/10/2026 · **Task:** T2.01 · **SPEC:** SPEC-1-002 · **Relatado por:** Pedro (print do modal "Convidar usuário")

## Sintoma

Ao convidar o usuário de teste "colaborador saude" (louisefaraguti@gmail.com, Colaborador/Comercial,
equipe Saúde e Odonto), o modal mostra o erro genérico **"Edge Function returned a non-2xx status
code"** — sem a mensagem real da função.

## Reprodução

Tela de Usuários → Convidar usuário → preencher e enviar. Erro aparece no modal. Reproduzido pela
Louise/Pedro no ambiente real (Skip/Supabase da N&G).

## Diagnóstico (cadeia causal)

1. O supabase-js, ao receber status ≠ 2xx de uma edge function, lança `FunctionsHttpError` cuja
   mensagem é o genérico acima; **o corpo JSON da resposta (com a mensagem em português escrita
   pela própria função) não é lido em lugar nenhum** no `src/services/usuarios.ts`.
2. O código antigo fazia `data?.error || error.message` — mas em erro HTTP o `data` vem vazio, então
   só sobrava a mensagem genérica.
3. A mensagem real (ex.: "Não foi possível enviar o convite: ...", "Já existe um usuário cadastrado
   com este e-mail", 401/403 de autorização) existe — só se perde no caminho.

**Causa raiz (do sintoma visível):** o frontend descartava o corpo do erro da edge function.
**Causa raiz (do 4xx em si):** ainda a confirmar com a mensagem real agora visível — candidatas,
em ordem: (a) envio de e-mail de convite recusado pelo serviço nativo do Supabase (limite/SMTP não
configurado no projeto da N&G); (b) autorização do chamador (401/403); (c) e-mail já existente.

## Correção aplicada (dentro do recorte da task — caminhos de erro obrigatórios da SPEC)

- `src/services/usuarios.ts`: novo helper `extrairErroEdge()` que lê o corpo JSON do
  `FunctionsHttpError` (via `error.json()` ou `error.context.json()`) e devolve a mensagem real;
  aplicado em `inviteUser`, `updateUser` and toggleUserStatus.
- Removidos os **fallbacks diretos ao banco** em `updateUser` e `toggleUserStatus`: eles escondiam
  falhas da função (o toggle precisa do ban/revogação em `auth.users`, que só a função/trigger
  garante — cair no banco silenciosamente mascararia exatamente o tipo de problema visto agora).
- A partir de agora, o modal mostra a mensagem real em português (ex.: o motivo exato da recusa do
  convite), o que permite fechar o diagnóstico do 4xx.

## Verificação automática

- `tsc --noEmit`: OK (0 erros) · `pnpm lint`: 0 erros (70 warnings pré-existentes, baseline) ·
  `pnpm build`: OK (26,3s).

## Próximo passo (gate)

Com a mensagem real visível, repetir o convite e reportar o texto exato do erro — se for recusa do
envio de e-mail, a configuração de SMTP do projeto Supabase (cofre) é o destravamento, com a
consultora; se for autorização, reviso a função. Estado: `em_correcao` → volta a
`aguardando_teste_humano` com o roteiro abaixo.
