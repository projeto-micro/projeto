# EKS

O projeto tem suporte a deploy no Kubernetes, com manifests já escritos para o `exchange-service` e o `postgres-service`.

## Estrutura dos manifests

Cada serviço que suporta K8s tem uma pasta `k8s/` com os seguintes recursos:

```
k8s/
├── k8s.yaml        # ConfigMap + Secret + Deployment + Service (em um arquivo só)
```

O PostgreSQL é separado em arquivos individuais:
```
postgres-service/k8s/
├── configmap.yaml
├── secrets.yaml
├── deployment.yaml
└── service.yaml
```

## PostgreSQL no Kubernetes

**ConfigMap** — configurações não sensíveis:
```yaml
POSTGRES_HOST:
POSTGRES_DB:
```

**Secret** — credenciais (base64):
```yaml
POSTGRES_USER:
POSTGRES_PASSWORD: 
```

**Deployment:**

| Campo | Valor |
|---|---|
| Imagem | `postgres:latest` |
| Porta | `5432` |
| CPU request/limit | `250m` / `500m` |
| Memória request/limit | `256Mi` / `512Mi` |

**Service:** `ClusterIP` na porta `5432` — acessível internamente como `postgres:5432`.

## Exchange Service no Kubernetes

**ConfigMap:**

| Variável | Valor padrão |
|---|---|
| `EXCHANGE_PROVIDER_URL` | URL da AwesomeAPI |
| `EXCHANGE_CACHE_TTL_SECONDS` | `60` |
| `EXCHANGE_REQUEST_TIMEOUT_SECONDS` | `5` |

**Deployment:**

| Campo | Valor |
|---|---|
| Imagem | `projetomicro/exchange:latest` |
| Porta | `8080` |
| CPU request/limit | `100m` / `500m` |
| Memória request/limit | `128Mi` / `256Mi` |
| `imagePullPolicy` | `Always` |

**Service:** `ClusterIP` na porta `8080` — acessível internamente como `exchange:8080`.

## Deploy

O deploy é feito automaticamente pelo Jenkins após o build:

```bash
kubectl apply -f k8s/k8s.yaml
```

Para aplicar manualmente:

```bash
# PostgreSQL
kubectl apply -f postgres-service/k8s/

# Exchange
kubectl apply -f exchange-service/k8s/k8s.yaml
```
