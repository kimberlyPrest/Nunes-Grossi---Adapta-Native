# Fase atual — Fase 1: Primeira jornada viva

> **Período previsto:** 20/10/2026 a 30/10/2026 · **Aceite:** Viviane Grossi
> **SPECs:** `04_fase-atual/specs/00-INDICE.md` · **Prompts do tronco:** `03_documentos/prompts-tronco-fase-1.md` · **Fluxos:** `03_documentos/fluxos-piloto-fase-1.md`


**Resultado:** a N&G já usa o sistema numa oportunidade real. Abre uma oportunidade de **Saúde PME
3–29 vidas** (com passagem para o pós-venda) ou de **Seguro Empresarial — Porto**, o fluxo aparece
sozinho com as etapas e o checklist, cada pessoa trabalha a sua etapa, a oportunidade passa de mão
em mão e termina em ganho ou perda.

**Por que estas duas jornadas:** a J6 (Saúde PME) é a mais completa (checklist extenso, críticas da
operadora, passagem para o pós-venda) e mostra o maior ganho; a J3 (Empresarial) é curta e da
equipe de Seguros, então as duas equipes começam a usar ao mesmo tempo.

**O que vocês vão ver no fim da fase:**
1. Tela de login com a identidade da N&G; cada pessoa com o seu acesso.
2. Cadastro de um cliente pelo CNPJ, com contatos e origem (“indicação do parceiro X”).
3. Botão **“Nova oportunidade”**: escolhe cliente, produto e operadora, e as 12 etapas da J6
   aparecem com o checklist.
4. A comercial anexa documentos, marca os itens, conclui a etapa; quando chega na etapa de
   boas-vindas, a oportunidade **aparece sozinha na fila do pós-venda**.
5. Quadro (kanban) com as oportunidades por etapa e um painel simples com quantas estão em cada
   etapa e quantas foram ganhas e perdidas no mês.

## Telas

| Tela | O que mostra | O que dá para fazer |
|---|---|---|
| **Login** | Logo e cores da N&G | Entrar, recuperar senha |
| **Início / Minha fila** | Oportunidades em que eu sou o responsável da etapa atual, ordenadas pela data de entrada na etapa | Abrir a oportunidade; filtrar por produto |
| **Clientes (lista)** | Nome/razão social, CPF/CNPJ, segmento, nº de oportunidades abertas | Buscar por nome ou documento; cadastrar novo |
| **Cliente (ficha)** | Dados PF/PJ, contatos, origem e indicado por, oportunidades (abertas, ganhas, perdidas) | Editar dados; incluir contato; abrir nova oportunidade para este cliente |
| **Nova oportunidade** | Escolha de cliente (ou cadastro rápido), produto, seguradora/operadora, responsável comercial, origem, valor estimado, data prevista de fechamento | Criar; o sistema aplica o fluxo e mostra a ficha |
| **Oportunidade (ficha)** | Cabeçalho (protocolo, cliente, produto, seguradora, responsável, valor, situação); trilha das etapas (concluídas, atual, futuras); checklist da etapa atual; anexos; interações; linha do tempo com o histórico; campo “nº da proposta/apólice no Quiver” | Marcar itens; anexar arquivos; registrar interação (ligação, WhatsApp, e-mail, reunião, com resumo); concluir etapa; voltar etapa com motivo; marcar ganho ou perda |
| **Quadro (kanban)** | Colunas = etapas de um fluxo; cartões = oportunidades | Filtrar por fluxo, responsável e equipe; abrir cartão |
| **Lista de oportunidades** | Todas as que posso ver, com busca por cliente, CPF/CNPJ ou protocolo | Buscar, filtrar, exportar para planilha |
| **Painel inicial** | Oportunidades abertas por etapa e por responsável; ganhas e perdidas no mês; principais motivos de perda | Filtrar por equipe e período |
| **Usuários (admin)** | Pessoas, perfil, equipe(s), ativo/inativo | Convidar, mudar perfil e equipe, desativar |

