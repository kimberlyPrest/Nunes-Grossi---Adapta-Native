# AP-2026-11-10-1831 — Provar tela de verdade: render em jsdom, não leitura de código

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T6.02 · SPEC-1-006
- Sinal: a mudança em `Quadro.tsx` era de fluxo de estado (erro vs. vazio, carregamento em `useCallback`). Ler o código não prova nenhum dos dois: um `if` aparentemente correto pode nunca ser alcançado. Montar uma prova com esbuild (bundle com os services dublados por `onResolve`) + jsdom + `React.createRoot` sob `act` permitiu rodar o componente **publicado** e comparar com o anterior: 9 ✅ / 4 ❌ antes, 13/13 depois. A mesma suíte, com `QUADRO_ARQUIVO` apontando para o arquivo antigo, reproduz o RED — então o teste não é escrito para passar.
- Evidência: `work/t602/rev/prova-quadro.mjs` (render real, 13/13) e a execução com `QUADRO_ARQUIVO=QuadroAnterior.tsx` (9/4, falhando exatamente ERRO-1..4).
- Regra reutilizável: para mudança de comportamento de tela, renderizar o componente em jsdom com os services dublados e afirmar sobre o texto/DOM; guardar o arquivo antigo e rodar a mesma suíte contra ele para provar o RED. Um teste que só passa não prova nada.
- Quando aplicar: qualquer task que mude estado de tela, tratamento de erro, condição de renderização ou texto visível ao usuário, quando o projeto não tiver suíte de testes de componente.
- Quando não aplicar: mudança puramente visual/CSS sem condicional, ou quando o repositório já tiver testes de componente configurados (aí usar o runner do projeto).
- Confiança: alta — o RED foi reproduzido na mesma execução e falhou apenas nos 4 pontos que a task corrige.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
