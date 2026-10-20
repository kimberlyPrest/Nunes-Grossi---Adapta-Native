# SPEC-1-001 — Tronco criado, auditado e mapeado

**Fase:** 1 — Primeira jornada viva
**Status:** em andamento (auditoria concluída em 20/10/2026; falta a T1.02)
**Dono:** Louise (prompts, condução da auditoria no Maestro e testes) · Maestro (auditoria e correções)
**Origem:** `04_fase-atual/fase.md` (sequência de construção, itens 1 a 6) e `01_projeto/constituicao.md`
**Degrau da solução:** nativo da plataforma: o Skip gera telas e banco a partir dos prompts do tronco; o Maestro só confere e completa o que faltou

## Resultado observável

O esqueleto completo da Fase 1 existe e funciona de forma básica: login, menu, usuários, clientes,
catálogo, os 3 modelos de fluxo, abertura de oportunidade, ficha, movimento de etapas e as páginas de
visão. Além disso, existe um **mapa do tronco** (`docs/mapa-do-tronco.md` no repositório de código) que
diz exatamente quais telas, tabelas, campos e ligações o Skip criou. Esse mapa é a base que todas as
SPECs seguintes usam.

## Limites e dependências

- **Inclui:** Passo 0 (conexão do Supabase), envio dos Prompts 1 a 10 (`03_documentos/prompts-tronco-fase-1.md`),
  auditoria do que foi gerado, correção das divergências **estruturais** (tabela, campo, ligação ou dado
  inicial faltando ou errado) e o mapa do tronco.
- **Fora de escopo:** regras de acesso no banco (SPEC-1-002), validação de CPF/CNPJ (SPEC-1-003),
  conferência fina dos fluxos (SPEC-1-004), regras do movimento (SPEC-1-005) e ajustes visuais (SPECs 1-003
  a 1-006). Nesta SPEC o Maestro **não redesenha telas**.
- **Entradas e pré-condições:** projeto Skip “login corretora” (05/10); Skip e Maestro conectados ao
  GitHub; projeto Supabase criado na conta da N&G, região São Paulo; plugin dos 7 agentes instalado no Maestro.
- **Saídas/artefatos:** sistema com o tronco; `docs/mapa-do-tronco.md`; tabela “Observações do tronco”
  preenchida no arquivo de prompts.
- **Dependências e responsáveis:** Louise envia os prompts e conduz a auditoria (T1.01 e T1.02) sozinha no Maestro.
- **Risco e plano B:** o Skip gerar nomes ou estruturas diferentes do esperado. **Plano B:** o mapa
  registra os nomes reais e as SPECs seguintes passam a usá-los. Só se renomeia o que estiver
  realmente errado (ex.: a mesma informação em duas tabelas).
- **Rollback ou reversão:** o histórico do GitHub permite voltar a versão do código; no banco, toda
  correção é feita por migração registrada no repositório.

## Modelo de referência do tronco

O que se **espera** encontrar depois dos 10 prompts. Os nomes reais podem ser diferentes; o que importa
é que a informação exista, no lugar certo e ligada corretamente.

