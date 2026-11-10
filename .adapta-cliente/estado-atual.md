# Estado atual — Adapta Cliente

- task_id: T5.04 (histórico automático, linha do tempo e interações)
- champion: Viviane Grossi (executora dos testes: Louise Faraguti)
- spec: 04_fase-atual/specs/SPEC-1-005-execucao-do-fluxo.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada — "a Kimberly aprovou a correção do bug que estava pendente dela decidir, siga com a implementação" (Pedro/owner, 10/11 ~16:05) + "D-24 confirmada — Opção C — híbrido" (formulário, 10/11 16:12)
- teste_humano: pendente — migração 20261110170000 JÁ APLICADA no Supabase de produção via MCP (10/11 16:45), sem depender do Skip; roteiro entregue a Pedro
- verificacao_automatica: passou — GREEN 13/13 em Postgres real (PGlite) com a cadeia completa de 25 migrações (24 do repo + a nova): desmarcar item grava evento com auth.uid(), editar valor de item concluído grava de/para, marcar item NÃO duplica, valor_estimado grava, UPDATE sem mudança real e de outra coluna não dispara, reversão documentada funciona; RED confirmado sem a migração: 9 FAIL nos mesmos casos
- PUBLICADO: migração no repositório de código via API (commit 882c718, byte-idêntica ao disco e reprovada GREEN com o arquivo publicado) e APLICADA no Supabase de produção via MCP apply_migration (25ª migração do banco; triggers trg_t504_valor_estimado e trg_t504_checklist_historico ativos). Teste de fumaça em produção: desmarcar item concluído gravou evento 'Item de checklist desmarcado: "Mensalidade fechada"' com autor Louise Faraguti — dentro de transação revertida (ROLLBACK, zero efeito no dado). Frontend sem mudança — produção segue servindo index-DMKkV_Ia.js.
- aprendizado: pendente (triagem no fechamento da task)
- ultima_acao: migração T5.04 escrita, provada (RED→GREEN), publicada no repo de código e APLICADA no banco de produção com teste de fumaça revertido; registros publicados no repositório de contexto
- proxima_acao: teste humano da T5.04 (roteiro 5 itens entregue); migração já aplicada
- riscos_residuais: (1) desmarcar Informação exige limpar o campo (texto vazio desmarca) — cobrir no roteiro; (2) valor da oportunidade não tem edição na tela — o trigger cobre chamada direta (regra 10); prova de tela não se aplica a esse furo
- atualizado_em: 2026-11-10T16:55:00-03:00
