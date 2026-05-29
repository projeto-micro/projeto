# Order API

**Responsável** -> Gabriel Sodré

**Repositório**: https://github.com/projeto-micro/projeto-order.order-service.git

Microservico Python/FastAPI responsavel pela criacao e consulta de pedidos.

## Responsabilidades

- Criar pedidos.
- Consultar pedidos.
- Consultar Product API para dados do produto.
- Consultar Exchange API para conversao de moeda.
- Persistir pedidos em PostgreSQL.

## Integracao

O Gateway roteia `/orders/**` para o servico `order`. O servico recebe o `id-account` da camada confiavel e usa esse identificador no registro do pedido.

## Evidencias tecnicas

- Codigo: `api/order-service`
- Dockerfile: `api/order-service/Dockerfile`
- Jenkinsfile: `api/order-service/Jenkinsfile`
- Kubernetes: `api/order-service/k8s/k8s.yaml`

# Vídeo funcionando

[Assista ao vídeo de demonstração](https://youtu.be/LxrGv-C8UaA)
