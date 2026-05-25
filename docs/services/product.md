# Product API

Microservico Spring Boot responsavel pelo cadastro e consulta de produtos.

## Responsabilidades

- Criar produtos.
- Listar produtos.
- Consultar produto por identificador.
- Persistir dados em PostgreSQL.
- Criar estrutura de banco via Flyway.

## Integracao

O Gateway roteia `/products/**` para o servico `product`. O Order consulta o Product para validar e obter informacoes de produto antes de criar pedidos.

## Evidencias tecnicas

- Contratos: `api/product`
- Codigo: `api/product-service`
- Dockerfile: `api/product-service/Dockerfile`
- Jenkinsfile: `api/product-service/Jenkinsfile`
- Kubernetes: `api/product-service/k8s/k8s.yaml`
