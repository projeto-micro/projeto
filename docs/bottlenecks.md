# Bottlenecks

Para melhorar desempenho, disponibilidade e capacidade de diagnostico, o projeto implementou mitigacoes para os principais gargalos esperados em uma arquitetura de microservicos. A tabela abaixo compara os bottlenecks sugeridos na rubrica com o que foi implementado no projeto.

## Resumo

| Bottleneck | Status | Implementacao no projeto | Evidencia |
| --- | --- | --- | --- |
| Caching | Implementado | Cache em memoria no `exchange-service` por par de moedas, com TTL configuravel. | `api/exchange-service/app/exchange_service.py` |
| Observability | Implementado | Prometheus coleta metricas de Gateway, Exchange, Product e Order; Grafana consome Prometheus. | `api/setup/prometheus/prometheus.yml` e Grafana local |
| Load Balancing | Implementado | Kubernetes `Service` distribui trafego entre pods; Gateway e Site expostos por `LoadBalancer` no EKS. | `kubectl get svc`, manifests `k8s.yaml` |
| Authentication & Authorization | Implementado | Gateway valida cookie JWT via Auth e injeta `id-account` nas requisicoes internas. | Login pelo site, cookie `__store_jwt_token`, logs do Gateway |
| Messaging | Nao implementado | Nao houve fila RabbitMQ/Kafka. O fluxo atual e sincrono via HTTP entre Gateway, Product, Order e Exchange. | Fora de escopo para a entrega atual |
| Vulnerability Scanning | Nao implementado | Nao foi configurado SonarQube, OWASP ZAP, Snyk ou Trivy no pipeline final. | Fora de escopo para a entrega atual |
| Payment Processing | Nao aplicavel | O projeto nao processa pagamentos reais. | Fora do dominio funcional desta entrega |

## Caching

O `exchange-service` consulta APIs externas para obter cotacoes de moeda. Esse tipo de chamada e mais lento e sujeito a limites de quota, como ocorreu com a AwesomeAPI retornando `429 QuotaExceeded` durante os testes em AWS.

Para reduzir chamadas repetidas e melhorar a resiliencia, o Exchange usa cache em memoria por par de moedas.

Fluxo:

1. O usuario autenticado chama `GET /exchanges/{from}/{to}`.
2. O Exchange normaliza o par de moedas, por exemplo `USD-BRL`.
3. Se houver valor valido no cache, retorna imediatamente.
4. Se nao houver cache ou o TTL expirou, consulta o provedor externo.
5. A resposta e armazenada em memoria por um tempo configuravel.

Configuracao:

```yaml
EXCHANGE_CACHE_TTL_SECONDS: "60"
```

Beneficios:

- reduz latencia em consultas repetidas;
- diminui dependencia do provedor externo;
- ajuda a evitar limite de quota;
- melhora a experiencia do usuario no frontend e no Order API.

## Observability

O projeto utiliza Prometheus e Grafana para observar o comportamento dos servicos.

Componentes:

- Gateway com Actuator/Prometheus.
- Exchange com endpoint `/metrics`.
- Order com endpoint `/metrics`.
- Product com Actuator/Prometheus.
- Prometheus configurado para coletar targets.
- Grafana provisionado com datasource Prometheus.

Evidencias:

```text
api/setup/prometheus/prometheus.yml
api/setup/grafana/provisioning/datasources/datasources.yml
```

Comandos uteis:

```bash
kubectl get pods
kubectl logs deploy/gateway
kubectl logs deploy/exchange
```

No ambiente local:

```text
http://localhost:9090
http://localhost:3000
```

## Load Balancing

O projeto usa Kubernetes para distribuir trafego entre pods por meio de `Service`.

No EKS:

- `gateway` foi exposto com `Service` do tipo `LoadBalancer`;
- `site` foi exposto com `Service` do tipo `LoadBalancer`;
- servicos internos usam `ClusterIP`;
- o frontend Nginx encaminha chamadas `/api/*` para o Gateway interno.

Exemplo de evidencia:

```bash
kubectl get svc
kubectl get pods
```

O teste de carga deve ser feito sobre o Gateway, com HPA:

```bash
kubectl autoscale deployment gateway --cpu-percent=50 --min=1 --max=10
kubectl get hpa
```

Esse teste demonstra que o Kubernetes pode aumentar o numero de replicas do Gateway conforme o uso de CPU.

## Authentication & Authorization

O Gateway funciona como camada confiavel. Rotas abertas, como login e registro, passam direto. Rotas protegidas exigem cookie JWT.

Fluxo:

1. Usuario registra ou faz login pelo frontend.
2. Auth gera cookie `__store_jwt_token`.
3. Gateway valida o token chamando o Auth.
4. Auth retorna o identificador da conta.
5. Gateway injeta `id-account` antes de encaminhar para Exchange, Product ou Order.

Beneficios:

- centraliza verificacao de autenticacao;
- evita que servicos internos recebam requisicoes externas diretamente;
- permite rastrear a conta responsavel pela operacao;
- protege Exchange, Order e demais endpoints sensiveis.

## Bottlenecks Nao Implementados

### Messaging

Messaging com RabbitMQ ou Kafka nao foi implementado. O sistema atual usa comunicacao HTTP sincrona entre microservicos. Para implementar messaging corretamente, seria necessario:

- adicionar RabbitMQ ou Kafka no Compose e no Kubernetes;
- publicar eventos no Order API, como `OrderCreated`;
- criar consumidores para processar tarefas assincronas;
- testar reentrega, falhas e idempotencia.

Como o fluxo sincrono ja atende a entrega, messaging ficou fora do escopo para evitar complexidade extra no final do projeto.

### Vulnerability Scanning

Vulnerability scanning tambem nao foi implementado no pipeline final. Uma melhoria futura seria adicionar Trivy, Snyk ou OWASP ZAP no Jenkins/GitHub Actions.

Exemplo de melhoria futura:

```bash
trivy image gunicacio/gateway:latest
trivy image gunicacio/auth:latest
trivy image gunicacio/exchange:latest
```

### Payment Processing

Payment Processing nao se aplica ao projeto atual, pois a aplicacao cria produtos, pedidos e cotacoes, mas nao executa pagamentos reais.

## Destaques Por Integrante

| Integrante | Bottlenecks destacados |
| --- | --- |
| Gustavo Nicacio | Caching no Exchange; Observability do Exchange com metricas Prometheus |
| Vitor Fengler | Load balancing do Product via Kubernetes Service; Observability do Product via Actuator/Prometheus |
| Gabriel Sodre | Load balancing do Order via Kubernetes Service; Observability do Order via `/metrics` |
