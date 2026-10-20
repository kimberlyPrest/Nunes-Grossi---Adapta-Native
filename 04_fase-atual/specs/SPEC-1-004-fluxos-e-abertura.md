# SPEC-1-004 — Fluxos corretos e abertura de oportunidade

**Fase:** 1 — Primeira jornada viva
**Status:** planejada
**Dono:** Maestro (implementação) · Isabelle (revisão da J6) · Sheyla (revisão da J3) · Louise (testes)
**Origem:** `03_documentos/fluxos-piloto-fase-1.md` (J3 e J6); `04_fase-atual/fase.md`; `01_projeto/regras-e-permissoes.md` (RN-01)
**Degrau da solução:** reuso do que o tronco gerou (Prompts 5 a 7), com a regra de escolha do fluxo e a cópia feitas no banco, numa única operação

## Resultado observável

Ao abrir uma oportunidade, **o fluxo certo aparece sempre**, com as etapas, os itens e os responsáveis
exatamente como a N&G revisou, e a oportunidade guarda a sua própria cópia. Se o modelo mudar depois,
as oportunidades abertas continuam iguais.

## Limites e dependências

- **Inclui:** conferência item a item dos modelos J6, J3 e genérico contra `03_documentos/fluxos-piloto-fase-1.md`; a
  revisão de Isabelle e Sheyla (D-09), aplicada no fim da fase (T4.00 e T4.03); a regra de escolha do modelo (produto + seguradora → produto →
  genérico) com o aviso; a cópia de etapas e itens com a versão do modelo; o protocolo sequencial
  único; a primeira etapa atual com o responsável certo; tudo numa operação só (ou cria tudo, ou não
  cria nada); a página Configurações → Fluxos (somente leitura) mostrando etapas, tipos, responsáveis,
  etapa de ganho e itens obrigatórios.
- **Fora de escopo:** criar e editar fluxos, etapas e itens pela tela, versões e duplicação (Fase 2); editar o
  catálogo pela tela (Fase 2). Nesta fase as páginas Fluxos e Catálogo são **só de consulta, de propósito**:
  não ganham botões de incluir ou editar. A Fase 2 entrega tudo configurável pela interface, sem depender
  de código; validações e cálculos
  automáticos (Fase 2); prazos com alerta (Fase 3).
- **Entradas e pré-condições:** SPEC-1-001 e SPEC-1-002 aceitas; resposta da
  D-11 (quem faz a documentação e a digitação em Saúde). Sem resposta da D-11, mantém-se o responsável
  comercial nas etapas 6 a 9 da J6. A D-09 (revisão dos fluxos) só é necessária para a T4.03, no fim da fase.
- **Saídas/artefatos:** modelos conferidos; regra de abertura no banco; seção “Fluxos” no mapa do tronco.
- **Dependências e responsáveis:** Isabelle e Sheyla devolvem a revisão (prints da página Fluxos com
  anotações bastam); o Maestro aplica.
- **Risco e plano B:** a revisão pedir muitas mudanças. **Plano B:** o que for estrutural (nova etapa,
  outro responsável) entra agora; o fino (texto de item) pode esperar o construtor da Fase 2.
- **Rollback ou reversão:** alterações nos modelos por migração; oportunidades de teste podem ser arquivadas.

## Fluxo e regras

1. Ao salvar a Nova oportunidade, o banco, numa operação única:
   a) escolhe o modelo ativo: produto + seguradora; senão, produto sem seguradora; senão, genérico;
   b) gera o protocolo NG-AAAA-NNNNNN, sequencial por ano e sem repetição, mesmo com duas pessoas salvando ao mesmo tempo;
   c) copia etapas e itens, guardando a versão do modelo;
   d) marca a etapa 1 como atual e define o responsável: “comercial da oportunidade” → o responsável comercial; “fila” → equipe + função, sem pessoa;
   e) define a equipe da oportunidade pela equipe do produto;
   f) grava “oportunidade criada” no histórico e notifica o responsável da etapa 1, se não for quem criou.
2. Se o genérico foi usado, a ficha mostra o aviso e os administradores recebem notificação: “Produto X sem fluxo próprio”.
3. Seguradora inativa ou de outra frente não aparece para o produto escolhido.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Saúde PME 3–29 + SulAmérica | Fluxo J6, 12 etapas, etapa 1 com o responsável comercial | — |
| Principal | Seguro Empresarial + Porto Seguro | Fluxo J3, 7 etapas | — |
| Limite | Seguro Empresarial + Tokio Marine (sem modelo próprio, sem modelo “sem seguradora”) | Fluxo genérico + aviso | — |
| Falha | Erro no meio da cópia | Nada é criado; mensagem “Não foi possível criar a oportunidade, tente de novo” | Operação desfeita por inteiro |
| Regressão | Modelo J3 alterado depois de abrir uma oportunidade | A oportunidade antiga mantém as etapas e os itens originais | — |

