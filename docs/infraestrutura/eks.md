# EKS

## Objetivo

Executar todos os microservicos no mesmo cluster Kubernetes gerenciado pela AWS.

## Recursos Kubernetes do projeto

| Servico | Manifesto |
| --- | --- |
| Postgres | `api/postgres-service/k8s/*.yaml` |
| Account | `api/account-service/k8s/k8s.yaml` |
| Auth | `api/auth-service/k8s/k8s.yaml` |
| Gateway | `api/gateway-service/k8s/k8s.yaml` |
| Exchange | `api/exchange-service/k8s/k8s.yaml` |
| Product | `api/product-service/k8s/k8s.yaml` |
| Order | `api/order-service/k8s/k8s.yaml` |

Cada microservico possui:

- `ConfigMap`
- `Secret`
- `Deployment`
- `Service`

## Aplicando no cluster

```bash
kubectl apply -f api/postgres-service/k8s/
kubectl apply -f api/account-service/k8s/k8s.yaml
kubectl apply -f api/auth-service/k8s/k8s.yaml
kubectl apply -f api/exchange-service/k8s/k8s.yaml
kubectl apply -f api/product-service/k8s/k8s.yaml
kubectl apply -f api/order-service/k8s/k8s.yaml
kubectl apply -f api/gateway-service/k8s/k8s.yaml
```

## Validacao

```bash
kubectl get pods
kubectl get services
kubectl logs deploy/gateway
kubectl logs deploy/exchange
```

O `gateway` usa `Service` do tipo `LoadBalancer`, expondo a aplicacao para os testes externos.

## Evidencias para entrega

- Print de `kubectl get pods`.
- Print de `kubectl get services`.
- DNS ou endpoint externo do LoadBalancer do Gateway.
- Video curto mostrando a aplicacao respondendo pelo endpoint do cluster.
