# PaaS

## Onde o grupo utilizou PaaS

| Plataforma/Servico | Uso |
| --- | --- |
| Amazon EKS | Kubernetes gerenciado para deploy dos microservicos |
| Docker Hub | Registro de imagens Docker usado pelo CI/CD |
| Jenkins | Automacao de build, push e deploy |
| Grafana | Visualizacao de metricas |
| Prometheus | Coleta de metricas |

## Justificativa

O uso de PaaS reduz o trabalho operacional do grupo. No caso do EKS, a AWS gerencia o plano de controle do Kubernetes. No Docker Hub, o grupo usa um registro pronto para distribuir imagens entre Jenkins e cluster. Grafana e Prometheus entram como camada operacional para observar gargalos e saude da aplicacao.

## Evidencias para entrega

- Prints dos servicos usados.
- Link das imagens no Docker Hub.
- Prints do cluster e dashboards.
