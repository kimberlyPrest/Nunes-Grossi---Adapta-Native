# AP-2026-11-10-0943 — Regra de negócio em RPC não se prova com UPDATE direto

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T5.03 (SPEC-1-005, CA-1-22)
- Sinal: a primeira tentativa de revalidar as regras de ganho e perda em Postgres real aplicou
  `UPDATE oportunidades SET situacao='Ganha' ...` e **8 de 10 casos falharam**, sugerindo que a
  migração não valia nada. A leitura da migração mostrou que as regras vivem nas funções
  `confirmar_ganho_oportunidade` e `marcar_oportunidade_perdida` (RPC chamada pela tela), não em
  trigger — o `UPDATE` direto nunca passava por validação alguma. Reescrito o teste para chamar as
  RPCs, o resultado foi 13/13 e, somado à regra 8, 16/16.
- Evidência: `work/tmp503/teste.cjs` e `work/tmp503/regra8.cjs` contra a migração
  `20261030190000` extraída do commit remoto; log 13 PASS/0 FAIL e 3 PASS/0 FAIL.
- Regra reutilizável: antes de escrever a prova de uma regra de banco, localizar **onde** a regra
  mora — trigger, `CHECK`, RLS ou função RPC — e exercitar exatamente essa porta. Testar por
  `UPDATE` uma regra que só existe numa RPC produz falso negativo em massa.
- Quando aplicar: toda task cujo critério de aceite é uma regra de negócio validada no banco
  (ganho, perda, transição de etapa, permissão de movimento).
- Quando não aplicar: quando a regra é de fato trigger ou `CHECK` — aí o `UPDATE` direto é o
  caminho certo e a RPC seria camada a mais.
- Confiança: alta — o falso negativo apareceu e desapareceu com a única mudança de chamar a RPC.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.

## Detalhe de montagem (evita repetir o retrabalho)

O esquema mínimo de teste precisou de quatro correções sucessivas, todas do **teste**, nenhuma da
migração: enum `user_funcao`; `updated_at` em `oportunidades_etapas`; `funcao` em `profiles` (não
na etapa); `profile_equipes`, `oportunidades_checklist_itens` e `oportunidades_documentos`; e o stub
`pode_movimentar_etapa(uuid, uuid, uuid)` — a assinatura real é de **três** parâmetros, descoberta
lendo a chamada na migração.

Regra derivada: mensagem de erro do PGlite do tipo "column X does not exist" durante a prova de uma
migração é quase sempre **esquema de teste incompleto**, não defeito da migração. Corrigir o
esquema e só desconfiar da migração depois que ela rodar até o fim.
