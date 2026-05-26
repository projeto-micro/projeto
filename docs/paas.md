# PaaS

O projeto usa duas camadas de plataforma: **Docker Compose** para desenvolvimento local e **Kubernetes (EKS)** para produção.

## Desenvolvimento local

Sobe todos os serviços com um único comando:

```bash
docker compose up --build -d
```

### Serviços do compose

| Serviço | Imagem | Porta exposta |
|---|---|---|
| `db` | `postgres:17` | `5432` |
| `account` | build local | — |
| `product` | build local | — |
| `auth` | build local | — |
| `exchange` | build local | — |
| `gateway` | build local | `8080` |
| `prometheus` | `prom/prometheus` | `9090` |
| `grafana` | `grafana/grafana-enterprise` | `3000` |

### Healthcheck do banco

O banco PostgreSQL tem healthcheck configurado — os serviços que dependem dele (`account`, `product`) só sobem após o banco estar pronto:

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
  interval: 5s
  timeout: 5s
  retries: 10
```

### Observabilidade local

- **Prometheus:** `http://localhost:9090`
- **Grafana:** `http://localhost:3000` (admin / admin)

O Prometheus coleta métricas de dois serviços:

| Job | Endpoint | Intervalo |
|---|---|---|
| `GatewayMetrics` | `gateway:8080/gateway/actuator/prometheus` | 1s |
| `ExchangeMetrics` | `exchange:8080/metrics` | 1s |

O Grafana já vem provisionado com o Prometheus como datasource padrão.

## Produção (Kubernetes)

Em produção, cada serviço roda como um `Deployment` com `Service` do tipo `ClusterIP`. As configurações sensíveis são gerenciadas via `Secret` do Kubernetes e as não-sensíveis via `ConfigMap`.

```bash
# Deploy de um serviço
kubectl apply -f k8s/k8s.yaml

# Verificar pods rodando
kubectl get pods

# Ver logs de um serviço
kubectl logs -l app=exchange
```