Estados que toda tela deve tratar: **vazio** (“nenhuma oportunidade ainda — crie a primeira”),
**carregando** e **erro** (mensagem em português, sem termos técnicos).

## Dados e permissões

- Gavetas criadas: usuários e equipes, clientes, contatos, seguradoras, produtos, modelos de fluxo
  (com etapas e itens), oportunidades, etapas da oportunidade, itens de checklist, documentos,
  interações, motivos de perda, histórico.
- **Pré-carregados** (sem tela de edição ainda): seguradoras e operadoras dos vídeos (Porto Seguro,
  Porto Saúde, SulAmérica, Tokio Marine, Akad, Capemisa, Ana Costa, Santa Saúde), produtos piloto,
  motivos de perda iniciais, origens de lead, os fluxos **J6** e **J3** e um **fluxo genérico**.
  Mudanças nesses itens durante a Fase 1 são pedidas à consultoria e feitas por task; a partir da
  Fase 2, vocês mesmos editam.
- Permissões de `01_projeto/regras-e-permissoes.md` aplicadas **no banco** desde o primeiro dia: colaborador só vê o que é seu,
  o que participa e a fila da sua equipe; ninguém exclui definitivamente.
- Documentos ficam na pasta segura, visíveis só para quem pode ver a oportunidade.

## Regras e automações

- Ao criar a oportunidade, o sistema escolhe o fluxo pela combinação produto + seguradora; se não
  houver, usa o genérico (lead → qualificação → cotação → proposta → negociação → ganho/perda).
- Etapa só é concluída com os obrigatórios completos (RN-02); concluída a etapa, o responsável da
  próxima é definido pelo fluxo (pessoa, papel ou fila da equipe) e recebe um aviso no sino de
  notificações (RN-03).
- Passagem automática para o pós-venda na última etapa da J6 (F-07).
- Ganho pede seguradora, valor (prêmio ou mensalidade) e nº da proposta; perda pede motivo da lista
  e observação (RN-05).
- Cliente único por CPF/CNPJ, com validação dos dígitos (RN-04).
- Toda mudança é gravada no histórico (RN-07); a ficha mostra a linha do tempo.

## Sequência de construção

1. **Base:** identidade visual, login, perfis, equipes, tela de usuários e regras de acesso no banco.
2. **Clientes:** cadastro PF/PJ, contatos, origem, validação e bloqueio de duplicidade.
3. **Estrutura de fluxos:** gavetas de seguradoras, produtos, modelos de fluxo, etapas e itens;
   carga dos fluxos J6, J3 e genérico.
4. **Oportunidade:** criação com aplicação do fluxo; ficha com etapas, checklist, anexos, interações
   e histórico.
5. **Movimento:** concluir/voltar etapa, passagem de bastão, notificações internas, ganho e perda,
   passagem para o pós-venda.
6. **Visões:** minha fila, quadro, lista com busca, painel inicial.
7. **Ensaio real:** a Isabelle (Saúde) e a Sheyla (Empresarial) rodam uma oportunidade cada uma, de
   verdade ou de teste, de ponta a ponta.

## Testes de aceite (para leigos)

1. Entre como **colaborador** e tente abrir a tela de usuários: não deve aparecer.
2. Cadastre um cliente com um CNPJ inválido: o sistema deve recusar.
3. Cadastre de novo um cliente que já existe: o sistema deve avisar e mostrar o cadastro existente.
4. Abra uma oportunidade “Saúde PME 3–29 · SulAmérica”: as 12 etapas da J6 devem aparecer.
5. Abra uma oportunidade de um produto sem fluxo cadastrado: deve aparecer o fluxo genérico.
6. Tente concluir a etapa “Documentação para implantação” sem anexar o cartão CNPJ: o sistema não
   deve deixar.
