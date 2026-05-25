# Arquitetura

## Visao Geral

O projeto segue a estrutura do repositorio base `pma.261`: um repositorio principal agrega submodules de cada microservico, e a comunicacao externa passa pelo Gateway.

```mermaid
flowchart LR
    User["Usuario"] --> Site["Frontend Nginx"]
    Site --> Gateway["gateway-service"]

    Gateway --> Auth["auth-service"]
    Gateway --> Account["account-service"]
    Gateway --> Exchange["exchange-service"]
    Gateway --> Product["product-service"]
    Gateway --> Order["order-service"]

    Auth --> Account
    Account --> DB[("PostgreSQL")]
    Product --> DB
    Order --> DB
    Order --> Product
    Order --> Exchange
    Exchange --> AwesomeAPI["3rd-party API"]

    Prometheus --> Gateway
    Prometheus --> Exchange
    Prometheus --> Order
    Prometheus --> Product
    Grafana --> Prometheus
```

## Camada Confiavel

O usuario acessa apenas o frontend e o Gateway. O Gateway valida o cookie JWT com o Auth e repassa o identificador da conta para os servicos internos quando necessario.

## Fluxo autenticado de cotacao

```mermaid
sequenceDiagram
    participant C as Cliente
    participant G as Gateway
    participant A as Auth
    participant E as Exchange
    participant P as AwesomeAPI

    C->>G: GET /exchanges/USD/BRL
    G->>A: POST /auth/solve
    A-->>G: idAccount
    G->>E: GET /exchanges/USD/BRL + id-account
    E->>P: Busca cotacao atual
    P-->>E: Cotacao
    E-->>G: sell, buy, date, id-account
    G-->>C: 200 OK
```
