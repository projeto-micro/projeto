# Arquitetura

## Visão Geral

O sistema segue o padrão de **API Gateway + Microserviços**. Todo o tráfego externo passa pelo gateway, que roteia para os serviços internos via hostname Docker/Kubernetes.

```mermaid
graph TD
    Client([Cliente]) --> GW[Gateway :8080]

    GW -->|/auth/**| Auth[auth-service :8080]
    GW -->|/accounts/**| Account[account-service :8080]
    GW -->|/products/**| Product[product-service :8082]
    GW -->|/exchanges/**| Exchange[exchange-service :8080]

    Auth -->|Feign| Account
    Account --> DB[(PostgreSQL)]
    Product --> DB
```

## Serviços

| Serviço | Linguagem | Porta | Responsabilidade |
|---|---|---|---|
| `gateway-service` | Java / Spring Cloud Gateway | 8080 | Roteamento, CORS |
| `auth-service` | Java / Spring Boot | 8080 | Autenticação JWT |
| `account-service` | Java / Spring Boot | 8080 | Gestão de usuários |
| `product-service` | Java / Spring Boot | 8082 | Catálogo de produtos |
| `exchange-service` | Python / FastAPI | 8080 | Conversão de moedas |

## Comunicação entre serviços

- **Externo → Gateway**: HTTP REST via porta `8080`
- **Gateway → Serviços**: roteamento por path via Spring Cloud Gateway
- **Auth → Account**: comunicação interna via **OpenFeign**
- **Banco de dados**: `account-service` e `product-service` conectam ao PostgreSQL via Flyway

## Padrão de contrato

Os serviços Java utilizam um módulo de contrato separado (`account/`, `product/`, `auth/`) que define as interfaces Feign e DTOs compartilhados entre serviços. Isso garante consistência nas chamadas internas.

```
api/
├── account/           # Contrato: interface Feign + DTOs
├── account-service/   # Implementação
├── product/           # Contrato: interface + DTOs
├── product-service/   # Implementação
├── auth/              # Contrato: interface Feign + DTOs
├── auth-service/      # Implementação
├── exchange-service/  # Serviço Python (sem contrato separado)
└── gateway-service/   # Gateway
```

## Autenticação

```mermaid
sequenceDiagram
    participant C as Cliente
    participant GW as Gateway
    participant Auth as auth-service
    participant Acc as account-service

    C->>GW: POST /auth/login
    GW->>Auth: POST /auth/login
    Auth->>Acc: POST /accounts/login (Feign)
    Acc-->>Auth: AccountOut
    Auth-->>GW: Set-Cookie: __store_jwt_token
    GW-->>C: 200 OK + Cookie JWT
```

O token JWT é armazenado em cookie HTTP-only (`__store_jwt_token`). O gateway injeta o header `id-account` nas requisições autenticadas para que os serviços downstream saibam o usuário logado.

## Observabilidade

```mermaid
graph LR
    GW[Gateway] -->|/gateway/actuator/prometheus| Prometheus
    Exchange[Exchange] -->|/metrics| Prometheus
    Prometheus --> Grafana
```

- **Prometheus** coleta métricas a cada 1 segundo
- **Grafana** exibe dashboards em `http://localhost:3000`
- **Prometheus UI** disponível em `http://localhost:9090`