7. Anexe um documento e entre com outro usuário, de outra equipe: ele não deve ver o documento.
8. Conclua a etapa 11 da J6: a oportunidade deve aparecer na fila do pós-venda, com aviso no sino.
9. Volte uma etapa: o sistema deve pedir o motivo, e ele deve aparecer na linha do tempo.
10. Marque uma oportunidade como perdida sem escolher motivo: não deve deixar.
11. Como colaborador, procure um botão de excluir oportunidade: não deve existir.
12. Abra o quadro: o cartão deve estar na coluna da etapa atual.
13. Abra o painel: os números de ganhas e perdidas do mês devem bater com o que vocês fizeram nos testes.

## Fora desta fase

Editar fluxos pela tela (Fase 2); prazos e alertas de atraso (Fase 3); validade de cotação (Fase 3);
painel completo e metas (Fase 3); e-mails (Fase 3); Quiver (Fase 4).

## Riscos desta fase

- Os fluxos J6 e J3 podem precisar de ajustes depois do primeiro uso: anotar tudo, pois a Fase 2 dá
  a vocês o controle pela tela.
- Primeira experiência com o ciclo Maestro → Skip → teste: a primeira task será feita junto com a
  consultoria.

## Dependências e decisões

D-01 (Vinícius), D-03 (visibilidade entre equipes), D-09 (revisão de J6 e J3), D-11 (quem faz a
documentação e a digitação em Saúde), D-12 (dados de saúde). Acessos Skip, Maestro e GitHub já
configurados em 05/10; projeto Supabase criado na conta da N&G.

## Definição de pronto

- [ ] Todos os 13 testes de aceite passaram.
- [ ] Uma oportunidade de J6 e uma de J3 percorreram o fluxo inteiro com pelo menos dois usuários.
- [ ] Equipes Seguros e Saúde/Odonto com os usuários reais cadastrados.
- [ ] Demonstração feita para a Viviane e aceite registrado.

---

## Instruções para o Maestro

Quando a Louise disser **“execute a próxima task”**:

1. Leia este arquivo e pegue a **primeira task com status ☐ cujo dono é “Maestro”** e cujas tasks
   anteriores da mesma leva estejam ☑. Tasks de outros donos (Louise, Isabelle, Sheyla, Viviane) são
   feitas por pessoas: se a próxima for de uma pessoa, avise quem precisa fazer o quê e pare.
2. Leia a SPEC da task **inteira**, esta página (resultado, telas e regras acima), `01_projeto/regras-e-permissoes.md` e o `docs/mapa-do-tronco.md` no
   repositório de código. **O mapa é a verdade sobre nomes de telas, tabelas e campos.** Não suponha
   nomes; o Skip criou o banco junto com as telas durante o tronco.
3. Antes de mudar qualquer coisa, **confira como está hoje** a parte do sistema que a task toca (telas,
   tabelas, regras de acesso) e rode o **RED** indicado no “Recorte da prova”.
4. Implemente no Skip **só o que a task pede**, reaproveitando o que o tronco criou. Mudanças no banco
   sempre por migração. Nunca exclua dados reais.
5. Rode o **GREEN** (e a regressão, quando indicada) e entregue à Louise uma lista curta de **testes em
   linguagem simples** (“entre como colaborador e tente…”), com o resultado esperado de cada um.
6. Atualize o `docs/mapa-do-tronco.md` com o que mudou.
7. Quando a Louise confirmar que os testes passaram, marque a task como ☑ aqui e na tabela “Tasks
   vinculadas” da SPEC.
8. Se algo travar, se a task pedir uma decisão que não está escrita ou se o pedido sair do que a SPEC
   diz: **pare**, registre o problema em `06_notas/debug/` (um arquivo por problema, com data) em linguagem simples e peça à Louise
   para avisar a Kimberly. Não invente regra de negócio.

Regras permanentes: tudo em português do Brasil; nenhuma exclusão definitiva; nenhum dado de cartão
nem conteúdo médico; segredos só no cofre do Supabase; permissões sempre no banco (SPEC-1-002).

