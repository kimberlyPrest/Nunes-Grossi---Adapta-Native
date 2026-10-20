# Mapa do tronco — Central do Fluxo Comercial N&G

**Atualizado em:** 20/10/2026 · **Base:** repositório de código, commit `afe8fff` (v0.0.13), depois da T1.02
**Para quem:** Maestro e consultoria. **Este mapa é a verdade sobre nomes de telas, tabelas e campos.**
Antes de mudar qualquer coisa, confira aqui; depois de mudar, atualize este arquivo.

Origem: auditoria em `06_notas/2026-10-20-auditoria-do-tronco.md` (T1.01) e ajustes estruturais da T1.02
(migração `supabase/migrations/20261017010000_seis_ajustes_estruturais.sql`).

## Árvore do tronco

```
Central do Fluxo Comercial N&G
├── Login (/)  ── Supabase Auth · "Esqueci minha senha"
└── Área logada (Layout: menu lateral, sino, Nova oportunidade)
    ├── Minha fila (/minha-fila) ─────────── oportunidades + oportunidades_etapas (comigo / fila da equipe)
    ├── Oportunidades (/oportunidades) ───── oportunidades · busca, filtros, CSV
    │   └── Ficha (/oportunidades/:id)
    │       ├── Cabeçalho ─────────────────── oportunidades (valor estimado, valor fechado, motivo da perda)
    │       ├── Trilha de etapas ──────────── oportunidades_etapas (concluída por, em)
    │       ├── Checklist da etapa atual ──── oportunidades_checklist_itens
    │       │   └── Documentos do item ────── oportunidades_documentos + bucket oportunidades-anexos
    │       ├── Interações ────────────────── oportunidades_interacoes
    │       ├── Linha do tempo ────────────── oportunidades_linha_tempo
    │       └── Ações: concluir · voltar · assumir · ganho · perda · arquivar (funções no banco)
    ├── Quadro (/quadro) ──────────────────── fluxo_modelos → oportunidades_etapas
    ├── Clientes (/clientes) ──────────────── clientes
    │   └── Ficha (/clientes/:id) ─────────── clientes_contatos · oportunidades do cliente
    ├── Painel (/painel) ──────────────────── oportunidades (+ motivos, equipes)
    ├── Nova oportunidade (modal) ─────────── abrir_oportunidade() → cópia de etapas e itens
    ├── Sino ──────────────────────────────── notificacoes
    └── Administrador
        ├── Usuários (/usuarios) ──────────── profiles · profile_equipes · equipes · função admin-users
        └── Configurações (/configuracoes) — só consulta na Fase 1 (edição pela tela na Fase 2)
            ├── Catálogo (/configuracoes/catalogo) ── seguradoras_operadoras · produtos · origens_lead · motivos
            └── Fluxos (/configuracoes/fluxos) ────── fluxo_modelos → fluxo_etapas → fluxo_checklist_itens
```

## Tabelas

Listas fixas (enum) entre colchetes. `id`, `created_at` e `updated_at` existem em quase todas e não são repetidos.

### Pessoas e equipes

| Tabela | Colunas principais | Ligações |
|---|---|---|
| `profiles` | nome, email (único), telefone, perfil [Administrador, Gestor, Colaborador], funcao [Comercial, Pós-venda, Gestão], situacao [ativo, inativo] | id = usuário de login (`auth.users`) |
| `equipes` | nome (único), descricao | — |
| `profile_equipes` | — | profile_id → profiles; equipe_id → equipes (único por par) |

### Clientes

| Tabela | Colunas principais | Ligações |
|---|---|---|
| `clientes` | tipo [PJ, PF]; PJ: cnpj, razao_social, nome_fantasia, segmento, porte [micro, pequena, média, grande], numero_vidas; PF: cpf, nome_completo, data_de_nascimento; cidade, uf, indicado_por_tipo [cliente, colaborador, parceiro], indicado_por_nome, observacoes, situacao [ativo, arquivado]; `origem` (texto antigo, mantido) | origem_id → origens_lead; responsavel_comercial_id → profiles |
| `clientes_contatos` | nome, cargo_area [RH, Financeiro, Sócio, Administrativo, Outro], telefone, email, principal (só um por cliente, automático) | cliente_id → clientes |

CPF e CNPJ são únicos (índice), sem validação de dígito ainda (SPEC-1-003).

### Catálogo

| Tabela | Colunas principais | Ligações |
|---|---|---|
| `seguradoras_operadoras` | nome (único), frente [Seguros, Saúde], canal_cotacao [Portal, Multicálculo, E-mail, WhatsApp], situacao [ativa, inativa] | — |
| `produtos` | nome (único), frente, equipe_nome, situacao | equipe_id → equipes |
| `origens_lead` | nome (único), situacao [ativa, inativa] | — |
| `motivos` | nome, tipo [perda, descarte, adiamento], situacao [ativa, inativa] (único por nome + tipo) | — |

### Modelos de fluxo

| Tabela | Colunas principais | Ligações |
|---|---|---|
| `fluxo_modelos` | nome, is_generico, versao, situacao [ativo, inativo], descricao | produto_id → produtos; seguradora_id → seguradoras_operadoras (vazio = qualquer) |
| `fluxo_etapas` | ordem (única no modelo), nome, descricao, tipo [Trabalho interno, Aguardando cliente, Aguardando seguradora], responsavel_tipo [responsavel_comercial, fila_equipe], funcao, prazo_dias, is_etapa_ganho | modelo_id → fluxo_modelos; equipe_id → equipes (fila) |
| `fluxo_checklist_itens` | ordem, titulo, tipo [Documento, Informação, Confirmação], formato [texto, número, valor, data, sim_nao, opções], opcoes (lista), obrigatorio | etapa_id → fluxo_etapas |