| Informação (gaveta) | Campos essenciais | Ligações | Prompt |
|---|---|---|---|
| Perfis de usuário | nome, e-mail, telefone, perfil (admin/gestor/colaborador), função (comercial/pós-venda/gestão), ativo | usuário de login | 2 |
| Equipes | nome (Seguros; Saúde e Odonto) | — | 2 |
| Usuário × equipe | — | usuário ↔ equipe (muitos para muitos) | 2 |
| Clientes | tipo PF/PJ, documento (único), nome/razão social, nome fantasia, segmento, porte, vidas, nascimento, cidade, UF, origem, indicado por (tipo + nome), responsável comercial, observações, arquivado | origem; responsável → usuário | 3 |
| Contatos | nome, cargo/área, telefone, e-mail, principal | → cliente | 3 |
| Seguradoras | nome, frente, canal de cotação, ativa | — | 4 |
| Produtos | nome, frente, equipe, ativo | → equipe | 4 |
| Origens | nome, ativa | — | 4 |
| Motivos | descrição, tipo (perda/descarte/adiamento), ativo | — | 4 |
| Modelos de fluxo | nome, produto, seguradora (opcional), genérico, versão, ativo | → produto; → seguradora | 5 |
| Etapas do modelo | ordem, nome, descrição, tipo, tipo de responsável (comercial da oportunidade / fila), equipe e função da fila, prazo em dias, etapa de ganho | → modelo | 5 |
| Itens do modelo | ordem, título, tipo (documento/informação/confirmação), formato, opções, obrigatório | → etapa do modelo | 5 |
| Oportunidades | protocolo (único), situação, valor estimado, data prevista, observações, valor fechado, nº proposta Quiver, motivo de perda, observação da perda, versão do modelo | → cliente, produto, seguradora, modelo, responsável comercial, equipe, origem, etapa atual | 7 |
| Etapas da oportunidade (cópia) | ordem, nome, tipo, etapa de ganho, situação (futura/atual/concluída), responsável, fila (equipe + função), entrou em, concluída em, concluída por | → oportunidade | 7 |
| Itens da oportunidade (cópia) | título, tipo, formato, obrigatório, valor preenchido, marcado, preenchido por, em | → etapa da oportunidade | 7 |
| Documentos | nome do arquivo, caminho na pasta privada, enviado por, em | → oportunidade; → item | 8 |
| Interações | tipo, data, resumo, usuário | → oportunidade | 8 |
| Histórico | ação, de, para, usuário, em | → oportunidade (e cliente) | 8 |
| Notificações | mensagem, lida, em | → usuário; → oportunidade | 9 |
| Pasta de arquivos (storage) | privada | — | 8 |

Dados iniciais esperados: 2 equipes; 8 seguradoras; 14 produtos; 8 origens; 9 motivos de perda;
3 modelos de fluxo (genérico com 5 etapas, Empresarial Porto com 7, Saúde PME 3–29 com 12).

## Fluxo e regras

1. Louise faz o Passo 0 e envia os Prompts 1 a 10, um por vez, fazendo a conferência rápida de cada um.
2. O Maestro (T1.01) lê o código no GitHub e o banco conectado e compara com o modelo de referência,
   **sem alterar nada**.
3. O Maestro escreve `docs/mapa-do-tronco.md` com: telas e rotas existentes; tabelas reais com colunas,
   tipos e ligações; dados iniciais encontrados; e uma lista de **lacunas** classificadas em
   *estrutural* (corrigir agora), *regra/segurança* (SPEC indicada) e *visual* (SPEC indicada).
4. A Louise lê a lista de lacunas que o Maestro entregou e confirma as estruturais (as de regra/segurança e visuais ficam para as SPECs indicadas).
5. O Maestro (T1.02) corrige só as lacunas estruturais, por migração, e atualiza o mapa.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Tronco gerado conforme os prompts | Mapa sem lacunas estruturais, ou com lacunas corrigidas na T1.02 | — |
| Limite | Skip juntou duas gavetas numa só (ex.: etapas do modelo e da oportunidade na mesma tabela) | Lacuna estrutural registrada; separar só se impedir a regra de cópia (Prompt 7, item 3) | O Maestro aplica essa regra na T1.02 e registra a decisão no mapa |
| Falha | Banco criado no lugar errado (nativo do Skip em vez do Supabase) | Auditoria para, registra em `06_notas/debug/` e não segue | Louise reconecta o Supabase no Skip e o Maestro refaz a T1.01 |

## Checklist de execução

- [x] Passo 0 concluído: Supabase conectado ao projeto Skip.
- [x] Prompts 1 a 10 enviados, cada um com a conferência rápida feita.
- [x] Observações do tronco registradas no arquivo de prompts.
- [ ] `docs/mapa-do-tronco.md` criado no repositório de código.
- [x] Lacunas estruturais confirmadas (`06_notas/2026-10-20-auditoria-do-tronco.md`).
- [ ] Lacunas estruturais corrigidas e mapa atualizado.

## Critérios de aceite

