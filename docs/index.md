# Projeto Exchange Store

## Grupo

Grupo: Gatchuscos

Integrantes:

- Gustavo Nicacio: Exchange API
- Vitor Kenzo Nishiwaki Fengler: Product API
- Gabriel Sodre da Costa: Order API

## Objetivo

Aplicacao web de compra e venda de produtos baseada em microservicos. O projeto integra Gateway, Auth, Account, Product, Order e Exchange, com banco PostgreSQL, observabilidade, CI/CD e manifests Kubernetes.

## Repositorios

| Repositorio | Papel |
| --- | --- |
| projeto-exchange | Repositorio principal com submodules |
| account | Contratos Java de Account |
| account-service | Cadastro e gerenciamento de contas |
| auth | Contratos Java de Auth |
| auth-service | Registro, login e JWT |
| gateway-service | API Gateway e camada confiavel |
| exchange-service | Cotacao de moedas em Python/FastAPI |
| product | Contratos Java de Product |
| product-service | Cadastro e consulta de produtos |
| order-service | Criacao e consulta de pedidos |
| site | Frontend estatico |

## Como rodar localmente

Backend:

```powershell
cd api
docker compose up -d --build
```

Frontend:

```powershell
cd web
docker compose up -d
```

URLs:

| Servico | URL |
| --- | --- |
| Site | http://localhost:8088 |
| Gateway | http://localhost:8080 |
| Prometheus | http://localhost:9090 |
| Grafana | http://localhost:3000 |
