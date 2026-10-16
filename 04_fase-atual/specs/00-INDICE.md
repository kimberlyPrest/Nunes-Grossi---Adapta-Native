# Índice de SPECs — Fase 1 (Primeira jornada viva)

Ordem de execução: primeiro o **tronco** (prompts no Skip, dentro da SPEC-1-001), depois os **galhos**
(SPECs 1-002 a 1-006, executadas pelo Maestro) e, por fim, o ensaio real e o aceite (SPEC-1-007).

| SPEC | Entrega | Tipo | Depende de | Critérios | Tasks | Status |
|---|---|---|---|---|---|---|
| [SPEC-1-001](SPEC-1-001-tronco-auditado.md) | Tronco criado, auditado e mapeado | Tronco (prompts) + auditoria | — | CA-1-01 a 04 | T0.00–T0.10, T1.01–T1.02 | planejada |
| [SPEC-1-002](SPEC-1-002-acesso-e-seguranca.md) | Acesso, perfis e segurança no banco | Galho — permissão | 1-001 | CA-1-05 a 09 | T2.01–T2.04 | planejada |
| [SPEC-1-003](SPEC-1-003-clientes.md) | Cadastro de clientes confiável | Galho — dado | 1-002 | CA-1-10 a 13 | T3.01–T3.02 | planejada |
| [SPEC-1-004](SPEC-1-004-fluxos-e-abertura.md) | Fluxos corretos e abertura de oportunidade | Galho — dado + regra | 1-002 | CA-1-14 a 18 | T4.00–T4.02 | planejada |
| [SPEC-1-005](SPEC-1-005-execucao-do-fluxo.md) | Execução do fluxo: checklist, passagem, ganho e perda | Galho — regra | 1-004 | CA-1-19 a 23 | T5.01–T5.04 | planejada |
| [SPEC-1-006](SPEC-1-006-visoes-e-painel.md) | Minha fila, quadro, lista e painel inicial | Galho — tela | 1-005 | CA-1-24 a 27 | T6.01–T6.03 | planejada |
| [SPEC-1-007](SPEC-1-007-ensaio-real-e-aceite.md) | Ensaio real e aceite da Fase 1 | Validação humana | 1-001 a 1-006 | CA-1-28 a 30 | T7.01–T7.05 | planejada |

## Cobertura dos 13 testes de aceite da Fase 1 (`04_fase-atual/fase.md`)

| Teste | Coberto por |
|---|---|
| 1 — Colaborador não vê Usuários | CA-1-05 |
| 2 — CNPJ inválido recusado | CA-1-10 |
| 3 — Cliente repetido avisa | CA-1-11 |
| 4 — J6 nasce com 12 etapas | CA-1-14 |
| 5 — Produto sem fluxo usa o genérico | CA-1-15 |
| 6 — Etapa não conclui sem o obrigatório | CA-1-19 |
| 7 — Documento invisível para outra equipe | CA-1-06 |
| 8 — Passagem para a fila do pós-venda | CA-1-20 |
| 9 — Voltar etapa pede motivo | CA-1-21 |
| 10 — Perda sem motivo recusada | CA-1-22 |
| 11 — Sem botão de excluir | CA-1-07 |
| 12 — Cartão na coluna certa do quadro | CA-1-25 |
| 13 — Painel bate com os testes | CA-1-27 |
