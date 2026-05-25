# Bottlenecks

## Implementados

| Bottleneck | Implementacao | Evidencia |
| --- | --- | --- |
| Caching | Cache em memoria no Exchange por par de moedas e TTL configuravel | Reduz chamadas repetidas para a API externa |
| Observability | Prometheus coleta metricas de Gateway, Exchange, Order e Product | `api/setup/prometheus/prometheus.yml` |
| Authentication & Authorization | Gateway valida JWT no Auth antes de liberar endpoints protegidos | Fluxo de login e cookie JWT |
| Load Balancing | Kubernetes Service distribui trafego entre pods do deployment | Manifests `k8s/k8s.yaml` |

## Destaques por integrante

| Integrante | Destaques |
| --- | --- |
| Gustavo Nicacio | Cache do Exchange; metricas Prometheus do Exchange |
| Vitor Fengler | Product API com persistencia; migrations Flyway |
| Gabriel Sodre | Order API integrada; comunicacao com Product e Exchange |

## Evidencias recomendadas

- Print do Prometheus targets.
- Print do Grafana.
- Logs mostrando cache hit/cache miss no Exchange, se habilitado.
- Video do HPA no teste de carga.
