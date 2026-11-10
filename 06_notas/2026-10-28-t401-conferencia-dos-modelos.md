# T4.01 — conferência dos modelos de fluxo contra `03_documentos/fluxos-piloto-fase-1.md`

**Data:** 2026-10-28 · **Task:** T4.01 · **SPEC:** SPEC-1-004
**Método:** leitura das migrações que criam os modelos (`20261016223000_create_fluxo_modelos_tables.sql`
e `20261016225500_seed_fluxo_saude_pme.sql`) comparada linha a linha com o documento dos fluxos piloto.

## Veredito: os três modelos conferem — nenhuma correção necessária

| Modelo | Etapas no banco | Etapas no documento | Veredito |
|---|---|---|---|
| J6 · Saúde PME 3–29 vidas (SulAmérica) | 12 | 12 | ✅ confere |
| J3 · Seguro Empresarial (Porto Seguro) | 7 | 7 | ✅ confere |
| Genérico | 5 | (não detalhado no documento) | ✅ suficiente para o papel de fallback |

## J6 · Saúde PME 3 a 29 vidas

| # | Documento | Banco | Veredito |
|---|---|---|---|
| 1 | Lead | Lead · Trabalho interno · responsável comercial | ✅ |
| 2 | Qualificação | Qualificação · Aguardando cliente · responsável comercial | ✅ |
| 3 | Cotação multioperadora | Cotação multioperadora · Trabalho interno | ✅ |
| 4 | Comparativo enviado | Comparativo enviado · Aguardando cliente | ✅ |
| 5 | Negociação | Negociação · Aguardando cliente | ✅ |
| 6 | Documentação para implantação | Documentação para implantação · Aguardando cliente | ✅ |
| 7 | Digitação no portal da operadora | Digitação no portal da operadora · Trabalho interno | ✅ |
| 8 | Assinatura e críticas | Assinatura e críticas · Aguardando cliente | ✅ |
| 9 | Primeiro boleto | Primeiro boleto · Aguardando cliente | ✅ |
| 10 | Cancelamento do plano anterior | Cancelamento do plano anterior · Aguardando cliente | ✅ |
| 11 | Fechamento | Fechamento · Trabalho interno · **etapa de ganho** | ✅ |
| 12 | Boas-vindas e implantação | Boas-vindas e implantação · fila da equipe Saúde e Odonto · função Pós-venda | ✅ |

**Itens de checklist:** os obrigatórios do documento estão presentes como `obrigatorio = true`
(Origem na etapa 1; CNPJ, nº de titulares, idades, cidade, plano atual na etapa 2; cotações e
operadoras cotadas na etapa 3; comparativo na 4; operadora e plano escolhidos na 5; documentos
na 6; proposta digitada na 7; assinatura e críticas na 8; boleto e pagamento na 9; plano anterior
na 10; Quiver e mensalidade na 11; materiais e contato do RH na 12).
Os itens não obrigatórios do documento (parceiro/indicador, hospital de preferência,
coparticipação) também entraram como opcionais. ✅

## J3 · Seguro Empresarial — Porto Seguro

| # | Documento | Banco | Veredito |
|---|---|---|---|
| 1 | Coleta de dados do risco | Coleta de dados do risco · Aguardando cliente | ✅ |
| 2 | Cotação | Cotação · Trabalho interno | ✅ |
| 3 | Cotação enviada | Cotação enviada · Aguardando cliente | ✅ |
| 4 | Elaborar proposta | Elaborar proposta · Trabalho interno | ✅ |
| 5 | Registro no Quiver (pendente) | Registro no Quiver — pendente · Trabalho interno | ✅ |
| 6 | Aguardando apólice (até 24h) | Aguardando apólice · Aguardando seguradora | ✅ |
| 7 | Apólice emitida | Apólice emitida · Trabalho interno | ✅ |

**Itens de checklist:** os obrigatórios do documento estão no banco (tipo de pessoa e CPF/CNPJ,
motivo, endereço do risco, atividade, valores por cobertura, imóvel alugado, histórico de sinistros
na etapa 1; PDF e validações na 2; envio, validade e follow-up na 3; dados da proposta e proposta
finalizada na 4; nº da proposta, prêmio/parcelas/comissão na 5; nº e data no Quiver, apólice e
boleto anexados na 7). ✅

## Observações (não são defeitos)

1. **Etapa 6 do J6 e etapa 7 têm `[VALIDAR D-11]` no documento.** O banco manteve "responsável
   comercial" nas etapas 6 a 9 e "fila da equipe / Pós-venda" só na 12, que é exatamente o que a
   SPEC-1-004 manda fazer enquanto a D-11 não for respondida.
2. **A etapa 10 do J6** já traz o ajuste da N&G de 20/10 descrito no documento ("ajuste da N&G no
   tronco"): a descrição deixa claro que o cancelamento do plano anterior só é exigido quando ele
   ainda estiver ativo. ✅
3. **Nomes de etapa:** "Registro no Quiver — pendente" e "Registro no Quiver (pendente)" diferem só
   na pontuação; não é divergência funcional.

## O que esta task corrigiu de fato

Não foi nos modelos: foi na **abertura**. Faltavam dois itens da regra 1f da SPEC, ambos agora
implementados na migração `20261028180000`:

1. **Histórico:** a abertura não gravava nada em `oportunidades_linha_tempo`. Agora grava um
   registro tipo `criacao`, com autor, protocolo, modelo, versão e etapa inicial.
2. **Notificação:** o responsável da etapa 1 não era avisado. Agora é, quando não for quem criou.
   E, no caso de fluxo genérico (regra 2), os administradores recebem "Produto sem fluxo próprio".

Prova de execução: `06_notas/2026-10-28-t401-prova-abertura.txt` — 35 casos, 35 PASS, em Postgres
real (PGlite). O teste pegou um bug antes da publicação: `v_responsavel_etapa_id` era reaproveitada
a cada volta do laço e, ao final, guardava a última etapa (de fila, NULL), então a notificação
nunca sairia. Corrigido com uma variável própria para o responsável da etapa 1.
