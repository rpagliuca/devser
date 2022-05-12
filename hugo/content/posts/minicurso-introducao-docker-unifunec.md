---
title: "Minicurso UNIFUNEC: Introdução a Docker"
date: 2022-05-12T10:48:26-03:00
categories:
- DevOps
- Linux
aliases:
- /posts/minicurso-introdução-docker-unifunec.md
---

Artigo de apoio ao minicurso "Introdução a Docker: desenvolvendo aplicações prontas para a nuvem".

* Data: 12/05/2022
* Local: UNIFUNEC, Santa Fé do Sul, SP - Brasil
* Conteúdo total: 130 minutos
* Duração total: 180 minutos (130 minutos de conteúdo + 20 minutos de intervalo + 30 minutos de suporte e dúvidas)
* Cronograma:
    * 19h00: Etapas 1 a 8 (70 minutos)
    * 20h10: Intervalo (20 minutos)
    * 20h30: Etapas 9 a 13 (60 minutos)
    * 21h30: Encerramento do conteúdo programado. Suporte, solução de dúvidas e dicas até 22h00.

## Conteúdo

O minicurso é dividido em 13 etapas, com conteúdo total estimado em 130 minutos.

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
* Docker: software utilizado usualmente via terminal/console para criar e rodar aplicações contêinerizadas
* Docker Hub: repositório ("loja") de aplicativos Docker
* Docker Playground - https://labs.play-with-docker.com/ : fornece um ambiente de laboratório virtual Linux com as principais ferramentas pré-instaladas: Docker + Docker Compose + Git

### Etapa 4 - Tópicos teóricos - Parte 1 (10 minutos)

* Apresentação do problema: "na minha máquina funciona"
* Apresentação da solução: contêinerização
* Docker no Windows (WSL2 ou Máquina Virtual) vs Docker no Linux

### Etapa 5 - Deploy de um jogo do Mario no Docker Playground (20 minutos)

* Acesse o Docker Playground no endereço https://labs.play-with-docker.com/
* Clique em "Login"
* Utilize seu usuário e senha cadastrado no "Docker Hub" anteriormente
* Clique em "Start"
* Após aparecer a contagem regressiva de 4 horas, clique em "+ ADD NEW INSTANCE"
* Rode o comando a seguir no terminal da instância criada: `docker run -d -p 8001:8080 pengbai/docker-supermario`
* Dica: para colocar um comando dentro do terminal, utilize o botão do meio do mouse, ou então utilize o atalho `SHIFT + INSERT`
* Aguarde alguns instantes até que a aplicação `docker-supermario` termine de carregar
* Para abrir a aplicação, clique no botão `8001` que irá aparecer ao lado de `OPEN PORT`
* Utilize as teclas `S`, `A` e `setas` para jogar
* Parabéns! Você concluiu um dos objetivos do minicurso!

### Etapa 6 - Deploy de um jogo de Tetris no Docker Playground (7,5 minutos)

* Após finalizar os passos da Etapa 5, rode o comando abaixo (pode usar a mesma instância, ou criar uma nova se preferir clicando em `+ ADD NEW INSTANCE`)
* Comando: `docker run -d -p 8002:80 uzyexe/tetris`
* Para abrir a aplicação, clique no botão `8002` que irá aparecer ao lado de `OPEN PORT`
* Utilize as teclas `Espaço`, e `setas` para jogar

### Etapa 7 - Deploy de uma aplicação de "Lista de tarefas" no Docker Playground (7,5 minutos)

* Após finalizar as etapas anteriores, rode o comando abaixo na mesma instância, ou se preferir crie uma nova clicando em `+ ADD NEW INSTANCE`
* Comando: `docker run -d -p 8003:8080 thoba/todo-list-app`
* Para abrir a aplicação, clique no botão `8003` que irá aparecer ao lado de `OPEN PORT`

### Etapa 8 - Tópicos teóricos - Parte 2 (5 minutos)
* Docker Compose
* Diferentes modos de interagir com o Docker: manualmente vs de forma automatizada

### << Intervalo de 20 minutos >>

### Etapa 9 - Deploy de uma aplicação complexa (chat) no Docker Playground (10 minutos)
* Eu considero esta aplicação como **complexa** por envolver múltiplos contêineres conversando entre si. Esta aplicação possui ao mesmo tempo 2 contêineres rodando:
    * 1 banco de dados MongoDB
    * 1 webapp Javascript
