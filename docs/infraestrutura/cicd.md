# CI/CD

## Objetivo

Automatizar build, publicacao de imagem Docker e deploy no Kubernetes.

## Pipelines

| Projeto | Jenkinsfile |
| --- | --- |
| account | `api/account/Jenkinsfile` |
| account-service | `api/account-service/Jenkinsfile` |
| auth | `api/auth/Jenkinsfile` |
| auth-service | `api/auth-service/Jenkinsfile` |
| exchange-service | `api/exchange-service/Jenkinsfile` |
| gateway-service | `api/gateway-service/Jenkinsfile` |
| order-service | `api/order-service/Jenkinsfile` |
| product | `api/product/Jenkinsfile` |
| product-service | `api/product-service/Jenkinsfile` |

## Estagios dos microservicos

1. `Dependencies`: instala dependencias ou compila contratos locais.
2. `Build`: compila o projeto e roda validacoes basicas.
3. `Build & Push Image`: cria imagem multi-arch e publica no Docker Hub.
4. `Deploy to K8s`: aplica os manifests no cluster com `kubectl apply`.

## Credenciais necessarias

- `dockerhub-credential`: usuario e token do Docker Hub.
- Credenciais AWS ou kubeconfig disponivel no agente Jenkins.

## Evidencias para entrega

- Print do pipeline executando.
- Print da imagem publicada no Docker Hub.
- Print do deploy no Kubernetes apos o pipeline.
