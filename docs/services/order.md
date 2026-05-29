# Order API

**Responsável** -> Gabriel Sodré

**Repositório**: [Repositório order](https://github.com/projeto-micro/projeto-order.order-service.git) 

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

<iframe width="1030" height="554" src="https://www.youtube.com/embed/LxrGv-C8UaA" title="Order funcional" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