## Checklist de execução

- [ ] J6, J3 e genérico conferidos com `03_documentos/fluxos-piloto-fase-1.md` (nº de etapas, tipos, responsáveis, etapa de ganho, itens e obrigatórios).
- [ ] Revisões de Isabelle (J6) e Sheyla (J3) aplicadas e registradas nas Emendas.
- [ ] Regra de escolha do modelo no banco.
- [ ] Protocolo sequencial e único.
- [ ] Cópia com versão, numa operação única.
- [ ] Responsável da etapa 1 e equipe da oportunidade corretos.
- [ ] Aviso do genérico e notificação aos administradores.
- [ ] Página Configurações → Fluxos mostrando tudo para conferência.

## Critérios de aceite

- [ ] **CA-1-14:** Oportunidade Saúde PME 3–29 + SulAmérica nasce com as 12 etapas da J6, e a etapa 1 é atual com o responsável comercial (teste de aceite 4).
- [ ] **CA-1-15:** Produto sem fluxo próprio usa o genérico, com aviso na ficha (teste de aceite 5).
- [ ] **CA-1-16:** Protocolos não se repetem: 10 oportunidades criadas em sequência rápida têm 10 protocolos distintos e em ordem.
- [ ] **CA-1-17:** Alterar o modelo J3 (via task de teste) não muda uma oportunidade J3 aberta antes da alteração.
- [ ] **CA-1-18:** A página Fluxos mostra para J6 e J3 exatamente o que Isabelle e Sheyla aprovaram.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Lista de divergências entre os modelos do tronco e os fluxos piloto / revisão | Comparar a página Fluxos com os fluxos piloto e as anotações | Divergências listadas (ou “nenhuma”) | Mapa, seção Fluxos |
| GREEN | Abertura correta nos 3 casos | Criar J6+SulAmérica, J3+Porto, Empresarial+Tokio | 12, 7 e 5 etapas; genérico com aviso | Prints |
| REFACTOR/REGRESSÃO | Cópia isolada e protocolo único | Alterar um item do J3 numa migração de teste e abrir a oportunidade antiga; criar 10 seguidas | Oportunidade antiga igual; 10 protocolos distintos | Prints; desfazer a alteração de teste |

**Dados/fixtures:** 2 clientes de teste (1 PJ Saúde, 1 PJ Seguros); usuário Colaborador Saúde e Colaborador Seguros.
**Caminhos de erro obrigatórios:** produto sem modelo; falha no meio da criação; seguradora de outra frente; criação simultânea.
**Evidência exigida:** prints + seção Fluxos no mapa + revisões registradas nas Emendas.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T4.01 | Conferir e corrigir os modelos J6, J3 e genérico com os fluxos piloto e completar a página Fluxos (só consulta) | Maestro | SPEC-1-004 | Checklist item 1 | RED (divergências) → GREEN (página confere com os fluxos piloto) | ☐ |
| T4.02 | Mover a abertura de oportunidade para uma operação única no banco (escolha do modelo, protocolo, cópia com versão, responsável, aviso do genérico) | Maestro | SPEC-1-004 | CA-1-14, CA-1-15, CA-1-16, CA-1-17 | GREEN + REGRESSÃO | ☐ |
| T4.00 | Revisar J6 (Isabelle) e J3 (Sheyla) na página Configurações → Fluxos e devolver as anotações (depois das visões, antes do ensaio) | Isabelle, Sheyla | SPEC-1-004 | Checklist item 2 | Anotações recebidas | ☐ |
| T4.03 | Aplicar nos modelos J6 e J3 a revisão de Isabelle e Sheyla e registrar nas Emendas | Maestro | SPEC-1-004 | CA-1-18 | GREEN (página confere com a revisão) | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| 20/10/2026 | Consultoria | T4.00, T4.03 | A revisão dos fluxos sai do início da fase e vai para depois das visões: T4.01 confere só com os fluxos piloto; a revisão é aplicada na nova T4.03 |
| 20/10/2026 | Consultoria | Fora de escopo | Páginas Fluxos e Catálogo seguem só de consulta na Fase 1; a configuração completa pela interface (incluir, editar, ativar/desativar) fica explícita na Fase 2 |
