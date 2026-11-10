# Estado atual — Adapta Cliente

- task_id: nenhuma (T6.01 concluída em 10/11)
- champion: Viviane Grossi (executora dos testes: Louise Faraguti)
- spec: 04_fase-atual/specs/SPEC-1-006-visoes-do-comercial.md
- etapa: concluida
- autorizacao_implementacao: confirmada — "sim" (Pedro/owner, 10/11 10:03), paginação + CSV juntos
- teste_humano: aprovado — "todos os itens testados e aprovados" (Pedro/owner, 10/11 12:19). Lista e CSV conferidos nos DOIS perfis: colaborador Saúde vê e exporta 3; administrador vê e exporta 8. Busca por CNPJ sem pontuação ok. Login: 4/4 aprovado.
- verificacao_automatica: passou — commit remoto 761cfd0: tsc 0 erros, oxlint 71 warnings/0 errors (= baseline), vite build OK; loop de paginação com mock do PostgREST: 2500 registros → 3 requisições → 2500 carregados, 0 duplicados; fatiamento testado isoladamente (3 sem controle, 26 → 25+1, página inexistente cai na última)
- PUBLICADO NO AR: bundle assets/index-DMKkV_Ia.js, 1.017.983 bytes, sha256 ce06c0dd… idêntico ao build local do commit 25be730. RESSALVA: o bloco deployment do .skip.config.json continua desatualizado (lastProdBuildRef=97ecee9) — a prova de publicação é o bundle servido, não o config.
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-11-10-1219-volume-por-perfil.md
- ultima_acao: T6.01 concluída e marcada ☑ 10/11 na fase; STATUS.md e changelog.md atualizados (24/35 = 69%); debug do login registrado no mesmo fechamento
- proxima_acao: apresentar a T6.02 (quadro por modelo de fluxo, CA-1-25) quando o cliente pedir a próxima task
- riscos_residuais: (1) paginação visual, reset ao filtrar e CSV na página 2 NÃO são testáveis com 8 oportunidades — cobertos por prova técnica, não por tela; o cenário que motivou a task (>1000 linhas) não é reproduzível na base atual; (2) o CSV carrega tudo antes de exportar — correto, mas lento em volumes grandes; (3) a busca textual roda em JS sobre o resultado completo, custo cresce com o volume; (4) T5.04 continua bloqueada pela D-24 (trigger × service), aguardando a Kimberly
- atualizado_em: 2026-11-10T12:19:00-03:00
