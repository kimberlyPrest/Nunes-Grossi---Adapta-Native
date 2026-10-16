# SPEC-1-007 — Ensaio real e aceite da Fase 1

**Fase:** 1 — Primeira jornada viva
**Status:** planejada
**Dono:** Louise (condução) · Isabelle e pós-venda Saúde (J6) · Sheyla (J3) · Viviane (aceite)
**Origem:** `04_fase-atual/fase.md` (sequência item 7, testes de aceite 1 a 13, definição de pronto); D-18 em `01_projeto/decisoes.md`
**Degrau da solução:** processo manual: validação humana com o sistema real, sem construção nova

## Resultado observável

A equipe da N&G usa a central numa **oportunidade de verdade** (ou num caso real recente reproduzido) de
Saúde PME e numa de Seguro Empresarial, do começo ao fim, com as pessoas reais. A Viviane vê a
demonstração e dá o aceite da Fase 1.

## Limites e dependências

- **Inclui:** cadastro dos usuários reais (perfis, equipes, funções); arquivamento das oportunidades de
  teste; ensaio J6 (comercial Saúde → pós-venda) e J3; os 13 testes de aceite da fase com evidência;
  decisão D-18 (Supabase Pro); demonstração e aceite.
- **Fora de escopo:** construção de funcionalidades novas (o que surgir vira anotação para a Fase 2 ou
  emenda, decidida pela Kimberly).
- **Entradas e pré-condições:** SPECs 1-001 a 1-006 aceitas.
- **Saídas/artefatos:** tabela de evidências preenchida (abaixo); lista de ajustes para a Fase 2; aceite registrado.
- **Dependências e responsáveis:** agenda de Isabelle, Sheyla e uma pessoa do pós-venda Saúde (cerca de 1h cada).
- **Risco e plano B:** não haver uma oportunidade real em andamento na semana. **Plano B:** reproduzir
  um caso real recente (ex.: o da Hauric, do vídeo), com dados verdadeiros do cliente ou mascarados.
- **Rollback ou reversão:** oportunidades do ensaio podem ficar (são reais) ou ser arquivadas.

## Fluxo e regras

1. Louise cadastra os usuários reais e arquiva as oportunidades de teste.
2. Isabelle abre a J6 e percorre até a etapa 9; o pós-venda Saúde assume a etapa 10; Isabelle conclui a 11 (ganho); o pós-venda conclui a 12.
3. Sheyla abre a J3 e percorre até “Apólice emitida”.
4. Louise roda os 13 testes de aceite da fase e preenche a tabela de evidências.
5. Ajustes percebidos no uso viram a lista “Para a Fase 2” (não são construídos agora).
6. Viviane decide a D-18 e assiste à demonstração (roteiro em `04_fase-atual/fase.md`).

## Checklist de execução

- [ ] Usuários reais cadastrados com perfil, equipes e função.
- [ ] Oportunidades de teste arquivadas.
- [ ] Ensaio J6 concluído com pelo menos 2 pessoas.
- [ ] Ensaio J3 concluído.
- [ ] 13 testes com evidência na tabela.
- [ ] Lista “Para a Fase 2” registrada.
- [ ] D-18 decidida.
- [ ] Demonstração feita e aceite registrado.

## Critérios de aceite

- [ ] **CA-1-28:** Os 13 testes de aceite da Fase 1 (`04_fase-atual/fase.md`) passaram, com evidência na tabela abaixo.
- [ ] **CA-1-29:** Uma J6 e uma J3 percorreram o fluxo inteiro com usuários reais, sendo a J6 com passagem para o pós-venda.
- [ ] **CA-1-30:** A Viviane registrou o aceite da Fase 1 no grupo do projeto (ou na Jornada de Execução).

## TDD da SPEC (roteiro de verificação)

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Antes do ensaio, a tabela de evidências está vazia e a J6/J3 reais não existem | — | 0 de 13 | Tabela abaixo |
| GREEN | Ensaio e testes | Passos 1 a 4 | 13 de 13; J6 e J3 concluídas | Tabela + prints |
| REFACTOR/REGRESSÃO | Depois de arquivar os testes, o painel e a fila continuam corretos | Abrir Minha fila e Painel com os usuários reais | Só as oportunidades reais aparecem | Prints |

**Evidência exigida:** tabela preenchida + prints + mensagem de aceite.

### Tabela de evidências dos 13 testes

| # | Teste (`04_fase-atual/fase.md`) | Passou? | Evidência (print/link) |
|---|---|---|---|
| 1 | Colaborador não vê a tela de usuários | | |
| 2 | CNPJ inválido é recusado | | |
| 3 | Cliente repetido avisa e mostra o existente | | |
| 4 | Saúde PME 3–29 · SulAmérica nasce com 12 etapas | | |
| 5 | Produto sem fluxo usa o genérico | | |
| 6 | Documentação para implantação não conclui sem o cartão CNPJ | | |
| 7 | Documento não é visível para outra equipe | | |
| 8 | Etapa 11 → fila do pós-venda com aviso no sino | | |
| 9 | Voltar etapa pede motivo e aparece na linha do tempo | | |
| 10 | Perda sem motivo não é aceita | | |
| 11 | Colaborador não tem botão de excluir | | |
| 12 | Quadro mostra o cartão na coluna certa | | |
| 13 | Painel bate com os testes | | |

> Observação para o teste 8: no fluxo J6 revisado, a primeira passagem para a fila do pós-venda acontece
> ao concluir a etapa 9 (a etapa 10 é do pós-venda). Se a revisão da Isabelle mudar a ordem, vale a
> etapa que entrega para o pós-venda.

### Para a Fase 2 (ajustes percebidos no uso)

| Quem sugeriu | Ajuste | Fluxo/tela |
|---|---|---|
| | | |

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T7.01 | Cadastrar os usuários reais e arquivar as oportunidades de teste | Louise | SPEC-1-007 | Checklist itens 1 e 2 | REGRESSÃO: só as reais aparecem | ☐ |
| T7.02 | Ensaio J6 com Isabelle e o pós-venda Saúde | Louise + Isabelle | SPEC-1-007 | CA-1-29 (J6) | GREEN J6 | ☐ |
| T7.03 | Ensaio J3 com Sheyla | Louise + Sheyla | SPEC-1-007 | CA-1-29 (J3) | GREEN J3 | ☐ |
| T7.04 | Rodar os 13 testes de aceite e preencher a tabela de evidências | Louise | SPEC-1-007 | CA-1-28 | GREEN 13/13 | ☐ |
| T7.05 | Decidir a D-18, assistir à demonstração e dar o aceite da Fase 1 | Viviane | SPEC-1-007 | CA-1-30 | Aceite registrado | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
