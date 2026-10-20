# Auditoria do tronco — Fase 1 (T1.01)

**Data:** 20/10/2026 · **SPEC:** SPEC-1-001 · **Base auditada:** repositório de código, commit `ae41b24`
(`05_entregas/central-do-fluxo-comercial`), migrações `supabase/migrations/` e telas em `src/`.

Conferência feita sem alterar nada no sistema. Este arquivo é a base do `docs/mapa-do-tronco.md`, que
o Maestro cria no repositório de código na T1.02, junto com a correção das lacunas estruturais.

## Resultado

Os 10 prompts do tronco foram aplicados. Todas as telas existem e os dados iniciais conferem.
Restam **6 lacunas estruturais** (abaixo), a corrigir na T1.02.

## Conferência dos dados iniciais (CA-1-02)

| Item | Esperado | Encontrado |
|---|---|---|
| Equipes | 2 | 2 (Seguros; Saúde e Odonto) |
| Seguradoras/operadoras | 8 | 8 |
| Produtos | 14 | 14 |
| Origens de lead | 8 | 8 |
| Motivos de perda | 9 | 9 |
| Modelos de fluxo | 3 (5, 7 e 12 etapas) | 3 (5, 7 e 12 etapas) |

## Telas e rotas (CA-1-03)

| Tela | Rota |
|---|---|
| Login + Esqueci minha senha | `/` |
| Minha fila | `/minha-fila` |
| Oportunidades (lista, exportar CSV) | `/oportunidades` |
| Ficha da oportunidade | `/oportunidades/:id` |
| Quadro | `/quadro` |
| Clientes / ficha do cliente | `/clientes`, `/clientes/:id` |
| Painel | `/painel` |
| Usuários (Administrador) | `/usuarios` |
| Configurações → Fluxos / Catálogo (Administrador) | `/configuracoes/fluxos`, `/configuracoes/catalogo` |
| Nova oportunidade | modal no topo e na ficha do cliente |

## Gavetas: nome real no banco (CA-1-01)

| Gaveta (modelo de referência) | Tabela real | Observação |
|---|---|---|
| Perfis de usuário | `profiles` | perfil, função e situação como listas fixas |
| Equipes | `equipes` | |
| Usuário × equipe | `profile_equipes` | |
| Clientes | `clientes` | PF e PJ na mesma tabela; `origem` é texto livre (lacuna 5) |
| Contatos | `clientes_contatos` | só um principal por cliente (automático) |
| Seguradoras | `seguradoras_operadoras` | |
| Produtos | `produtos` | ligado à equipe |
| Origens | `origens_lead` | sem ativo/inativo (lacuna 6) |
| Motivos | `motivos` | sem ativo/inativo (lacuna 6) |
| Modelos de fluxo | `fluxo_modelos` | |
| Etapas do modelo | `fluxo_etapas` | |
| Itens do modelo | `fluxo_checklist_itens` | |
| Oportunidades | `oportunidades` | sem valor fechado (lacuna 1) e sem motivo de perda (lacuna 2) |
| Etapas da oportunidade (cópia) | `oportunidades_etapas` | sem "concluída por" (lacuna 3) |
| Itens da oportunidade (cópia) | `oportunidades_checklist_itens` | o anexo fica no próprio item (lacuna 4) |
| Documentos | — (colunas `arquivo_*` no item) | lacuna 4 |
| Interações | `oportunidades_interacoes` | |
| Histórico | `oportunidades_linha_tempo` | |
| Notificações | `notificacoes` | |
| Pasta de arquivos | bucket privado `oportunidades-anexos` | |
| Protocolo | `oportunidades_protocolo_seq` + `gerar_proximo_protocolo()` | formato NG-AAAA-000001 |

Ações do fluxo no banco: `abrir_oportunidade`, `concluir_etapa_oportunidade`, `assumir_etapa_oportunidade`,
`voltar_etapa_oportunidade`, `confirmar_ganho_oportunidade`, `marcar_oportunidade_perdida`,
`arquivar_oportunidade`, `marcar_notificacoes_lidas`; visibilidade em `pode_ver_oportunidade`.

## Lacunas estruturais (corrigir na T1.02)

| # | Lacuna | Efeito hoje |
|---|---|---|
| 1 | Oportunidade sem campo de **valor fechado** | Ao confirmar o ganho, o valor fechado sobrescreve o valor estimado |
| 2 | Oportunidade sem **motivo de perda** e **observação da perda** | O motivo só fica na linha do tempo; o Painel conta motivos a partir dela |
| 3 | Etapa da oportunidade sem **concluída por** | O sistema usa o responsável da etapa no lugar de quem concluiu |
| 4 | **Documentos** guardados no próprio item do checklist (um arquivo por item) | Item como "Cotações anexadas" não aceita vários arquivos; novo anexo substitui o anterior |
| 5 | **Origem do cliente** em texto livre | Não usa a lista de origens do catálogo |
| 6 | **Origens** e **motivos** sem ativo/inativo | Não é possível desativar uma opção sem apagá-la |

## Alterações feitas pela N&G durante o tronco (aceitas)

- **Prompt 6, Saúde PME 3 a 29 vidas, etapa 10 (Cancelamento do plano anterior):** a equipe ajustou a
  etapa para ficar com o responsável comercial (e não com a fila Pós-venda) e trocou o item único por
  três itens condicionais: "Cliente possui plano anterior ativo?" (obrigatório), "Plano anterior já foi
  cancelado?" e a orientação/carta de cancelamento (opcionais). A etapa 12 continua na fila Saúde e
  Odonto / Pós-venda. Não é lacuna.
