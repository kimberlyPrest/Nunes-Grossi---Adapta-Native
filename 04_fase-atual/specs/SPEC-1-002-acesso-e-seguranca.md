# SPEC-1-002 — Acesso, perfis e segurança no banco

**Fase:** 1 — Primeira jornada viva
**Status:** planejada
**Dono:** Maestro (implementação) · Louise (testes) · Viviane (decisões D-01 e D-03)
**Origem:** `01_projeto/regras-e-permissoes.md` (perfis e permissões, RN-06), `01_projeto/constituicao.md` (segurança)
**Degrau da solução:** recurso nativo da plataforma: regras de acesso por linha (RLS) e pasta privada do Supabase; nada de controle de acesso só pela tela

## Resultado observável

Cada pessoa vê e faz **apenas o que o perfil, a equipe e a função permitem**, e isso é garantido pelo
banco de dados, não só pelas telas. Mesmo que alguém tente acessar um endereço direto ou que uma tela
tenha um erro, um colaborador não vê oportunidades e documentos que não são dele. Ninguém consegue
excluir clientes ou oportunidades definitivamente.

## Limites e dependências

- **Inclui:** tela de usuários completa (convidar, editar, desativar/reativar, bloquear login de
  inativo); regras de acesso em **todas** as tabelas do mapa do tronco, para as 4 operações (ler,
  criar, alterar, excluir); pasta de documentos privada com regra por oportunidade e links temporários
  de download; menus e rotas por perfil.
- **Fora de escopo:** edição de catálogo e fluxos pela tela (Fase 2); tela de auditoria (Fase 3).
- **Entradas e pré-condições:** SPEC-1-001 aceita; `docs/mapa-do-tronco.md` com os nomes reais;
  respostas D-01 (Vinícius) e D-03 (visibilidade entre equipes). Sem resposta da D-03, aplicar a
  proposta da consultoria (ver Regras).
- **Saídas/artefatos:** migrações com as regras de acesso; seção “Matriz de acesso” em
  `docs/mapa-do-tronco.md`; 4 usuários de teste (um por perfil e um de outra equipe).
- **Dependências e responsáveis:** Maestro implementa; Louise roda os testes com os usuários de teste.
- **Risco e plano B:** uma regra muito restrita pode esconder dados do próprio responsável.
  **Plano B:** os testes de regressão abaixo cobrem o responsável, a fila e o gestor antes de aceitar.
- **Rollback ou reversão:** cada regra numa migração própria, reversível.

## Fluxo e regras

**Quem vê o quê (oportunidades e tudo o que pende delas: etapas, itens, documentos, interações, histórico):**

| Perfil | Ler | Criar | Alterar | Excluir |
|---|---|---|---|---|
| Administrador | Todas | Sim | Todas | **Não** (só arquivar) |
| Gestor | As das suas equipes | Sim | As das suas equipes | **Não** (só arquivar) |
| Colaborador | Em que é responsável comercial, ou foi/é responsável de alguma etapa, ou cuja etapa atual está na fila da sua equipe + função | Sim | Só a etapa atual em que é o responsável (checklist, conclusão), interações e dados básicos das suas | **Não** |

- **Clientes e contatos:** todos os usuários ativos leem e criam; alteram administradores, gestores e o
  responsável comercial do cliente; ninguém exclui (arquivar: administradores e gestores).
- **Catálogo e modelos de fluxo:** todos os usuários ativos leem; só administradores alteram (por
  enquanto, só via task); ninguém exclui.
- **Usuários, perfis, equipes:** cada pessoa lê o próprio perfil e a lista básica de nomes (para escolher
  responsável); só administradores criam e alteram; ninguém muda o próprio perfil.
- **Notificações:** cada pessoa lê e marca como lidas só as suas.
- **Histórico:** ninguém altera nem exclui; é gravado pelo próprio sistema.
- **Documentos (pasta privada):** ler e enviar só quem pode ler a oportunidade; download por link
  temporário (válido por minutos); ninguém exclui pela tela.
- **D-03 (proposta, se não houver resposta):** colaborador de uma equipe que abre a ficha de um cliente
  vê que existem oportunidades de outra equipe (produto, situação, responsável), mas não abre a ficha
  nem os documentos.
