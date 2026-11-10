# Estado atual — Adapta Cliente

- task_id: T5.04 (histórico automático, linha do tempo e interações)
- champion: Viviane Grossi (executora dos testes: Louise Faraguti)
- spec: 04_fase-atual/specs/SPEC-1-005-execucao-do-fluxo.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada — "a Kimberly aprovou a correção do bug que estava pendente dela decidir, siga com a implementação" (Pedro/owner, 10/11 ~16:05) + "D-24 confirmada — Opção C — híbrido" (formulário, 10/11 16:12)
- teste_humano: pendente — depende do Skip aplicar a migração 20261110170000; roteiro entregue a Pedro
- verificacao_automatica: passou — GREEN 13/13 em Postgres real (PGlite) com a cadeia completa de 25 migrações (24 do repo + a nova): desmarcar item grava evento com auth.uid(), editar valor de item concluído grava de/para, marcar item NÃO duplica, valor_estimado grava, UPDATE sem mudança real e de outra coluna não dispara, reversão documentada funciona; RED confirmado sem a migração: 9 FAIL nos mesmos casos
- PUBLICADO: migração no repositório de código via API (commit 882c718, byte-idêntica ao disco e reprovada GREEN com o arquivo publicado); frontend sem mudança — produção segue servindo index-DMKkV_Ia.js. Falta o Skip aplicar a migração no banco.
- aprendizado: pendente (triagem no fechamento da task)
- ultima_acao: migração T5.04 escrita, provada (RED→GREEN) e publicada no repo de código; registros publicados no repositório de contexto
- proxima_acao: Skip aplicar a migração (prompt entregue a Pedro) → teste humano da T5.04
- riscos_residuais: (1) o teste humano depende do Skip aplicar a migração primeiro; (2) desmarcar Informação exige limpar o campo (texto vazio desmarca) — cobrir no roteiro; (3) valor da oportunidade não tem edição na tela — o trigger cobre chamada direta (regra 10); prova de tela não se aplica a esse furo
- atualizado_em: 2026-11-10T16:45:00-03:00
