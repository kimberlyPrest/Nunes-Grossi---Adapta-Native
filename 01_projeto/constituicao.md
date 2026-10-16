# Constituição do projeto

Regras permanentes. Valem para todas as fases, para as pessoas e para os agentes. Só mudam com decisão
registrada da Viviane e da consultora.

## Papéis

| Papel | Quem | Pode decidir |
|---|---|---|
| Champion | Viviane Grossi | Prioridades, decisões D-xx, aceite de fase |
| Patrocinador | Pedro Nunes | Investimentos e direção do projeto |
| Executora | Louise Faraguti | Rodar tasks e testes, configurar o que for pela tela, registrar problemas |
| Validadores de fluxo | Isabelle Duarte, Sheyla Bernardes, Carol | Conteúdo dos fluxos da sua jornada (etapas, documentos, prazos) |
| Consultora técnica | Kimberly Prestes | SPECs, tasks, emendas e destravamentos técnicos |
| Agente (Maestro) | — | Implementar a task em execução, dentro da SPEC |

## Stack permitida

| Peça | Uso |
|---|---|
| Skip | Telas do sistema e hospedagem |
| Supabase (conta da N&G, região São Paulo) | Banco de dados, regras de acesso (RLS), pasta de arquivos privada, funções no servidor, rotinas agendadas e cofre de segredos |
| Maestro | Execução das tasks |
| GitHub | Repositório de contexto (este) e repositório do código |

Qualquer outro serviço (envio de e-mail, WhatsApp, IA, bibliotecas pagas) só entra quando a SPEC da
fase disser, com custo aprovado pela Viviane.

## Limites da executora e do agente

**Pode fazer sem pedir:** rodar as tasks na ordem; corrigir textos, cores e detalhes visuais dentro da
SPEC; cadastrar usuários, clientes e dados de catálogo pela tela.

**Precisa da consultora antes:** criar ou mudar tabelas fora do que a task pede; mudar regras de
acesso; conectar serviços externos; alterar a lógica do fluxo, do ganho ou da perda; qualquer coisa
que não esteja escrita na SPEC.

## Linhas vermelhas

1. **Nada é excluído de verdade.** Clientes e oportunidades são arquivados; o histórico é preservado.
2. **Nenhum dado de cartão de crédito** (foto, número, validade, código) entra no sistema.
3. **Nenhum conteúdo médico.** Para dados de saúde, o sistema registra só “sim/não” e “entregue”.
4. **Senhas e chaves só no cofre de segredos do Supabase.** Nunca em código, prompt, arquivo deste
   repositório, print ou WhatsApp.
5. **Permissões aplicadas no banco**, não só nas telas.
6. **Sem dados reais em teste**, exceto no ensaio real da fase, com usuários reais.
7. **Tudo em português do Brasil**, com mensagens sem termos técnicos.

## Dívida técnica

Quando algo for feito de forma provisória para não travar a fase, registre na SPEC (seção Emendas) e
no código, neste formato:

```
adapta-divida: <limite aceito>; <gatilho para resolver>
```

Exemplo: `adapta-divida: alertas só dentro do sistema; resolver na Fase 3 (avisos por e-mail)`.
