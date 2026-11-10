# Estado atual — Adapta Cliente

- task_id: T6.03
- champion: Viviane Grossi (executora dos testes: Louise Faraguti · validação do painel: Viviane)
- spec: 04_fase-atual/specs/SPEC-1-006-visoes-e-painel.md
- etapa: concluida
- autorizacao_implementacao: confirmada — "sim" (Pedro/owner, 10/11 18:55), respondendo ao relatório de análise da T6.03 entregue às 18:52
- teste_humano: rodada 2 aprovada 5/5 (Pedro, 19:46) + melhoria "Voltar ao painel com filtros" revalidada e aprovada (Pedro, 19:51 "aprovado")
- revalidacao_conclusao (19:55, do zero): PGlite 19/19 · Painel jsdom 23/23 · Lista jsdom 15/15 (RED 4 na v0.0.59) · service 8/8 · tsc 0 · oxlint 63 warnings 0 erros · produção index-YqC7kbNV.js sha256 8bd25aeb com "painelAno"/"painel_metricas" no bundle servido · RPC INVOKER ativa no banco, migração 2026111019 registrada, painel_metricas(2026,10) = 5 ganhas/R$ 10.572,50 · 2 perdidas/R$ 5.000 · 3 em andamento (bate com contagem direta das tabelas: 5/2/3)
- verificacao_automatica: passou — PGlite 19/19 (cadeia de 21 migrações; RLS do chamador vale na função; desfecho por data_ganho/data_perda, não updated_at; 1202 ganhas sem teto de 1000; reversão OK); tela em jsdom 23/23 (RED 12+ no Painel publicado v0.0.57: sem clique, sem carregamento, contas fora do banco); service 8/8 (RED 6 no publicado; regressão T6.01: 2102 linhas em 3 requests, 0 perda); tsc 0; oxlint 64 warnings 0 erros (baseline 71, queda só de imports mortos removidos); QA Skip 5/5
- PUBLICADO: Skip v0.0.60, commit 493cbba, produção servindo index-YqC7kbNV.js (sha256 8bd25aeb) — conferido no bundle SERVIDO: "painel_metricas", "Voltar ao painel", "painelAno". (v0.0.58 e v0.0.59 foram as publicações anteriores da T6.03)
- MIGRAÇÃO APLICADA em produção (10/11 19:30, MCP apply_migration, projeto ucuxyhbdrlvnqgtrqvas): função painel_metricas(integer, integer, uuid) ativa; teste real: painel_metricas(2026,10,NULL) devolve 5 ganhas/R$ 10.572,50 · 2 perdidas/R$ 5.000 · 3 em andamento/R$ 10.500 · motivo "Cliente desistiu" ×2 — bate com a conferência da análise
- aprendizado: AP-2026-11-10-1955 gravado (ver 06_notas/aprendizado-continuo/)
- ultima_acao: T6.03 CONCLUÍDA (28/35, 80%). Painel com contas no banco (RPC SECURITY INVOKER), clique no número abrindo a lista exata e "Voltar ao painel" preservando período/equipe
- proxima_acao: nenhuma — última task de implementação da Fase 1 concluída. Faltam para fechar a fase: T4.00/T4.03 (Isabelle/Sheyla) e a leva T7 de ensaio/aceite (com a Kimberly)
- riscos_residuais: (1) "em andamento" continua incluindo "Ganha — em finalização" (mesma definição da tela anterior); (2) filtro de equipe usa oportunidades.equipe_id (ressalva herdada da T6.02); (3) espelho GitHub do código segue parado em 16/10; (4) espelho local do repo de código estava defasado em Oportunidades.tsx (sem paginação T6.01) — sincronizado com o working tree do Skip em 10/11 19:20.
- atualizado_em: 2026-11-10T19:55:00-03:00
