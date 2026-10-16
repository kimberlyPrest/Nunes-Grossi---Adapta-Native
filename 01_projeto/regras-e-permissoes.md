# Regras e permissões

## Perfis de acesso

| Ação | Administrador | Gestor | Colaborador |
|---|---|---|---|
| Ver oportunidades | Todas | Todas da(s) sua(s) equipe(s) | As suas, as que participa e a fila da sua equipe |
| Criar oportunidade e cliente | Sim | Sim | Sim |
| Avançar etapa, marcar checklist, anexar | Sim | Sim | Nas oportunidades em que é responsável da etapa |
| Reatribuir responsável | Sim | Na sua equipe | Não |
| Marcar ganho ou perda | Sim | Sim | Sim, com motivo obrigatório |
| Arquivar oportunidade | Sim | Sim | Não |
| Excluir definitivamente | Ninguém | — | — |
| Painel | Completo | Da(s) sua(s) equipe(s) | Os próprios números |
| Usuários e configurações | Sim | Não | Não |

**Equipes:** Seguros e Saúde/Odonto (uma pessoa pode estar nas duas). **Funções:** Comercial,
Pós-venda, Gestão. Uma **fila** é a combinação equipe + função (ex.: Saúde e Odonto / Pós-venda).

## Regras do sistema na Fase 1

| ID | Regra |
|---|---|
| RN-01 | Toda oportunidade aberta tem cliente, produto, responsável e etapa atual. |
| RN-02 | Uma etapa só é concluída com os itens obrigatórios do checklist completos. |
| RN-03 | Ao concluir uma etapa, o responsável da próxima é definido pela regra do fluxo e avisado. |
| RN-04 | Cliente é único por CPF/CNPJ. Não se cria cliente duplicado. |
| RN-05 | Perda exige motivo da lista padronizada. |
| RN-06 | Não existe exclusão definitiva: oportunidades e clientes são arquivados, com histórico preservado. |
| RN-07 | Toda alteração de etapa, responsável, valor ou status é registrada no histórico (quem e quando). |

## Regras do negócio dos fluxos da Fase 1

| ID | Regra | Fluxo |
|---|---|---|
| RO-02 | Danos Morais ≤ 100% da Responsabilidade Civil. | J3 |
| RO-03 | Perda de aluguel = aluguel mensal × 6. | J3 |
| RO-09 | Saúde: vigência começa após a quitação do 1º boleto (até 72h). | J6 |
| RO-10 | Saúde: cancelar o plano anterior só depois da implantação e do pagamento do novo. | J6 |
| RO-11 | Saúde: redução de carência exige ≥ 12 meses no plano anterior e cancelamento há ≤ 30 dias. | J6 |
| RO-13 | Saúde: sócio que não adere ao plano precisa de carta de não adesão. | J6 |

## Dados que o sistema não guarda

- Fotos, números, validade ou código de segurança de cartão de crédito.
- Declarações de saúde e informações médicas de beneficiários (só “entregue” ou “sim/não”).
- Senhas de portais de seguradoras.
