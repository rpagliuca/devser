---
title: "Minicurso UNIFUNEC: Introdução a Docker"
date: 2022-05-12T10:48:26-03:00
categories:
- DevOps
- Linux
---

Artigo de apoio ao minicurso "Introdução a Docker: desenvolvendo aplicações prontas para a nuvem".

* Data: 12/05/2022
* Local: UNIFUNEC, Santa Fé do Sul, SP - Brasil
* Duração do conteúdo: 130 minutos
* Cronograma:
    * 19h00: Etapas 1 a 8 (70 minutos)
    * 20h10: Intervalo (20 minutos)
    * 20h30: Etapas 9 a 13 (60 minutos)
    * 21h30: Encerramento do conteúdo programado. Suporte, solução de dúvidas e dicas até 22h00.

## Conteúdo

O minicurso é dividido em 13 etapas, com duração total estimada em 130 minutos.

### Etapa 1 - Cadastro de usuário no Docker Hub e GitHub (5 minutos)

Antes de começarmos com o curso, faça seu cadastro de usuário nas plataformas Docker Hub e GitHub. Vamos precisar usar essas 2 plataformas, e sem esses usuários é impossível prosseguir com o curso.

É importante fazer isso de imediato, pois às vezes os e-mails de confirmação de conta podem demorar alguns minutos até serem recebidos. Em caso de demora, não esqueça de verificar sua caixa de SPAM.

* Cadastro de usuário no Docker Hub: https://hub.docker.com
* Cadastro de usuário no GitHub: https://github.com

### Etapa 2 - Objetivos do minicurso (5 minutos)

Para ser acessível a uma grande diversidade da audiência do curso, composta por alunos com diferentes níveis de experiência, temos objetivos que vão do básico ao avançado. Se você é novo na área e tiver problemas em atingir os objetivos intermediários ou avançados, não se preocupe, isso é compreensível. Foque nos objetivos básicos, e o curso já terá sido proveitoso para você.

* Objetivo básico 1: obter familiaridade básica com ferramentas como Docker e Git, e entender seus propósitos na engenharia de software
* Objetivo básico 2: rodar uma aplicação simples (jogo do Mario) no Docker Playground usando modo imperativo
* Objetivo intermediário: rodar uma aplicação complexa (Wikipédia) no Docker Playground usando modo declarativo (Docker Compose)
* Objetivo avançado: construir uma imagem Docker (aplicação Docker) customizada utilizando GitHub Actions e rodá-la no Docker Playground

### Etapa 3 - Apresentação dos softwares e serviços que usaremos no minicurso (10 minutos)

* Git: software utilizado usualmente via terminal/console para controle de versão de projetos de software
* GitHub: aplicação web/ecossistema integrado ao Git para controle de versão de projetos de software, cooperação entre colaboradores, e pipelines automatizados
* Docker: software utilizado usualmente via terminal/console para criar e rodar aplicações contêneirizadas
* Docker Hub: repositório ("loja") de aplicativos Docker
* Docker Playground (https://labs.play-with-docker.com/): fornece um ambiente de laboratório virtual Linux com as principais ferramentas pré-instaladas: Docker + Docker Compose + Git

### Etapa 4 - Tópicos teóricos - Parte 1 (10 minutos)

* Apresentação do problema: "na minha máquina funciona"
* Apresentação da solução: contêinerização
* Docker no Windows (WSL2 ou Máquina Virtual) vs Docker no Linux

### Etapa 5 - Deploy de um jogo do Mario no Docker Playground (20 minutos)

* Comando: `docker run -d -p 8001:8080 pengbai/docker-supermario`

### Etapa 6 - Deploy de um jogo de Tetris no Docker Playground (7,5 minutos)

* Comando: `docker run -d -p 8002:80 uzyexe/tetris`

### Etapa 7 - Deploy de uma aplicação de "Lista de tarefas" no Docker Playground (7,5 minutos)

* Comando: `docker run -it -p 8003:8080 --name todo_list_app thoba/todo-list-app`

### Etapa 8 - Tópicos teóricos - Parte 2 (5 minutos)
* Docker Compose
* Diferentes modos de interagir com o Docker: manualmente vs de forma automatizada

### Etapa 9 - Deploy de uma aplicação complexa (chat) no Docker Playground (10 minutos)
* Esta aplicação possui ao mesmo tempo 2 contêineres rodando: 1 banco de dados + 1 webapp Javascript
* Página do projeto: https://hub.docker.com/r/ageapps/docker-chat
* Vamos rodá-la de modo imperativo (manual)
* Comando 1: `docker run -v "$(pwd)"/database:/data --name mongo_chat_db -d mongo mongod`
* Comando 2: `docker run -d --name node_chat_server -v "$(pwd)"/database:/data --link mongo_chat_db:db -p 8004:4000 ageapps/docker-chat`

### Etapa 10 - Deploy de uma aplicação complexa (Wikipédia) no Docker Playground (15 minutos)

* Esta aplicação inclui ao mesmo tempo 2 contêineres:
    * 1 banco de dados
    * 1 aplicação WEB em PHP
* Vamos rodá-la de modo declarativo (manifesto) com Docker Compose
* Instruções: https://hub.docker.com/r/bitnami/mediawiki/
* Adicionar env: `- MEDIAWIKI_HOST=<seu_host_aqui>` (para descobrir o seu host, abra o link de uma das aplicações que subimos anteriormente, remova o http:// e a barra final, e substitua a porta (exemplo: 8004) para 80)
* Rodar comando: `docker-compose up`

### Etapa 11 - Tópicos teóricos - Parte 2 (5 minutos)
* Docker não é a melhor solução para aplicações GUI Linux ou Windows
* Docker não é máquina virtual
* Docker é principalmente recomendado para aplicações WEB
* Plataformas que dão suporte a construir (build) aplicações/imagens Docker
    * Bitbucket (Bitbucket Pipelines)
    * Github (Github Actions)
    * GitLab
    * Travis CI
    * Jenkins
    * Circle CI
* Algumas plataformas que dão suporte a rodar/hospedar aplicações Docker
    * AWS (Elastic Beanstalk; Elastic Container Service)
    * Digital Ocean
    * Google Cloud
    * Kubernetes (em qualquer provedor)

### Etapa 12 - Build de aplicação customizada com Docker + GitHub Actions (20 minutos)
* Fork do repositório: https://github.com/rpagliuca/demo-github-actions-publish-to-docker-hub
* Configurar as secrets de USUÁRIO e SENHA do Docker Hub dentro do projeto do GitHub
* Comando: `git clone https://github.com/rpagliuca/demo-github-actions-publish-to-docker-hub.git`
* Alterar arquivo `.github/workflows/build-docker-image.yml` e atualizar o seu nome de usuário do Docker Hub
* Alterar o arquivo `app/main.go`, customizando a string da linha 29
* Commit e push das mudanças para o seu fork, com os comandos abaixo
* Comando 1: `git commit -am 'Customização'`
* Comando 2: `git push`

### Etapa 13 - Conclusão e dúvidas (10 minutos)
