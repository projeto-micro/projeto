# Testes de Carga

## Objetivo

Validar que o Gateway suporta aumento de carga e que o Kubernetes escala pods com HPA.

## Endpoint de teste

O endpoint recomendado pela rubrica e:

```text
http://<dns-name>/info
```

Caso o endpoint `/info` nao esteja habilitado no Gateway no momento do teste, usar um endpoint leve autenticado ou adicionar `/info` antes da gravacao.

## Criar HPA

```bash
kubectl autoscale deployment gateway --cpu-percent=50 --min=1 --max=10
kubectl get hpa
watch -n 1 'kubectl get hpa'
```

## Executar carga

Exemplo com `hey`:

```bash
hey -z 3m -c 50 http://<dns-name>/info
```

Exemplo com pod temporario:

```bash
kubectl run load-generator --rm -it --image=busybox --restart=Never -- /bin/sh
while true; do wget -q -O- http://gateway/info; done
```

## Encerrar

```bash
kubectl delete hpa gateway
```

## Evidencias para entrega

- Video do teste de carga em execucao.
- Video ou print do `kubectl get hpa`.
- Video ou print do aumento de replicas do deployment `gateway`.
