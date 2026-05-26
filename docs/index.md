# Projeto Microservice

Bem-vindo à documentação do **Store API**, um sistema de e-commerce construído com arquitetura de microserviços usando Spring Boot.

## Visão Geral

O projeto é composto por serviços independentes que se comunicam via HTTP, orquestrados por um API Gateway e containerizados com Docker.

## Serviços

| Serviço | Porta interna | Descrição |
|---|---|---|
| `gateway-service` | 8080 | Ponto de entrada único, roteamento e CORS |
| `auth-service` | 8080 | Autenticação JWT |
| `account-service` | 8080 | Gestão de usuários |
| `product-service` | 8082 | Catálogo de produtos |
| `exchange-service` | 8080 | Conversão de moedas |

## Como rodar

### Pré-requisitos

- Docker e Docker Compose instalados
- Maven (para compilar os serviços localmente)

### Configuração

Crie um arquivo `.env` na pasta `api/` com base no `.env.example`:

```dotenv
VOLUME_DB=./volume/db
DB_USER=root
DB_PASSWORD=sua_senha
DB_NAME=store
CORS_ALLOWED_ORIGINS=http://localhost,http://localhost:80
CORS_ALLOWED_CREDENTIALS=true
JWT_SECRET_KEY=sua_chave_jwt
JWT_HTTP_ONLY=true
```

### Subindo o ambiente

```bash
docker compose up --build -d
```

### Derrubando o ambiente

```bash
docker compose down -v
```

## Tecnologias

- **Java 25** + **Spring Boot 4**
- **PostgreSQL 17**
- **Flyway** — migrations de banco
- **Spring Cloud Gateway** — roteamento
- **JWT** — autenticação stateless
- **Docker Compose** — orquestração local
- **Prometheus + Grafana** — observabilidade