- [ ] **CA-1-01:** Todas as gavetas do modelo de referência existem no banco (com nome real registrado no mapa) e as ligações listadas estão criadas.
- [ ] **CA-1-02:** Os dados iniciais conferem: 2 equipes, 8 seguradoras, 14 produtos, 8 origens, 9 motivos de perda e 3 modelos de fluxo com 5, 7 e 12 etapas.
- [ ] **CA-1-03:** As 10 páginas do tronco abrem sem erro (Minha fila, Oportunidades, Quadro, Clientes, ficha do cliente, Nova oportunidade, ficha da oportunidade, Painel, Usuários, Configurações → Catálogo/Fluxos).
- [ ] **CA-1-04:** `docs/mapa-do-tronco.md` existe, lista telas, tabelas reais, colunas e lacunas, e nenhuma lacuna estrutural continua aberta.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Antes do tronco, as consultas de conferência não encontram as gavetas | Rodar as consultas de contagem (abaixo) no banco antes do Prompt 1 | Tabelas inexistentes / contagem zero | Print ou saída anotada no mapa |
| GREEN | Depois do tronco + T1.02, as consultas retornam o esperado | Mesmas consultas | Contagens do CA-1-02; todas as tabelas presentes | Seção “Conferência” do mapa |
| REFACTOR/REGRESSÃO | As correções da T1.02 não quebraram as telas | Abrir as 10 páginas do CA-1-03 e criar 1 oportunidade de cada modelo | Todas abrem; as 3 oportunidades nascem com 5, 7 e 12 etapas | Prints anexados no mapa |

**Dados/fixtures:** catálogo e modelos dos Prompts 4 a 6; 1 cliente PJ de teste.
**Consultas de conferência:** contagem de linhas por tabela de catálogo; contagem de etapas por modelo;
lista de tabelas e colunas do esquema público; lista de pastas de arquivos.
**Caminhos de erro obrigatórios:** gaveta ausente; ligação ausente; dado inicial faltando; banco errado.
**Evidência exigida:** `docs/mapa-do-tronco.md` com a seção de conferência e os prints.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T0.00 | Passo 0 — conectar o Supabase ao projeto Skip | Louise | SPEC-1-001 | Checklist item 1 | Supabase aparece conectado nas configurações do Skip | ☑ |
| T0.01 | Prompt 1 — Estrutura e navegação | Louise | SPEC-1-001 | Conferência rápida P1 | — | ☑ |
| T0.02 | Prompt 2 — Usuários, perfis, equipes e funções | Louise | SPEC-1-001 | Conferência rápida P2 | — | ☑ |
| T0.03 | Prompt 3 — Clientes e contatos | Louise | SPEC-1-001 | Conferência rápida P3 | — | ☑ |
| T0.04 | Prompt 4 — Catálogo | Louise | SPEC-1-001 | Conferência rápida P4 | — | ☑ |
| T0.05 | Prompt 5 — Estrutura dos fluxos + genérico + Empresarial | Louise | SPEC-1-001 | Conferência rápida P5 | — | ☑ |
| T0.06 | Prompt 6 — Fluxo Saúde PME 3–29 | Louise | SPEC-1-001 | Conferência rápida P6 | — | ☑ |
| T0.07 | Prompt 7 — Nova oportunidade | Louise | SPEC-1-001 | Conferência rápida P7 | — | ☑ |
| T0.08 | Prompt 8 — Ficha da oportunidade | Louise | SPEC-1-001 | Conferência rápida P8 | — | ☑ |
| T0.09 | Prompt 9 — Movimento | Louise | SPEC-1-001 | Conferência rápida P9 | — | ☑ |
| T0.10 | Prompt 10 — Visões e painel | Louise | SPEC-1-001 | Conferência rápida P10 | — | ☑ |
| T1.01 | Auditar o tronco e escrever o mapa (sem alterar nada) | Maestro (conduzido pela Louise) | SPEC-1-001 | CA-1-04 (mapa e lacunas) | RED + GREEN parcial: consultas de conferência registradas | ☑ |
| T1.02 | Corrigir as lacunas estruturais e atualizar o mapa | Maestro | SPEC-1-001 | CA-1-01, CA-1-02, CA-1-03 | GREEN + REGRESSÃO | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| 20/10/2026 | Auditoria do tronco | — | Etapa 10 do Saúde PME ajustada pela N&G durante o tronco (responsável comercial, 3 itens condicionais); aceita como está |