- **Usuário inativo:** não entra e não aparece como opção de responsável.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Colaborador da equipe Saúde abre a lista | Vê só as suas, as em que foi responsável e as da fila Saúde/Comercial | — |
| Limite | Colaborador Seguros acessa pelo endereço direto uma oportunidade de Saúde | Mensagem “Você não tem acesso a esta oportunidade” | Nada é exibido; tentativa não quebra a tela |
| Limite | Fila Saúde/Pós-venda recebe a etapa 12 da J6 | Pessoas do pós-venda Saúde passam a ver e podem assumir | — |
| Falha | Alguém tenta excluir uma oportunidade pela API | Operação recusada pelo banco | Mensagem de erro amigável |

## Checklist de execução

- [ ] Matriz de acesso escrita no mapa com os nomes reais das tabelas.
- [ ] Regras de acesso ativas em todas as tabelas, para as 4 operações.
- [ ] Exclusão bloqueada em clientes, oportunidades e tabelas dependentes.
- [ ] Pasta de documentos privada, com regra por oportunidade e link temporário.
- [ ] Tela de usuários completa; login de inativo bloqueado.
- [ ] Menu e rotas: Usuários e Configurações só para administradores.
- [ ] Usuários de teste criados: Admin, Gestor Seguros, Colaborador Seguros, Colaborador Saúde/Pós-venda.

## Critérios de aceite

- [ ] **CA-1-05:** Entrando como colaborador, as telas Usuários e Configurações não aparecem no menu e, acessadas pelo endereço direto, mostram “sem acesso”.
- [ ] **CA-1-06:** Um colaborador de Seguros não consegue abrir, nem pelo endereço direto, uma oportunidade de Saúde em que não participa, nem baixar os documentos dela.
- [ ] **CA-1-07:** Não existe forma (tela ou chamada direta) de excluir cliente ou oportunidade; arquivar só aparece para administrador e gestor.
- [ ] **CA-1-08:** Usuário desativado não consegue entrar e some da lista de responsáveis.
- [ ] **CA-1-09:** Todas as tabelas do mapa têm regras de acesso ativas (conferido pela consulta de verificação).

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Com o tronco recém-gerado, o colaborador de Seguros consegue ler oportunidades de Saúde (ou alguma tabela está sem regra) | Consulta “tabelas sem regra de acesso” + teste com o usuário Colaborador Seguros | Falha esperada registrada | Mapa, seção Matriz de acesso |
| GREEN | Regras aplicadas | Repetir a consulta e os testes CA-1-05 a CA-1-08 com os 4 usuários de teste | Zero tabelas sem regra; os 4 testes passam | Prints dos testes |
| REFACTOR/REGRESSÃO | O responsável e a fila continuam vendo o que devem | Colaborador Saúde/Pós-venda vê a etapa 12 da J6 na fila e assume; o gestor vê as da equipe | Passa | Prints |

**Dados/fixtures:** os 4 usuários de teste; 1 oportunidade J3 (Seguros) e 1 J6 (Saúde) com um documento anexado em cada.
**Caminhos de erro obrigatórios:** acesso por endereço direto; tentativa de exclusão; usuário inativo; download por link vencido.
**Evidência exigida:** matriz de acesso no mapa + prints dos testes.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T2.01 | Completar a tela de usuários (convidar, editar, desativar/reativar, bloqueio de inativo) e criar os 4 usuários de teste | Maestro | SPEC-1-002 | CA-1-08 | GREEN: teste do inativo | ☐ |
| T2.02 | Aplicar as regras de acesso por perfil, equipe e função em todas as tabelas e bloquear exclusões | Maestro | SPEC-1-002 | CA-1-06, CA-1-07, CA-1-09 | RED → GREEN: consulta de verificação + teste do colaborador Seguros | ☐ |
| T2.03 | Tornar a pasta de documentos privada, com regra por oportunidade e link temporário | Maestro | SPEC-1-002 | CA-1-06 (parte documentos) | GREEN: download negado para o colaborador de outra equipe | ☐ |
| T2.04 | Ajustar menus e rotas por perfil, com tela de “sem acesso”, e rodar a regressão | Maestro | SPEC-1-002 | CA-1-05 | GREEN + REGRESSÃO | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
