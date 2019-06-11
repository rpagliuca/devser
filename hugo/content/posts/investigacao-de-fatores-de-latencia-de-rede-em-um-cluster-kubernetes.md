---
title: "Investigação de fatores de latência de rede em um cluster Kubernetes"
date: 2019-06-11T09:44:39-03:00
draft: false
categories: Devops
aliases:
- /posts/investigacao-da-latencia-do-nosso-cluster-do-kubernetes
---

Realizei testes para investigar a latência de rede do cluster Kubernetes da minha empresa.

Eu gostaria de responder algumas perguntas:

* Se eu acessar diretamente o nó que está rodando o pod, a latência será menor do que se eu acessar o mesmo serviço utilizando como bridge um segundo nó da rede?
* E se eu colocar um load balancer distribuindo a carga entre todos os nós da rede? Qual é o aumento na latência?
* E por último, qual é o aumento de latência que o serviço Ambassador (https://www.getambassador.io) adiciona aos requests?

Para responder essas e outras perguntas, rodei os seguintes testes:

1. Acesso ao serviço diretamente pelo nó que está rodando o pod
2. Acesso ao serviço utilizando um nó que não contém o pod como bridge
3. Acesso via load balancer atrelado diretamente ao serviço
4. Acesso via load balancer atrelado ao ambassador
5. Acesso via load balancer atrelado ao ambassador com HTTPS

Para realizar esses testes, configurei um *service* e um *deployment* no meu cluster com a imagem nginxdemos/hello:plain-text (https://hub.docker.com/r/nginxdemos/hello/).

### Teste 1) Acesso direto ao nó contendo o pod

Para realizar esse teste, depois de subir o deployment no namespace `labs`, rodei os seguintes comandos:
```
$ kubectl get pods -n labs -o wide
NAME                                    READY   STATUS    RESTARTS   AGE   IP             NODE
# ... informações de outros pods omitidas ...
hello-nginx-plaintext-7bc68d7db-vlbb2   1/1     Running       0          14d   100.96.67.21   ip-172-20-58-134.ec2.internal
```

Rodei também o seguinte comando:
```
$ kubectl get nodes -o wide
NAME                            STATUS   ROLES    AGE   VERSION    EXTERNAL-IP      OS-IMAGE                      KERNEL-VERSION   CONTAINER-RUNTIME
# ... informações de outros nós omitidas ...
ip-172-20-58-134.ec2.internal   Ready    node     18d   v1.10.11   54.91.3.252      Debian GNU/Linux 8 (jessie)   4.4.121-k8s      docker://17.3.2
```

Juntando as informações dos dois comandos acima, eu determinei que o IP externo do nó onde está rodando o pod hello-nginx-plaintext-7bc68d7db-zqjls é 54.91.3.252.

Para acessar diretamente o nó, sem intermediários, eu configurei o service com o tipo NodePort, na porta 30193:

```
apiVersion: v1
kind: Service
metadata:
  name: hello-nginx-plaintext
  namespace: labs
spec:
  selector:
    project: hello-nginx-plaintext
    type: http-server
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30193
```

Depois de configurar o Security Groups da instância EC2 dentro do painel da AWS para permitir conexões Inbound na porta 30193, eu estava pronto para rodar o comando abaixo:

```
$ (for i in {1..30}; do time curl 54.91.3.252:30193; done) 2>&1 | grep real | awk '{ print $2 }'
0m0.446s
0m0.409s
0m0.410s
0m0.923s
0m0.408s
0m0.408s
0m0.411s
0m0.410s
0m0.409s
0m0.415s
0m0.337s
0m0.290s
0m0.391s
0m0.410s
0m0.299s
0m0.417s
0m0.416s
0m0.436s
0m0.479s
0m0.410s
0m0.409s
0m0.410s
0m0.409s
0m0.348s
0m0.369s
0m0.409s
0m0.408s
0m0.411s
0m0.410s
0m1.301s
```

* Média: 447.3ms
* Desvio padrão: 191.2ms

### Teste 2) Acesso a um nó que não contém o pod

Segui quase os mesmos passos do teste anterior, mas dessa vez peguei o IP externo de um outro nó qualquer do cluster, diferente daquele que está hospedando o pod.

Rodei o comando novamente:
```
$ kubectl get nodes -o wide
NAME                            STATUS   ROLES    AGE   VERSION    EXTERNAL-IP      OS-IMAGE                      KERNEL-VERSION   CONTAINER-RUNTIME
# ... informações de outros nós omitidas ...
ip-172-20-56-156.ec2.internal   Ready    node     1h    v1.10.11   3.89.105.70      Debian GNU/Linux 8 (jessie)   4.4.121-k8s      docker://17.3.2
```

Rodei novamente o curl, dessa vez apontando para o IP 3.89.105.70.
```
$ (for i in {1..30}; do time curl 3.89.105.70:30193; done) 2>&1 | grep real | awk '{ print $2 }'
0m0.368s
0m0.411s
0m0.406s
0m0.411s
0m0.409s
0m0.410s
0m0.410s
0m0.409s
0m0.297s
0m0.420s
0m0.410s
0m0.409s
0m0.410s
0m0.306s
0m0.410s
0m0.512s
0m0.407s
0m0.411s
0m1.388s
0m0.454s
0m0.410s
0m0.410s
0m0.307s
0m0.347s
0m0.370s
0m0.409s
0m0.410s
0m0.408s
0m0.410s
0m0.410s
```
* Média: 432.0ms
* Desvio padrão: 185.3ms

### Teste 3) Acesso via load balancer atrelado diretamente ao serviço                                                             

Para fazer esse teste, alterei o service para o tipo LoadBalancer. Assim, o IP do LoadBalancer da AWS
apontará para todos os nós do meu cluster, e balanceará os requests entre todos os nós.

```
apiVersion: v1
kind: Service
metadata:
  name: hello-nginx-plaintext-2
  namespace: labs
spec:
  type: LoadBalancer
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
  selector:
    project: hello-nginx-plaintext
    type: http-server
```

Depois de criado o serviço, obtive o DNS do load balancer:

```
$ kubectl get svc -n labs
hello-nginx-plaintext-3   LoadBalancer   100.67.114.224   acf840d7a8c4811e990020aec1ba06c5-1407280343.us-east-1.elb.amazonaws.com   80:32714/TCP   1m
```

Rodei novamente o curl, agora para o destino acf840d7a8c4811e990020aec1ba06c5-1407280343.us-east-1.elb.amazonaws.com:

```
$ (for i in {1..30}; do time curl acf840d7a8c4811e990020aec1ba06c5-1407280343.us-east-1.elb.amazonaws.com; done) 2>&1 | grep real | awk '{ print $2 }'
0m0.388s
0m0.408s
0m0.409s
0m0.409s
0m0.410s
0m0.410s
0m0.407s
0m0.316s
0m0.402s
0m0.409s
0m0.396s
0m0.424s
0m0.408s
0m0.411s
0m0.409s
0m0.407s
0m0.411s
0m0.410s
0m0.410s
0m0.408s
0m1.436s
0m0.406s
0m0.413s
0m0.409s
0m0.409s
0m0.410s
0m0.410s
0m0.409s
0m0.408s
0m0.410s
```

* Média: 439.4ms
* Desvio padrão: 189.1ms

Esse resultado mostra que o LoadBalancer não aumenta a latência de modo significativo.

### Teste 4) Acesso via load balancer atrelado ao ambassador

Agora, configuramos o serviço Ambassador (https://www.getambassador.io/) em nosso cluster,
e expusemos o serviço do Ambassador via load balancer (assim como no Teste 3).

Sendo assim, temos agora o seguinte fluxo:
Cliente -> Load Balancer -> Ambassador -> Serviço hello-nginx-plaintext

A configuração do serviço hello-nginx-plaintext ficou assim:

```
apiVersion: v1
kind: Service
metadata:
  name: hello-nginx-plaintext
  namespace: labs
  annotations:
    getambassador.io/config: |
      ---
      apiVersion: ambassador/v0
      kind: Mapping
      name: hello-nginx-plaintext-mapping
      host: hello-nginx-plaintext.lb1.tecnologiafox.com.br
      prefix: /
      service: hello-nginx-plaintext.labs
spec:
  selector:
    project: hello-nginx-plaintext
    type: http-server
  ports:
  - port: 80
    targetPort: 80
```

A configuração do serviço Ambassador foge do escopo deste artigo, mas pode ser
consultada em https://www.getambassador.io/.

Agora o curl terá como alvo o domínio hello-nginx-plaintext.lb1.tecnologiafox.com.br.

```
$ (for i in {1..30}; do time curl hello-nginx-plaintext.lb1.tecnologiafox.com.br; done) 2>&1 | grep real | awk '{ print $2 }'
0m0.423s
0m0.310s
0m0.406s
0m0.410s
0m0.409s
0m0.411s
0m0.409s
0m0.408s
0m0.352s
0m0.316s
0m0.357s
0m0.409s
0m0.410s
0m0.409s
0m0.375s
0m0.444s
0m0.407s
0m0.412s
0m0.409s
0m0.408s
0m0.411s
0m2.458s
0m0.306s
0m0.410s
0m0.310s
0m0.408s
0m0.409s
0m0.410s
0m0.409s
0m0.444s
```
* Média: 462.3ms
* Desvio padrão: 378.83ms

### Teste 5) Acesso via load balancer atrelado ao ambassador com HTTPS

Nosso Ambassador está configurado com Let's Encrypt para permitir conexões HTTPS. Sendo assim, fizemos
teste similar ao Teste 4, mas agora apontando para a URL https://hello-nginx-plaintext.lb1.tecnologiafox.com.br:

```
$ (for i in {1..30}; do time curl https://hello-nginx-plaintext.lb1.tecnologiafox.com.br; done) 2>&1 | grep real | awk '{ print $2 }'
0m0.853s
0m0.715s
0m0.921s
0m0.813s
0m0.824s
0m0.818s
0m0.822s
0m0.820s
0m0.819s
0m0.819s
0m0.813s
0m1.336s
0m0.920s
0m0.820s
0m0.819s
0m0.921s
0m1.229s
0m0.819s
0m0.767s
0m0.763s
0m0.821s
0m0.720s
0m0.835s
0m0.859s
0m0.866s
0m2.048s
0m0.821s
0m0.812s
0m0.825s
0m0.717s
```

* Média: 891.8ms
* Desvio padrão: 253.3ms

### Conclusões

Considerando o desvio padrão, não tivemos mudanças significativas dos testes 1 ao 4. Esse é um resultado positivo,
que reforça a impressão de que o LoadBalancer da AWS e também o serviço Ambassador não aumentam, de forma significativa,
a latência do nosso cluster Kubernetes.

Por outro lado, o teste 5 mostra que utilizar HTTPS em vez de HTTP aumenta consideravelmente (mais de 400ms) a latência
do request. Mas isso já é um fato conhecido, e não trouxe nenhuma surpresa. Lembremos, ainda, que os clientes
HTTPS normalmente utilizam a funcionalidade *Keep-Alive* do protocolo, para preservar o *handshake* e evitar
grandes latências no cenário de múltiplas chamadas subsequentes.
