# Controle de aprendizado contínuo — N&G

- 2026-10-26T19:10:00-03:00 · task T2.01 · capturado: 06_notas/aprendizado-continuo/AP-20261026-1910-skip-diverge-mesclar-antes-de-push.md (Skip diverge do local; mesclar antes de push)
- 2026-10-27T13:25:00-03:00 · task T2.02 · capturado: 06_notas/aprendizado-continuo/AP-20261027-1325-teste-rls-trocar-de-papel.md (teste de RLS exige SET LOCAL ROLE; editor roda como dono e ignora RLS)
- 2026-10-27T13:40:00-03:00 · task T2.02 · capturado: 06_notas/aprendizado-continuo/AP-20261027-1340-publicacao-por-hash-do-bundle.md (provar publicação comparando hash do bundle servido com o build do commit)
- 2026-10-27T14:30:00-03:00 · task T2.03 · capturado: 06_notas/aprendizado-continuo/AP-20261027-1430-medir-pela-funcao-da-politica.md (provar permissao chamando a funcao que a politica usa; nao mandar placeholder de UUID para o cliente)
- 2026-10-28T11:00:00-03:00 · task T2.04 · capturado: 06_notas/aprendizado-continuo/AP-20261028-1100-checklist-dois-renderizadores.md (checklist tem dois renderizadores - checklist editavel e trilha somente leitura; mudanca em item precisa dos dois)
- 2026-10-30T14:32:00-03:00 · task T5.01 · capturado: 06_notas/aprendizado-continuo/AP-20261030-1432-roteiro-de-teste-nasce-da-spec.md (roteiro de teste humano deve citar CA/emenda e ser cruzado com as emendas Fora de escopo da fase)
- 2026-10-30T19:25:00-03:00 · task T5.02 · capturado: 06_notas/aprendizado-continuo/AP-20261030-1925-regra-literal-trava-fila.md (etapa de fila se autoriza por equipe+funcao, nao por individuo; assumir e marcador, nao reserva exclusiva)
- 2026-10-30T19:57:00-03:00 · task T5.03a · capturado: 06_notas/aprendizado-continuo/AP-20261030-1957-encerrar-so-com-todas-etapas.md (condicao de fechamento deve ser 'todas concluidas', sem isentar a etapa atual; testar com a J6 real)
- 2026-11-10T09:43:00-03:00 · task T5.03 · capturado:06_notas/aprendizado-continuo/AP-2026-11-10-0943-regra-em-rpc-nao-se-prova-com-update.md
- 2026-11-10T12:19:00-03:00 · task T6.01 · capturado:06_notas/aprendizado-continuo/AP-2026-11-10-1219-volume-por-perfil.md
- 2026-11-10T16:20:00-03:00 · task T4.02 · capturado:06_notas/aprendizado-continuo/AP-2026-11-10-1620-pglite-shims-supabase.md (cadeia completa de migrações no PGlite exige shims do ambiente Supabase: auth schema, uid/jwt, roles+GRANTs, storage, stubs de pgcrypto/unaccent)
- 2026-11-10T17:45:00-03:00 · task T5.04 · capturado:06_notas/aprendizado-continuo/AP-2026-11-10-1732-migracao-aplicada-via-mcp-supabase.md (migração DDL pura aplica direto via MCP apply_migration com prova list_migrations+pg_trigger+fumaça revertida; Skip só quando houver código novo)
- 2026-11-10T18:30:00-03:00 · task T6.02 · capturado:06_notas/aprendizado-continuo/AP-2026-11-10-1830-bundle-de-producao-e-a-prova.md (prova de "está no ar" é o bundle servido com string de UI nova; working tree do Skip, espelho GitHub e .skip.config.json não provam)
- 2026-11-10T18:31:00-03:00 · task T6.02 · capturado:06_notas/aprendizado-continuo/AP-2026-11-10-1831-provar-tela-com-render-real.md (mudança de comportamento de tela prova-se renderizando o componente publicado em jsdom com services dublados, e rodando a MESMA suíte contra o arquivo antigo para o RED)

## AP-2026-11-10-1955 — T6.03 (painel)

**Sinal**: em telas de agregado (painel/dashboard), calcular no banco via RPC `SECURITY INVOKER` em vez de varrer linhas no navegador — a RLS do chamador vale dentro da função, o número respeita a permissão sem duplicar regra, e o teto de 1000 linhas do PostgREST some porque a agregação é `count/sum` no servidor.
**Evidência**: PGlite 19/19 com RLS comprovada por perfil; produção `painel_metricas(2026,10)` = 5 ganhas/R$ 10.572,50 · 2 perdidas · 3 em andamento, idêntico à contagem direta nas tabelas; prova jsdom do Painel antigo (RED) mostrava contas no cliente e teto de eventos.
**Aplicação futura**: qualquer métrica/indicador de novo módulo nasce como função SQL SECURITY INVOKER + service fino chamando `rpc()`, nunca `useMemo` sobre lista completa.
**Limites**: agrupar por NOME quando a entidade é clonada por linha (etapa por oportunidade — id não agrega); "voltar" de uma lista filtrada deve levar o contexto de origem na URL (ida: `painelAno/Mes/Equipe`; volta: `?ano&mes&equipe`), senão o usuário volta para o estado padrão e perde o que estava vendo.
**Não reutilizável**: detalhes visuais do Painel, valores do cliente.
