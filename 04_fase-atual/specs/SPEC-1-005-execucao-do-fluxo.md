# SPEC-1-005 — Execução do fluxo: checklist, passagem de bastão, ganho e perda

**Fase:** 1 — Primeira jornada viva
**Status:** planejada
**Dono:** Maestro (implementação) · Louise (testes)
**Origem:** `04_fase-atual/fase.md` (regras e automações); `01_projeto/regras-e-permissoes.md` (RN-02, RN-03, RN-05 a RN-07)
**Degrau da solução:** reuso do que o tronco gerou (Prompts 8 e 9), com as regras de conclusão, passagem e ganho/perda no banco, para não depender só da tela

## Resultado observável

A oportunidade **anda sozinha de mão em mão**: a pessoa preenche o checklist, conclui a etapa, e a
próxima pessoa (ou a fila do pós-venda) recebe a oportunidade com aviso no sino. Etapa incompleta não
avança, perda sempre tem motivo e tudo fica registrado na linha do tempo.

## Limites e dependências

- **Inclui:** preenchimento dos 3 tipos de item (documento anexado ao próprio item, informação no
  formato certo, confirmação); bloqueio de conclusão com obrigatórios pendentes **no banco**; concluir e
  voltar etapa (com motivo); passagem para pessoa ou fila; “Assumir” na fila (só uma pessoa assume;
  quem chegar depois vê “já assumida por…”); notificações; etapa de ganho (situação “Ganha — em
  finalização”, confirmação de seguradora, valor fechado e nº da proposta); conclusão na última etapa
  (“Concluída”); perda com motivo obrigatório em qualquer etapa; arquivar; histórico automático de
  toda mudança; interações.
- **Fora de escopo:** prazos e alertas de atraso (Fase 3); próxima ação obrigatória (Fase 3);
  validações e cálculos de campo (Fase 2); adiamento e descarte (Fase 3).
- **Entradas e pré-condições:** SPEC-1-004 aceita (oportunidades nascem com a cópia correta).
- **Saídas/artefatos:** regras no banco; ficha ajustada; seção “Movimento” no mapa do tronco.
- **Dependências e responsáveis:** Maestro; Louise testa com os usuários de teste da SPEC-1-002.
- **Risco e plano B:** duas pessoas da fila clicarem “Assumir” ao mesmo tempo. **Plano B:** assumir é
  uma operação única no banco; a segunda recebe a mensagem “já assumida”.
- **Rollback ou reversão:** “Voltar etapa” desfaz a conclusão sem perder o que foi preenchido; ganho e
  perda podem ser reabertos só por administrador, com motivo registrado no histórico.

## Fluxo e regras

1. **Item Documento:** o arquivo vai para a pasta privada ligado ao item; o item conta como preenchido quando há ao menos um arquivo.
2. **Item Informação:** o valor é validado pelo formato (número, valor em R$, data, sim/não, lista).
3. **Concluir etapa:** só o responsável da etapa atual (ou administrador/gestor da equipe) conclui; o banco confere os obrigatórios e recusa com a lista do que falta.
4. **Passagem:** próxima etapa vira atual; responsável = responsável comercial **ou** fila (equipe + função); notificação para o responsável ou para todos da fila.
5. **Assumir:** pessoa da fila assume; a etapa passa a ter responsável; os demais da fila veem que foi assumida.
6. **Voltar etapa:** motivo obrigatório; a etapa anterior volta a ser atual, com o mesmo responsável que a concluiu.
7. **Ganho:** concluir a etapa marcada como de ganho abre a confirmação (seguradora, valor fechado, nº da proposta, todos obrigatórios); situação → “Ganha — em finalização”; a oportunidade grava a **data do ganho**; as etapas seguintes continuam.
8. **Conclusão:** concluir a última etapa → “Concluída”. Se a última etapa vier antes de passar por uma etapa de ganho (fluxo sem etapa de ganho), pede a confirmação de ganho nesse momento.
9. **Perda:** em qualquer etapa, com motivo (lista de perda) e observação obrigatórios; situação → “Perdida”; a oportunidade grava a **data da perda**; nenhuma etapa fica atual.
10. **Histórico:** gravado pelo banco em toda mudança de etapa, responsável, situação, valor, item, documento e interação, com usuário e data/hora.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | J6 da etapa 1 à 12 | Passa pelas etapas; ganho na 11; na 12 cai na fila Saúde/Pós-venda; “Concluída” na 12 | — |
| Limite | Concluir a etapa 6 da J6 sem o cartão CNPJ | Recusado com a lista do que falta | Anexar e concluir |
| Limite | Dois usuários do pós-venda clicam “Assumir” juntos | Um assume; o outro vê “já assumida por …” | — |
| Falha | Perda sem motivo | Recusada | Escolher motivo |
| Regressão | Voltar etapa depois de concluir | Itens preenchidos continuam lá; histórico mostra o motivo | — |

