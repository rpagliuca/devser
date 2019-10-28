---
title: "Adicionando usuários a um cluster do Kubernetes"
date: 2019-10-08T13:27:39-03:00
categories: DevOps
---

## Parte 1 - Etapas a serem executadas pelo próprio usuário

### 1.1) Gerar um par de chaves RSA ou SSA

Se você já possui um par de chaves (certificados) que utiliza para fazer autenticação em servidores SSH,
GitHub, BitBucket, entre outros, provavelmente você pode pular diretamente para o passo 2, e usar
os certificados presentes na sua pasta `~/.ssh` (assumindo que você esteja utilizando o Linux).

Caso contrário, gere um par de chaves com o comando abaixo. Você pode trocar `meu_certificado` por um outro nome
de sua preferência.

{{<highlight bash>}}
ssh-keygen -f meu_certificado.key
{{</highlight>}}

### 1.2) Gerar uma requisição de assinatura de certificado

Para gerar uma requisição de assinatuar de certificado (CSR - Certificate Signing Request), rode
o comando abaixo passando o nome do certificado gerado no passo 1 (no nosso caso, `meu_certificado` é o nome da chave).

{{<highlight bash>}}
openssl req -new -key meu_certificado.key -out meu_certificado.csr -subj "/CN=usuario_kubernetes/O=grupo_kubernetes"
{{</highlight>}}

## Parte 2 - Etapas a serem executadas pelo administrador do cluster Kubernetes

### 2.1) Criar um CSR no Kubernetes

{{<highlight bash>}}
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
    name: meu_certificado.csr
spec:
  request: $(cat meu_certificado.csr | base64 | tr -d '\n')
  usages:
  - client auth
EOF
{{</highlight>}}

### 2.2) Aprovar certificado
{{<highlight bash>}}
kubectl certificate approve meu_certificado.csr
{{</highlight>}}

### 2.3) Extrair certificado assinado
{{<highlight bash>}}
kubectl get csr meu_certificado.csr -o jsonpath={.status.certificate} | base64 -d > meu_certificado.crt
{{</highlight>}}

### 2.4) Revisar certificado
{{<highlight bash>}}
openssl x509 -noout -text -in meu_certificado.crt
{{</highlight>}}

### 2.5) Criar ClusterRoleBinding
Esse é o passo final, que associa o usuário criado (no nosso exemplo, `usuario_kubernetes`) a um ClusterRole.
Aqui estamos assumindo que o ClusterRole já tenha sido criado, e se chama `developer`.

Arquivo yaml a ser aplicado (`ClusterRoleBinding.usuario_kubernetes.yaml`):

{{<highlight bash>}}
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: usuario_kubernetes
subjects:
- kind: User
  name: usuario_kubernetes
  namespace: default
roleRef:
  kind: ClusterRole
  name: developer
  apiGroup: rbac.authorization.k8s.io
{{</highlight>}}

{{<highlight bash>}}
kubectl apply -f ClusterRoleBinding.usuario_kubernetes.yaml
{{</highlight>}}