### Oportunidades

| Tabela | Colunas principais | Ligações |
|---|---|---|
| `oportunidades` | protocolo (único, NG-AAAA-000001), fluxo_versao, usou_fluxo_generico, valor_estimado, **valor_fechado**, data_prevista_fechamento, observacoes, situacao [Em andamento, Ganha — em finalização, Concluída, Perdida, Arquivada; também Ganha, Descartada, Adiada, sem uso], numero_quiver, **observacao_perda** | cliente_id, produto_id, seguradora_id, fluxo_modelo_id, equipe_id, responsavel_comercial_id, origem_id, criado_por, etapa_atual_id → oportunidades_etapas, **motivo_perda_id** → motivos |
| `oportunidades_etapas` (cópia) | ordem, nome, descricao, tipo, responsavel_tipo, funcao, prazo_dias, is_etapa_ganho, situacao [pendente, em_andamento, concluida], iniciada_em, concluida_em | oportunidade_id; equipe_id (fila); responsavel_usuario_id → profiles; **concluida_por_id** → profiles |
| `oportunidades_checklist_itens` (cópia) | ordem, titulo, tipo, formato, opcoes, obrigatorio, valor, concluido, concluido_por, concluido_em; arquivo_nome, arquivo_url, arquivo_tamanho (legado, último arquivo) | etapa_oportunidade_id → oportunidades_etapas |
| **`oportunidades_documentos`** | arquivo_nome, arquivo_caminho (pasta privada), arquivo_tamanho, enviado_por, enviado_em | oportunidade_id; checklist_item_id → oportunidades_checklist_itens |
| `oportunidades_interacoes` | tipo [Ligação, WhatsApp, E-mail, Reunião, Outro], data_contato, resumo, registrado_por | oportunidade_id |
| `oportunidades_linha_tempo` | tipo (criacao, etapa_concluida, troca_responsavel, documento, interacao, checklist_item, quiver, geral), titulo, descricao, metadados | oportunidade_id; usuario_id |
| `notificacoes` | titulo, mensagem, link, lida, lida_em | destinatario_id → profiles; oportunidade_id |
| `oportunidades_protocolo_seq` | ano, ultimo_numero | — |

**Pasta de arquivos:** bucket privado `oportunidades-anexos` (até 50 MB por arquivo); download por link temporário de 1 hora.

## Funções no banco

| Função | O que faz |
|---|---|
| `abrir_oportunidade` | Escolhe o modelo (produto + seguradora → produto → genérico), gera protocolo, copia etapas e itens, define etapa 1 e responsável |
| `gerar_proximo_protocolo` | Protocolo sequencial por ano, sem repetição |
| `concluir_etapa_oportunidade` | Confere obrigatórios (Documento = ao menos um arquivo), grava concluída em/por, passa para a próxima etapa (pessoa ou fila), notifica; última etapa → Concluída |
| `assumir_etapa_oportunidade` | Pessoa da equipe (e função) assume a etapa da fila |
| `voltar_etapa_oportunidade` | Motivo obrigatório; reabre a etapa anterior e limpa concluída em/por |
| `confirmar_ganho_oportunidade` | Grava seguradora, valor fechado e nº da proposta; situação → Ganha — em finalização |
| `marcar_oportunidade_perdida` | Grava motivo e observação da perda; situação → Perdida |
| `arquivar_oportunidade` | Administrador e gestor; situação → Arquivada |
| `marcar_notificacoes_lidas` | Marca uma ou todas as notificações da pessoa |
| `pode_ver_oportunidade` | Regra de visibilidade por perfil, equipe, responsável e fila |
| `is_admin`, `is_active_user` | Apoio às regras de acesso |

Função no servidor: `admin-users` (convidar, editar e ativar/desativar usuários; só administrador).

## Dados iniciais

2 equipes · 8 seguradoras/operadoras · 14 produtos · 8 origens · 9 motivos de perda · 3 modelos de fluxo:
Fluxo Genérico (5 etapas), Seguro Empresarial — Porto Seguro (7), Saúde PME 3 a 29 vidas (12; etapa 10 com o
responsável comercial por ajuste da N&G; etapa 12 na fila Saúde e Odonto / Pós-venda).

## Ajustes estruturais da T1.02

| # | Ajuste | Onde ficou |
|---|---|---|
| 1 | Valor fechado separado do estimado | `oportunidades.valor_fechado`; ficha mostra os dois; Painel soma o fechado |
| 2 | Motivo e observação da perda | `oportunidades.motivo_perda_id`, `observacao_perda`; ficha e Painel usam esses campos |
| 3 | Quem concluiu cada etapa | `oportunidades_etapas.concluida_por_id`; trilha mostra o nome |
| 4 | Vários arquivos por item | tabela `oportunidades_documentos`; anexar sempre adiciona |
| 5 | Origem do cliente ligada à lista | `clientes.origem_id`; clientes antigos migrados (sem correspondência → "Outro" + texto nas observações) |
| 6 | Ativo/inativo em origens e motivos | `origens_lead.situacao`, `motivos.situacao` |

### Pendências menores da T1.02 (ajustar na SPEC indicada)

- A lista de origens da **Nova oportunidade** ainda mostra também as inativas (o cadastro de cliente e a
  perda já filtram só as ativas) → SPEC-1-004 (T4.02).
- A página **Configurações → Catálogo** não mostra a situação (ativa/inativa) de origens e motivos → SPEC-1-004 (T4.01).
- Na conclusão de etapa, um item Documento marcado como concluído conta como preenchido mesmo sem arquivo;
  deve valer só "ao menos um arquivo" → SPEC-1-005 (T5.01).
