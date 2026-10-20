# Central do Fluxo Comercial N&G — contexto para o agente

Você está trabalhando no projeto da **Nunes & Grossi Corretora de Seguros** (Santos/SP) no programa
Adapta Native. Este repositório é o **contexto** do projeto: objetivo, regras, fase atual, SPECs e
tarefas. O código do sistema fica no repositório gerado pelo Skip.

## Objetivo

Construir a **Central do Fluxo Comercial N&G**: ao abrir uma oportunidade (cliente + produto +
seguradora/operadora), o fluxo certo aparece com etapas, checklist, responsáveis e prazos; a
oportunidade passa de pessoa em pessoa até o ganho ou a perda; a gestão vê o funil em tempo real. Nas
fases seguintes, a central se conecta ao Quiver PRO e passa a identificar oportunidades na carteira.
Detalhes em `01_projeto/objetivo.md` e `01_projeto/visao-do-projeto.md`.

## Pessoas

- **Champion e aceite das fases:** Viviane Grossi (sócia-diretora, comercial e operacional)
- **Patrocinador:** Pedro Nunes (sócio-fundador)
- **Executora (roda as tasks e os testes):** Louise Faraguti
- **Validação dos fluxos:** Isabelle Duarte (Saúde), Sheyla Bernardes (Seguros corporativos), Carol (Auto)
- **Consultora técnica:** Kimberly Prestes (Adapta)

## Onde está cada coisa

| Pasta/arquivo | Conteúdo |
|---|---|
| `01_projeto/` | Escopo definitivo completo (`escopo-definitivo.md`), objetivo, visão das 5 fases, constituição (regras permanentes), regras e permissões, decisões pendentes |
| `02_reunioes/` | Índice e atas das reuniões |
| `03_documentos/` | Prompts do tronco (Skip) e fluxos piloto da fase atual |
| `04_fase-atual/fase.md` | **A fase em execução**: resultado, telas, regras, testes de aceite e a tabela de Tasks |
| `04_fase-atual/specs/` | SPECs da fase atual (contrato de cada entrega, com critérios de aceite e prova) |
| `05_entregas/` | Entregas concluídas e evidências; `central-do-fluxo-comercial/` é o submódulo com o código do sistema (somente leitura) |
| `06_notas/` | Notas de trabalho do projeto; `debug/` para registros de problemas |
| `STATUS.md` / `changelog.md` | Situação atual e histórico |

## Como trabalhar

Quando pedirem **“execute a próxima task”**, siga as **Instruções para o Maestro** em
`04_fase-atual/fase.md`. Em resumo:

1. Pegue a primeira task ☐ cujo dono é o Maestro e leia a SPEC dela por inteiro.
2. Use `docs/mapa-do-tronco.md` (no repositório de código) como verdade sobre telas, tabelas e campos;
   o Skip cria o banco junto com as telas, então não suponha nomes.
3. Confira como está hoje antes de mudar, rode a prova RED, implemente só o que a task pede, rode o
   GREEN e entregue testes em linguagem simples para a Louise.
4. Atualize o mapa, marque a task quando os testes passarem e registre no `changelog.md`.
5. Se faltar decisão, travar ou sair do que a SPEC diz: pare, registre em `06_notas/debug/` e peça
   para avisar a Kimberly. Não invente regra de negócio.

## Linhas vermelhas (sempre)

Leia `01_projeto/constituicao.md`. Em especial: nenhuma exclusão definitiva de dados; nenhum dado de
cartão nem conteúdo médico; senhas e chaves só no cofre do Supabase, nunca em código, prompt, arquivo
deste repositório ou WhatsApp; permissões sempre aplicadas no banco; tudo em português do Brasil.
