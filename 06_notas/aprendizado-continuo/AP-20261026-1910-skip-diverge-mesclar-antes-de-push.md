# AP-20261026-1910 — Skip diverge do repositório local: mesclar antes de empurrar

- Status: candidato
- Escopo: projeto do cliente (N&G — Central do Fluxo Comercial)
- Task/SPEC: T2.01 · SPEC-1-002
- Sinal: entre a edição local e o push via API, o Skip acrescentou código próprio no remoto (telas de senha/recuperação, `updatePassword`, redirect de recuperação, fix de tipos na migração do trigger). Um push cego da versão local teria apagado esse trabalho.
- Evidência: diff `afe8fff..origin/main` no submódulo (commits 1b4acd5, 7afd9a5, 4b09344, e020c28); arquivo remoto lido antes do push mostrava `updatePassword` e `AuthCallback`/`DefinirSenha` ausentes no local.
- Regra reutilizável: antes de qualquer push para o repositório de código de um projeto Skip, ler o estado atual do remoto (get_file_contents ou fetch) e mesclar as adições do Skip; nunca empurrar a versão local por cima.
- Quando aplicar: toda sincronização Maestro → GitHub em projetos com agente ativo no Skip.
- Quando não aplicar: repositório sem agente/plataforma escrevendo concorrentemente.
- Confiança: alta — divergência observada duas vezes no mesmo dia (64850af e 89053d3).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
