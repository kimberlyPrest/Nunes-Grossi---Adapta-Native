# AP-20261030-1432 — Roteiro de teste humano deve nascer da SPEC, não do plano de implementação

- **Data**: 2026-10-30T14:32:00-03:00
- **Origem**: T5.01 (Central do Fluxo Comercial, N&G)
- **Tipo**: processo de verificação
- **Evidência**: item 4 do roteiro de teste ("origem inativa não aparece no seletor") não era testável — a emenda de 20/10 da SPEC-1-004 deixa as páginas Fluxos e Catálogo **somente leitura na Fase 1** ("a configuração completa pela interface (incluir, editar, ativar/desativar) fica explícita na Fase 2"), e as 8 origens de produção estavam todas ativas. O item gastou uma rodada de teste e produziu um "falhou" que não era falha do sistema.

## Sinal reutilizável

Ao montar o roteiro de teste humano, cada item deve citar a origem do comportamento (CA da SPEC
ou emenda com data) e ser conferido contra o que a fase entrega. Item que depende de uma tela
declarada fora de escopo da fase não entra no roteiro como teste de tela: ou vira prova indireta
(banco + bundle), ou fica registrado como residual da fase que entrega a tela. Antes de enviar o
roteiro, cruzar cada item com as emendas "Fora de escopo" das SPECs da task.

## Como aplicar

1. Listar os itens do roteiro com o CA/emenda que origina cada um.
2. Conferir emendas "Fora de escopo" da SPEC — se o item depende de tela/ação fora do escopo,
   substituir por prova indireta ou residual.
3. No relatório ao cliente, marcar o item como "não testável na fase X (motivo)" em vez de
   "falhou" — evita leitura de regressão onde não há.