* Página do projeto: https://hub.docker.com/r/ageapps/docker-chat
* Vamos rodá-la de modo imperativo (manual)
* Rodar comando 1: `docker run -v "$(pwd)"/database:/data --name mongo_chat_db -d mongo mongod`
* Rodar comando 2: `docker run -d --name node_chat_server -v "$(pwd)"/database:/data --link mongo_chat_db:db -p 8004:4000 ageapps/docker-chat`
* Para abrir a aplicação, clique no botão `8004` que irá aparecer ao lado de `OPEN PORT`


### Etapa 10 - Deploy de uma aplicação complexa (Wikipédia) no Docker Playground (15 minutos)

* Eu também considero esta aplicação como **complexa** por envolver múltiplos contêineres se comunidando entre si. Esta aplicação inclui ao mesmo tempo 2 contêineres:
    * 1 banco de dados MariaDB (similar ao MySQL)
    * 1 aplicação WEB em PHP
* Vamos rodá-la de modo declarativo (manifesto) com Docker Compose
* Abra uma nova instância no Docker Playground
* Rodar comando 1: `curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-mediawiki/master/docker-compose.yml > docker-compose.yml; echo "" >> docker-compose.yml`
* Após rodar o comando 1, clique em `EDITOR`
* Clique no arquivo `docker-compose.yml` 
* Logo abaixo da linha `- ALLOW_EMPTY_PASSWORD=yes`, adicione a linha `- MEDIAWIKI_HOST=<seu_host_aqui>`
* Para descobrir o seu host e substituir na parte <seu_host_aqui>, clique no botão `OPEN PORT` e digite `80`. Em seguida, copie a URL da nova janela e cole no editor. Depois, remova o trecho "http://" e, por último, remova a barra final do endereço, se existir.
* Confirme que o seu host inicia com `ip` e termina com `.com`
* Clique no botão `SAVE`. Em seguida, feche a janela de edição de arquivo
* Rodar comando 2: `docker-compose up -d`
* Para abrir a aplicação, clique no botão `80` que irá aparecer ao lado de `OPEN PORT`
* Atenção: na primeira vez que a Wiki é carregada, é preciso atualizar a página algumas vezes até que esteja disponível para uso. Em caso de dificuldades, tente clicar com o botão direito do mouse no botão `80` e escolher `ABRIR EM ABA ANÔNIMA` (ou opção similar).
* Parabéns! Você concluiu mais um objetivo deste minicurso!

### Etapa 11 - Tópicos teóricos - Parte 2 (5 minutos)
* Docker não é a melhor solução para aplicações GUI Linux ou Windows
* Docker não é máquina virtual
* Docker é principalmente recomendado para aplicações WEB
* Aplicação web monolítica versus microsserviços

### Etapa 12 - Build de aplicação customizada com Docker + GitHub Actions (20 minutos)

#### Parte 1 - Fork do repositório
* Fork do repositório: https://github.com/rpagliuca/demo-github-actions-publish-to-docker-hub
* Observar o arquivo `app/main.go` para entender superficialmente o que ele faz: pequena API na linguagem Golang que devolve uma mensagem JSON
* Observar o arquivo `Dockerfile` para entender o que ele faz: importar e compilar nosso arquivo Golang, e invocar este arquivo compilado na inicializão de contêineres baseados nessa imagem/aplicação
* Observar o arquivo `.github/workflows/build-docker-image.yml` para entender superficialmente o que ele faz: instruir o GitHub Actions a fazer o build de nossa imagem Docker baseada no nosso Dockerfile, que por sua vez importa o nosso API desenvolvido em Golang.

