# Exchange API

Microservico em Python/FastAPI responsavel por buscar cotacoes entre moedas.

## Endpoint principal

```http
GET /exchanges/{from}/{to}
```

Exemplo:

```http
GET /exchanges/USD/BRL
```

Resposta:

```json
{
  "sell": 5.71,
  "buy": 5.70,
  "date": "2026-05-09 14:23:42",
  "id-account": "0195ae95-5be7-7dd3-b35d-7a7d87c404fb"
}
```

## Requisitos atendidos

- Implementado em Python.
- Usa FastAPI.
- Consome servico externo de cotacao.
- Exige usuario autenticado via Gateway.
- Usa cache em memoria com TTL para reduzir chamadas repetidas.
- Expoe metricas para Prometheus.

## Evidencias tecnicas

- Codigo: `api/exchange-service`
- Dockerfile: `api/exchange-service/Dockerfile`
- Jenkinsfile: `api/exchange-service/Jenkinsfile`
- Kubernetes: `api/exchange-service/k8s/k8s.yaml`
