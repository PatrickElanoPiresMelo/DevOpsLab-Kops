# DevOpsLab-Kubernetes
Laboratório de Construção de um Cluster de Kubernetes com Deploy de um WordPress

## Pré-Requisitos

Para rodar essa receita de Ansible para Provisionar um Cluster de Kubernetes via Kops e realizar deploy de um Wordpress você precisará:
- Ter uma conta na AWS
- Criar uma máquina "nano" para servir de Bastion no provisionamento da Infra
- Instalar o Ansible na máquina Bastion ( # apt-get install ansible )
- Criar um Route53 com a zona interna que será usada no Kubernetes
- Gerar credenciais da AWS para que o Kops possa provisionar os Objetos

## Provisionando um Cluster de Kubernetes 

1. Após baixar o projeto você precisará preencher o arquivo de variáveis com suas credenciais, nome do Bucket S3 que será criado para guardar o estado do Cluster, a Zona na AWS e o Domínio interno já cadastrado no Route53:

```
$ cat group_vars/all 
---
# Kops e Kubectl Version
kops_version: 1.9.1
kubectl_version: v1.10.4

# AWS Credentials
id: 
key: 

# AWS S3 State Kops
bucket: 

# Kops Configuration
zone: 
domain: 
```
2.  Na sequência basta rodar a playbook:

```
$ ansible-playbook site.yml 
```

## Destruindo o Cluster 

Caso deseja excluir a infra provisionada basta executar:

```
$ kops delete cluster --name {{ domain }} --yes
```
* O comando acima não irá excluir o Route53 e o Bucket S3, portanto você precisará apagar manualmente.