## Checklist de execução

- [ ] Itens dos 3 tipos funcionando, com documento ligado ao item.
- [ ] Bloqueio de obrigatórios no banco.
- [ ] Concluir, voltar (com motivo) e assumir como operações únicas no banco.
- [ ] Notificações para pessoa e fila; sino com marcar como lida.
- [ ] Ganho na etapa de ganho, conclusão na última, perda com motivo.
- [ ] Data do ganho e data da perda gravadas na oportunidade (campos próprios), com as oportunidades já ganhas ou perdidas preenchidas a partir da linha do tempo.
- [ ] Histórico automático e linha do tempo completa.
- [ ] Interações registradas.

## Critérios de aceite

- [ ] **CA-1-19:** A etapa “Documentação para implantação” da J6 não conclui sem o cartão CNPJ (teste de aceite 6).
- [ ] **CA-1-20:** Ao concluir a etapa 11 (ganho) da J6, a etapa 12 cai na fila Saúde/Pós-venda com aviso no sino de quem é dessa fila; a pessoa assume e conclui (teste de aceite 8).
- [ ] **CA-1-21:** Voltar etapa exige motivo, e ele aparece na linha do tempo (teste de aceite 9).
- [ ] **CA-1-22:** Perda sem motivo não é aceita (teste de aceite 10); ganho exige seguradora, valor e nº da proposta; a oportunidade ganha mostra a data do ganho e a perdida, a data da perda.
- [ ] **CA-1-23:** Uma J3 e uma J6 chegam a “Concluída”, e a linha do tempo mostra cada passo com pessoa e horário.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | No tronco, a conclusão com obrigatório pendente é aceita por chamada direta, ou dois “Assumir” simultâneos passam | Tentar concluir a etapa 6 da J6 sem o cartão CNPJ, pela tela e por chamada direta; dois usuários assumem juntos | Falha esperada registrada | Mapa, seção Movimento |
| GREEN | Regras no banco | Repetir os testes + percorrer a J6 inteira | Recusas corretas; J6 concluída; fila e notificações certas | Prints |
| REFACTOR/REGRESSÃO | Voltar etapa, perda e histórico | Voltar a etapa 3 da J3; perder outra oportunidade; conferir a linha do tempo | Dados preservados; motivos visíveis | Prints |

**Dados/fixtures:** 1 J6 e 1 J3 de teste; usuários Colaborador Saúde (comercial), Colaborador Saúde/Pós-venda (2 pessoas, para o teste de assumir), Colaborador Seguros; 1 PDF de teste.
**Caminhos de erro obrigatórios:** obrigatório pendente; quem não é responsável tenta concluir; assumir duplicado; perda sem motivo; ganho sem nº da proposta.
**Evidência exigida:** prints + linha do tempo de uma J6 completa.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T5.01 | Itens do checklist (3 tipos, documento ligado ao item) e bloqueio de obrigatórios no banco | Maestro | SPEC-1-005 | CA-1-19 | RED → GREEN do teste da etapa 6 | ☐ |
| T5.02 | Concluir, voltar e assumir como operações no banco; passagem para pessoa ou fila; notificações e sino | Maestro | SPEC-1-005 | CA-1-20, CA-1-21 | GREEN da fila do pós-venda + teste do assumir duplicado | ☐ |
| T5.03 | Etapa de ganho, conclusão, perda com motivo e arquivar; data do ganho e data da perda | Maestro | SPEC-1-005 | CA-1-22 | GREEN de ganho e perda, com as datas gravadas | ☐ |
| T5.04 | Histórico automático, linha do tempo completa e interações | Maestro | SPEC-1-005 | CA-1-23 | REGRESSÃO: J3 e J6 até “Concluída” | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| 20/10/2026 | Auditoria do tronco | CA-1-20 | A etapa 10 da J6 passou para o responsável comercial (ajuste da N&G); a passagem para a fila do pós-venda é testada na etapa 12 |
| 20/10/2026 | Auditoria do tronco | T5.03, CA-1-22 | O tronco não grava data do ganho nem data da perda (só na linha do tempo); a SPEC-1-006 usa essas datas no Painel |
