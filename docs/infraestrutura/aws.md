# AWS

## Objetivo

Preparar a conta AWS usada para hospedar a aplicacao no EKS e registrar as evidencias da configuracao.

## Roadmap da configuracao

1. Criar ou acessar a conta AWS do grupo.
2. Criar usuario IAM para o projeto.
3. Criar access key para uso local e no Jenkins.
4. Instalar e configurar AWS CLI.
5. Validar acesso com `aws sts get-caller-identity`.

## Comandos de apoio

```bash
aws configure
aws sts get-caller-identity
aws configure list
```

## Evidencias para entrega

- Print do usuario IAM sem expor secrets.
- Print ou saida do `aws sts get-caller-identity`.
- Regiao escolhida para o projeto.
- Politicas anexadas ao usuario ou role do projeto.

## Observacao de seguranca

Access keys nao devem ser commitadas. No Jenkins, as credenciais devem ser cadastradas no gerenciador de credenciais e consumidas por `withCredentials`.
