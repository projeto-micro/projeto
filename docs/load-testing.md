# Testes de Carga

## Objetivo

O objetivo do teste de carga e demonstrar que a aplicacao suporta aumento de requisicoes e que o Kubernetes consegue escalar o `gateway` automaticamente usando HPA (Horizontal Pod Autoscaler).

A rubrica pede um video mostrando:

- o teste de carga em execucao;
- o HPA em execucao;
- o aumento de replicas quando a carga cresce.

## Ambiente

O teste deve ser executado com a aplicacao publicada no EKS.

Antes do teste, validar:

```bash
kubectl get nodes
kubectl get pods
kubectl get svc
```

Resultado esperado:

- todos os pods principais em `Running`;
- `gateway` exposto como `LoadBalancer`;
- `site` exposto como `LoadBalancer`, caso a demonstracao use o frontend.

## Endpoint de Teste

A rubrica recomenda testar:

```text
http://<dns-name>/info
```

No projeto, o endpoint deve apontar para o DNS externo do `gateway` no EKS. O DNS pode ser obtido com:

```bash
kubectl get svc gateway
```

Exemplo:

```text
http://a1028517a82d04b1fa07775fdad32085-1608226285.sa-east-1.elb.amazonaws.com/info
```

Antes de iniciar a carga, testar se o endpoint responde:

```bash
curl -i http://<dns-name>/info
```

Caso `/info` nao esteja disponivel, usar outro endpoint leve do Gateway para a gravacao ou adicionar o endpoint antes do teste.

## Criar HPA

Criar o autoscaler para o deployment `gateway`:

```bash
kubectl autoscale deployment gateway --cpu-percent=50 --min=1 --max=10
```

Verificar o HPA:

```bash
kubectl get hpa
```

Exemplo de saida esperada:

```text
NAME      REFERENCE            TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
gateway   Deployment/gateway   cpu: 1%/50%   1         10        1          66s
```

## Monitorar HPA e Pods

Em um terminal, monitorar o HPA:

```bash
watch -n 1 'kubectl get hpa'
```

Em outro terminal, monitorar os pods:

```bash
watch -n 1 'kubectl get pods'
```

No Windows PowerShell, caso `watch` nao esteja disponivel:

```powershell
while ($true) { kubectl get hpa; Start-Sleep -Seconds 1; Clear-Host }
```

E para pods:

```powershell
while ($true) { kubectl get pods; Start-Sleep -Seconds 1; Clear-Host }
```

## Executar Carga

### Opcao 1: hey

Se a ferramenta `hey` estiver instalada:

```bash
hey -z 3m -c 50 http://<dns-name>/info
```

Parametros:

- `-z 3m`: executa por 3 minutos;
- `-c 50`: usa 50 requisicoes concorrentes.

### Opcao 2: Pod Temporario no Kubernetes

Tambem e possivel gerar carga de dentro do cluster:

```bash
kubectl run load-generator --rm -it --image=busybox --restart=Never -- /bin/sh
```

Dentro do shell do pod:

```sh
while true; do wget -q -O- http://gateway/info; done
```

Essa opcao e util quando a maquina local nao tem ferramentas de carga instaladas.

## Evidencias Para o Video

Durante a gravacao, mostrar:

1. `kubectl get pods` com todos os servicos rodando.
2. `kubectl get svc gateway` com o DNS externo.
3. O endpoint `/info` respondendo.
4. `kubectl get hpa` antes da carga.
5. O comando de carga em execucao.
6. O HPA durante a carga.
7. A quantidade de pods do `gateway` aumentando, se houver carga suficiente.

## Encerrar o Teste

Ao final da gravacao, remover o HPA:

```bash
kubectl delete hpa gateway
```

Opcionalmente, voltar o Gateway para uma replica:

```bash
kubectl scale deployment gateway --replicas=1
```

Conferir o estado final:

```bash
kubectl get hpa
kubectl get pods
```

## Observacoes

- O HPA depende de metricas de CPU no cluster. Se `kubectl get hpa` mostrar `unknown`, verificar se o Metrics Server esta instalado no EKS.
- Em clusters pequenos, a escalabilidade pode ser limitada pela capacidade dos nodes.
- Para a entrega, o mais importante e mostrar o teste de carga rodando e o HPA monitorando o `gateway`.
