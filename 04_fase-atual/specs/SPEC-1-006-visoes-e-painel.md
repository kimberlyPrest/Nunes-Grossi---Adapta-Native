# SPEC-1-006 — Minha fila, quadro, lista e painel inicial

**Fase:** 1 — Primeira jornada viva
**Status:** planejada
**Dono:** Maestro (implementação) · Louise (testes) · Viviane (validação do painel)
**Origem:** `04_fase-atual/fase.md` (telas Minha fila, Quadro, Lista, Painel inicial)
**Degrau da solução:** reuso do que o tronco gerou (Prompt 10), com contas feitas no banco para os números baterem

## Resultado observável

Cada pessoa abre o sistema e vê **o que é dela e da sua fila**, a gestão enxerga as oportunidades no
quadro por etapa, encontra qualquer oportunidade pelo protocolo, CNPJ ou nome, e o painel mostra números
que **batem** com a realidade.

## Limites e dependências

- **Inclui:** Minha fila (“Comigo” e “Fila da minha equipe”, com Assumir, dias na etapa, ordenação e
  filtro); lista de oportunidades com busca, filtros e exportação CSV (respeitando o que a pessoa pode
  ver); quadro por modelo de fluxo; painel inicial (em andamento por etapa e por responsável, ganhas e
  perdidas no mês em quantidade e valor, principais motivos de perda, filtros por equipe e período),
  com clique no número abrindo a lista correspondente; estados vazio, carregando e erro.
- **Fora de escopo:** conversão, tempo médio por etapa, previsão, metas e alertas (Fase 3); arrastar
  cartões no quadro para mudar de etapa (a etapa só muda pela conclusão, com checklist).
- **Entradas e pré-condições:** SPEC-1-005 aceita.
- **Saídas/artefatos:** telas ajustadas; consultas do painel documentadas no mapa do tronco (seção “Painel”).
- **Dependências e responsáveis:** Maestro; Louise testa; Viviane confere se o painel responde às perguntas dela.
- **Risco e plano B:** lentidão com muitos registros. **Plano B:** paginação na lista e contas do painel
  feitas no banco; isso já é o padrão desta SPEC.
- **Rollback ou reversão:** só leitura; sem risco para os dados.

## Fluxo e regras

1. “Comigo” = etapa atual com responsável = eu. “Fila da minha equipe” = etapa atual sem responsável, na fila de uma das minhas equipes com a minha função.
2. “Dias na etapa” = hoje − data de entrada na etapa atual.
3. A lista e o CSV trazem só o que a pessoa pode ver (regras da SPEC-1-002), com as colunas protocolo, cliente, documento, produto, seguradora, situação, etapa atual, responsável, valor estimado, data de criação e data prevista.
4. Quadro: cartões só das oportunidades em andamento ou “Ganha — em finalização” daquele modelo; não dá para arrastar.
5. Painel: “ganhas no mês” = oportunidades que entraram em “Ganha” no mês (por data do ganho); valor = valor fechado. “Perdidas no mês” = por data da perda; valor = valor estimado.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Colaborador Saúde com 2 oportunidades e 1 na fila | “Comigo” mostra 2; “Fila” mostra 1, com Assumir | — |
| Limite | Busca por CNPJ sem pontuação | Encontra | — |
| Limite | Usuário sem nenhuma oportunidade | Estados vazios com mensagem amigável | — |
| Falha | Erro ao carregar o painel | Mensagem em português e botão “tentar de novo” | — |

## Checklist de execução

- [ ] Minha fila com as duas seções, Assumir, dias na etapa, ordenação e filtro.
- [ ] Lista com busca (protocolo, cliente, CPF/CNPJ com ou sem pontuação), filtros, paginação e CSV.
- [ ] Quadro por modelo, sem arrastar, com filtros.
- [ ] Painel com as contas no banco e clique abrindo a lista.
- [ ] Estados vazio, carregando e erro em todas as visões.

## Critérios de aceite

- [ ] **CA-1-24:** A Minha fila de cada usuário de teste mostra exatamente as oportunidades esperadas nas duas seções.
- [ ] **CA-1-25:** No quadro do fluxo J3, cada cartão está na coluna da etapa atual (teste de aceite 12).
- [ ] **CA-1-26:** Buscar pelo protocolo e pelo CNPJ sem pontuação encontra a oportunidade; o CSV de um colaborador não traz oportunidades que ele não pode ver.
- [ ] **CA-1-27:** Os números de ganhas e perdidas do mês no painel batem com os testes feitos, e clicar no número abre a lista exata (teste de aceite 13).

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Planilha de conferência montada à mão com o que cada usuário deveria ver e os números do mês | Comparar com as telas do tronco | Divergências listadas | Mapa, seção Painel |
| GREEN | Telas e painel ajustados | Repetir a comparação com os 4 usuários de teste | Tudo confere | Prints |
| REFACTOR/REGRESSÃO | Ganhar e perder uma oportunidade nova | Atualizar a página do painel | Números mudam corretamente | Prints antes/depois |

**Dados/fixtures:** as oportunidades de teste das SPECs 1-004 e 1-005 (no mínimo 1 ganha, 1 perdida, 2 em andamento, 1 na fila).
**Caminhos de erro obrigatórios:** lista vazia; busca sem resultado; falha de carregamento; colaborador exportando CSV.
**Evidência exigida:** planilha de conferência + prints.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T6.01 | Minha fila (duas seções, Assumir, dias na etapa) e lista com busca, filtros, paginação e CSV | Maestro | SPEC-1-006 | CA-1-24, CA-1-26 | GREEN com os 4 usuários | ☐ |
| T6.02 | Quadro por modelo de fluxo (sem arrastar, com filtros) | Maestro | SPEC-1-006 | CA-1-25 | GREEN do quadro J3 | ☐ |
| T6.03 | Painel inicial com contas no banco e clique abrindo a lista | Maestro | SPEC-1-006 | CA-1-27 | GREEN + REGRESSÃO | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
