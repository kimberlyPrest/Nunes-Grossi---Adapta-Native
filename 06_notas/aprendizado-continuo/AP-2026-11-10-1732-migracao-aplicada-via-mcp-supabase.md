# AP-2026-11-10-1732 — Migração pode ser aplicada direto no Supabase via MCP

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T5.04 · SPEC-1-005 (regra 10, CA-1-23) · D-24 opção C
- Sinal: o ciclo "pedir ao Skip para aplicar migração" gastava uma rodada e dependia de agente externo; o conector Supabase (MCP) aplica a migração direto no banco de produção, com registro de versão e prova observável (list_migrations + pg_trigger + teste de fumaça em transação revertida).
- Evidência: versão 20261110194225 registrada no projeto Fluxo Comercial; triggers trg_t504_valor_estimado e trg_t504_checklist_historico ativos; evento real gravado e revertido no mesmo teste.
- Regra reutilizável: para migrações DDL puras do projeto, aplicar via MCP apply_migration e provar com list_migrations + pg_trigger + teste de fumaça revertido; usar o fluxo do Skip somente quando a entrega tiver arquivo de código novo.
- Quando aplicar: migração pura de banco (função, trigger, política, coluna) sem arquivo de código novo.
- Quando não aplicar: quando a mesma entrega precisa de build/publicação de frontend — aí o fluxo do Skip cobre os dois numa passada.
- Confiança: alta — duas provas observáveis no banco real (registro de versão e evento gravado com autor).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto (nomes de trigger e evento genérico).
