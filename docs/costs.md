# Custos & SLA

## Servicos considerados

| Servico | Uso no projeto | Custo esperado |
| --- | --- | --- |
| EKS Cluster | Orquestracao dos microservicos | Custo fixo do control plane |
| EC2 Node Group | Execucao dos pods | Varia por tipo e quantidade de instancias |
| Load Balancer | Exposicao do Gateway | Custo por hora e trafego |
| RDS ou Postgres em EC2/K8s | Banco relacional | Depende da opcao escolhida |
| EBS | Volumes dos nodes e banco | Custo por GB provisionado |
| CloudWatch | Logs e metricas | Custo por ingestao/retencao |

## Plano de otimizacao

- Usar instancias pequenas para a entrega.
- Desligar o cluster apos gravar evidencias.
- Configurar minimo de replicas durante testes fora do HPA.
- Usar budgets/alerts para evitar gasto inesperado.
- Registrar estimativa final pela AWS Pricing Calculator antes da entrega.

## SLA

Para ambiente academico, o SLA esperado e demonstrativo. Em producao, a disponibilidade dependeria de:

- multiplas zonas de disponibilidade;
- banco gerenciado com backup;
- replicas minimas por microservico;
- monitoramento e alertas;
- estrategia de rollback no CI/CD.

## Evidencias para entrega

- Print da estimativa na AWS Pricing Calculator.
- Print do AWS Budgets ou Cost Explorer, se usado.
- Texto explicando escolhas de menor custo.