#### Parte 2 - Configurar secrets de integração entre GitHub e Docker Hub
* É necessário gerar uma chave (senha) dentro do Docker Hub para liberar acesso automatizado a partir do GitHub Actions
* Para obter o `DOCKERHUB_TOKEN`, acesse https://hub.docker.com, clique no nome do usuário no lado direito da barra superior, depois em `Account Settings`, depois em `Security` no menu lateral esquerdo, depois em `New Acess Token`. Dê um nome qualquer para o seu token, por exemplo `github-minicurso`. Não altere os `Access permissions`, deixe o padrão. A chave gerada no quadrado preto é o seu `DOCKERHUB_TOKEN`, para você inserir na configuração do projeto dentro do GitHub.
* Configurar o token gerado acima dentro do projeto do GitHub clicando em `Settings -> Menu lateral esquerdo -> Seção Secrets -> Actions`. Os nomes das variáveis são `DOCKERHUB_USERNAME` e `DOCKERHUB_TOKEN`, conforme descrito no arquivo `build-docker-image.yml`
* Utilize o editor web do GitHub para atualizar o nome do seu repositório Docker na linha 45 do arquivo `build-docker-image.yml`. Substitua `rpagliuca` pelo seu nome de usuário do Docker Hub.

#### Parte 3 - Habilitar GitHub Actions
* Após fazer as alterações acima, abra novamente seu repositório no GitHub, e clique em `Actions`, botão que fica entre `Pull Requests` e `Projects`, no menu cinza do meio da tela.
* Clique em `I understand my workflows, go ahead and enable them`

#### Parte 4 - Criar novo commit no projeto
* Utilize o editor web do Github para alterar o arquivo `app/main.go`, customizando a string da linha 29. Digite uma mensagem de sua preferência, mensagem está que será retornada em todas as chamadas da sua nova aplicação.

#### Parte 5 - Validar que workflow/action rodou com sucesso
* Clique novamente em `Actions` no meu cinza do meio da tela. Depois da alteração da linha 29, um novo workflow deve ter sido inicializado. Se tudo der certo, você terá sua aplicação publicada dentro do repositório do Docker Hub.
* Caso o workflow tenha executado com erro, confirme se as secrets `DOCKERHUB_USERNAME` e `DOCKERHUB_TOKEN` foram configuradas corretamente em seu repositório. Confirme também que você alterou o nome do repositório na linha 45 do arquivo `build-docker-image.yml`.

#### Parte 6 - Rodar sua nova aplicação dentro do Docker Playground
* Abrir o Docker Playground: https://labs.play-with-docker.com/
* Iniciar uma nova instância
* Rodar o comando abaixo, substituindo o trecho `rpagliuca/go-simple-app:latest` pelo mesmo nome de projeto que você definiu na linha 45 do arquivo `build-docker-image.yml`
* Comando: `docker run -d -p 8005:8080 rpagliuca/go-simple-app:latest`
* Para abrir a aplicação, clique no botão `8005` que irá aparecer ao lado de `OPEN PORT`
* Você vai ver uma mensagem simples de texto no padrão JSON
* Parabéns! Você concluiu o último objetivo deste minicurso.

### Etapa 13 - Conclusão (10 minutos)
* Próximos passos
    * Instalar e utilizar Docker na sua máquina local, em vez de usar o Docker Playground
    * Utilizar o Git via terminal/console junto com alguma IDE como Visual Code, por exemplo, em vez de alterar os códigos-fonte na interface web do GitHub
    * Construir uma imagem Docker customizada para a sua aplicação web (Javascript / Golang / Java / .NET / Python / Ruby on Rails / PHP / etc)
    * Criar um pipeline de automação da imagem Docker usando sua plataforma favorita
    * Hospedar sua aplicação Docker usando seu provedor favorito. Atenção: usualmente hospedar uma aplicação é um serviço com custos financeiros!

## Anexos

### Lista de comandos do Linux
* Criar um novo diretório: `mkdir`
* Entrar em um diretório: `cd nome_do_diretorio`
* Listar conteúdo da pasta atual: `ls -a`

### Lista de comandos do Docker para o terminal
* Listar contêineres rodando: `docker ps`
* Parar um contêiner que está rodando: `docker stop hash_do_contêiner`

### Lista de comandos  Git para o terminal
* Clonar repositório: `git clone url_do_repositório`
* Novo commit: `git commit -am 'Meu commit'`
* Push (enviar commit para o repositório remoto): `git push`

### Plataformas que dão suporte a construir (build) aplicações/imagens Docker
* Bitbucket (Bitbucket Pipelines)
* Github (Github Actions)
* GitLab
* Travis CI
* Jenkins
* Circle CI

### Algumas plataformas que dão suporte a rodar/hospedar aplicações Docker
* AWS (Elastic Beanstalk; Elastic Container Service)
* Digital Ocean
* Google Cloud
* Kubernetes (em qualquer provedor)