---

## Tasks

Legenda de status: ☐ a fazer · ◐ em andamento · ☑ concluída · ⛔ bloqueada

### Leva 0 — Tronco (prompts no Skip)

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Prazo | Status |
|---|---|---|---|---|---|---|---|
| T0.00 | Passo 0: conectar o Supabase ao projeto | Louise | SPEC-1-001 | Supabase conectado | Configurações do Skip | 20/10 | ☑ |
| T0.01 | Prompt 1: login, estrutura e navegação | Louise | SPEC-1-001 | Conferência P1 | — | 20/10 | ☑ |
| T0.02 | Prompt 2: usuários, perfis, equipes e funções | Louise | SPEC-1-001 | Conferência P2 | — | 20/10 | ☑ |
| T0.03 | Prompt 3: clientes e contatos | Louise | SPEC-1-001 | Conferência P3 | — | 21/10 | ☑ |
| T0.04 | Prompt 4: catálogo | Louise | SPEC-1-001 | Conferência P4 | — | 21/10 | ☑ |
| T0.05 | Prompt 5: estrutura dos fluxos + genérico + Empresarial | Louise | SPEC-1-001 | Conferência P5 | — | 21/10 | ☑ |
| T0.06 | Prompt 6: fluxo Saúde PME 3–29 | Louise | SPEC-1-001 | Conferência P6 | — | 21/10 | ☑ |
| T0.07 | Prompt 7: nova oportunidade | Louise | SPEC-1-001 | Conferência P7 | — | 22/10 | ☑ |
| T0.08 | Prompt 8: ficha da oportunidade | Louise | SPEC-1-001 | Conferência P8 | — | 22/10 | ☑ |
| T0.09 | Prompt 9: movimento | Louise | SPEC-1-001 | Conferência P9 | — | 22/10 | ☑ |
| T0.10 | Prompt 10: visões e painel | Louise | SPEC-1-001 | Conferência P10 | — | 22/10 | ☑ |

### Leva 1 — Conferir o tronco

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Prazo | Status |
|---|---|---|---|---|---|---|---|
| T1.01 | Auditar o tronco (sem alterar nada); registro em `06_notas/2026-10-20-auditoria-do-tronco.md` | Maestro (conduzido pela Louise) | SPEC-1-001 | CA-1-04 | RED + consultas de conferência | 23/10 | ☑ |
| T1.02 | Corrigir as lacunas estruturais e escrever `docs/mapa-do-tronco.md` | Maestro | SPEC-1-001 | CA-1-01, 02, 03 | GREEN + REGRESSÃO | 23/10 | ☐ |
| T4.00 | Revisar J6 (Isabelle) e J3 (Sheyla) em Configurações → Fluxos | Isabelle, Sheyla | SPEC-1-004 | Anotações entregues | — | 23/10 | ☐ |

### Leva 2 — Galhos: segurança e clientes

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Prazo | Status |
|---|---|---|---|---|---|---|---|
| T2.01 | Tela de usuários completa + 4 usuários de teste | Maestro | SPEC-1-002 | CA-1-08 | GREEN: teste do inativo | 26/10 | ☐ |
| T2.02 | Regras de acesso em todas as tabelas e bloqueio de exclusões | Maestro | SPEC-1-002 | CA-1-06, 07, 09 | RED → GREEN | 26/10 | ☐ |
| T2.03 | Pasta de documentos privada com regra por oportunidade | Maestro | SPEC-1-002 | CA-1-06 (documentos) | GREEN | 26/10 | ☐ |
| T2.04 | Menus e rotas por perfil + tela “sem acesso” + regressão | Maestro | SPEC-1-002 | CA-1-05 | GREEN + REGRESSÃO | 26/10 | ☐ |
| T3.01 | Validação e unicidade de CPF/CNPJ (tela + banco) | Maestro | SPEC-1-003 | CA-1-10, 11 | RED → GREEN | 27/10 | ☐ |
| T3.02 | Ficha do cliente completa e arquivamento com regras | Maestro | SPEC-1-003 | CA-1-12, 13 | GREEN + REGRESSÃO | 27/10 | ☐ |

