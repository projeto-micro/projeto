# projeto-exchange

Full-stack microservices project based on `pma.261`, adding a Python/FastAPI Exchange API to the existing Gateway, Auth and Account structure.

## Repository Structure

```text
projeto-exchange/
├── api/
│   ├── account/             # Shared Java Feign client library
│   ├── account-service/     # Spring Boot account microservice
│   ├── auth/                # Shared Java Feign client library
│   ├── auth-service/        # Spring Boot auth/JWT microservice
│   ├── exchange-service/    # Python FastAPI exchange microservice
│   ├── gateway-service/     # Spring Cloud Gateway
│   ├── postgres-service/    # Kubernetes example for PostgreSQL
│   ├── setup/               # Prometheus and Grafana setup
│   └── compose.yaml
├── web/
│   ├── site/                # Static frontend served by Nginx
│   └── compose.yaml
└── jenkins/
    └── compose.yaml
```

## Running Locally

Backend:

```bash
cd api/
docker compose up -d --build
```

Frontend:

```bash
cd web/
docker compose up -d
```

The frontend runs at `http://localhost` and the gateway runs at `http://localhost:8080`.