### Leva 3 — Galhos: fluxos e movimento

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Prazo | Status |
|---|---|---|---|---|---|---|---|
| T4.01 | Conferir e corrigir J6, J3 e genérico; completar a página Fluxos | Maestro | SPEC-1-004 | CA-1-18 | RED → GREEN | 27/10 | ☐ |
| T4.02 | Abertura de oportunidade como operação única no banco | Maestro | SPEC-1-004 | CA-1-14, 15, 16, 17 | GREEN + REGRESSÃO | 28/10 | ☐ |
| T5.01 | Itens do checklist e bloqueio de obrigatórios no banco | Maestro | SPEC-1-005 | CA-1-19 | RED → GREEN | 28/10 | ☐ |
| T5.02 | Concluir, voltar e assumir; passagem; notificações | Maestro | SPEC-1-005 | CA-1-20, 21 | GREEN | 28/10 | ☐ |
| T5.03 | Ganho, conclusão, perda com motivo e arquivar | Maestro | SPEC-1-005 | CA-1-22 | GREEN | 29/10 | ☐ |
| T5.04 | Histórico automático, linha do tempo e interações | Maestro | SPEC-1-005 | CA-1-23 | REGRESSÃO | 29/10 | ☐ |

### Leva 4 — Galhos: visões

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Prazo | Status |
|---|---|---|---|---|---|---|---|
| T6.01 | Minha fila e lista com busca, filtros e CSV | Maestro | SPEC-1-006 | CA-1-24, 26 | GREEN | 29/10 | ☐ |
| T6.02 | Quadro por modelo de fluxo | Maestro | SPEC-1-006 | CA-1-25 | GREEN | 29/10 | ☐ |
| T6.03 | Painel inicial com contas no banco | Maestro | SPEC-1-006 | CA-1-27 | GREEN + REGRESSÃO | 29/10 | ☐ |

### Leva 5 — Ensaio real e aceite

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Prazo | Status |
|---|---|---|---|---|---|---|---|
| T7.01 | Cadastrar usuários reais e arquivar os testes | Louise | SPEC-1-007 | Checklist 1–2 | REGRESSÃO | 30/10 | ☐ |
| T7.02 | Ensaio J6 com Isabelle e o pós-venda Saúde | Louise + Isabelle | SPEC-1-007 | CA-1-29 | GREEN J6 | 30/10 | ☐ |
| T7.03 | Ensaio J3 com Sheyla | Louise + Sheyla | SPEC-1-007 | CA-1-29 | GREEN J3 | 30/10 | ☐ |
| T7.04 | 13 testes de aceite com evidência | Louise | SPEC-1-007 | CA-1-28 | GREEN 13/13 | 30/10 | ☐ |
| T7.05 | D-18 + demonstração + aceite da Fase 1 | Viviane | SPEC-1-007 | CA-1-30 | Aceite registrado | 30/10 | ☐ |

---

## Decisões que destravam a fase

| ID | Decisão | Quem | Até | Usada em |
|---|---|---|---|---|
| D-01 | Papel do Vinícius | Viviane | 20/10 | T2.01 |
| D-03 | Visibilidade entre Seguros e Saúde | Viviane | 20/10 | T2.02 |
| D-09 | Revisão de J6 e J3 | Isabelle, Sheyla | 23/10 | T4.01 |
| D-11 | Quem faz documentação e digitação em Saúde | Viviane + Isabelle | 23/10 | T4.01 |
| D-12 | Dados de saúde: só “sim/não” e “entregue” | Viviane | 20/10 | Prompt 6, T4.01 |
| D-18 | Supabase Pro | Viviane | 30/10 | T7.05 |
