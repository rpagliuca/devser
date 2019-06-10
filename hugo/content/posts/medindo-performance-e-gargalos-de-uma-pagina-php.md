---
title: "Medindo performance e gargalos de uma página PHP com KCacheGrind + XDebug"
date: 2017-04-17
draft: false
aliases:
- /2017/04/medindo-performance-e-gargalos-de-uma-pagina-php/
categories:
- PHP e DB
---
# Introdução

> "Conheces teu inimigo e conhece-te a ti mesmo" — Sun Tzu (a.k.a “Conheça o gargalo de seu script e atue para otimizá-lo")

Utilizando KCacheGrind + XDebug no PHP, podemos medir quais métodos de um script são os principais “inimigos" de tempo de processamento, o gargalo da execução.

Normalmente, das centenas de métodos que são chamados em um único script, pouquíssimos deles são responsáveis por mais de 90% do processamento, portanto pequenos ajustes nos lugares certos fazem uma grande diferença no tempo de resposta do seu script. Saber como encontrar esses gargalos é o seu maior aliado na hora de otimizar páginas lentas.

Esse será um treinamento bem simples, onde apenas guiarei a instalação desses 2 softwares via apt-get no Debian, e darei exemplo de utilização dos softwares. Não haverá programação envolvida, e não utilizarei o Docker, para diminuir um pouco a complexidade e poder focar no que realmente interessa.

# Instalação do XDebug
Instalar o XDebug para PHP é muito fácil no Debian, já que está disponível no repositório oficial:

`sudo apt-get install php5-xdebug`

# Instalação do KCachegrind
O KCacheGrind também está disponível no repositório oficial, mas infelizmente, esse software necessita de diversas bibliotecas do KDE, que por padrão não estão instaladas em uma distribuição Debian, fazendo com que dezenas de pacotes e centenas de megabytes de dependência sejam baixados e instalados pelo comando a seguir:

`sudo apt-get install kcachegrind`

# Configuração do XDebug
A configuração do XDebug é realizada via php.ini. Em um artigo anterior do blog (<a href="{{<ref "posts/ferramentas-de-debug-para-desenvolvimento-web-xdebug-developer-tools-console-curl-logs.md">}}">Ferramentas de debug para desenvolvimento web: Xdebug + Developer Tools Console + Curl + Logs</a>), aprendemos como configurar o XDebug para debugar interativamente seu script. Hoje, não estamos interessados na função Debug, e sim na função Profile do Xdebug, portanto a configuração é um pouco diferente.

## Modo sempre ativado
Quando estamos debugando um único script PHP diretamente pelo terminal, podemos configurar o XDebug no php.ini para gerar sempre um novo relatório de performance.
Nesses casos, adicionaremos a seguinte configuração no arquivo php.ini:

`xdebug.profiler_enable=1`

## Modo ativação temporária / gatilho
Outras vezes, quando estamos testando um script PHP por meio de um navegador de internet, é mais interessante configurar o XDebug em modo gatilho, para que o relatório de performance seja gerado apenas nos requests escolhidos, pois caso contrário, teríamos dificuldade em encontrar os relatórios certos em meio a dezenas de requisições ajax e GET e que muitas vezes são realizadas paralelamente durante o carregamento de uma página simples.

`xdebug.profiler_enable_trigger=1`

Além disso, deve ser instalado alguma extensão no navegador de internet para setar um cookie com valor XDEBUG_PROFILE=1 a cada vez que se deseja um novo relatório de performance. A extensão que eu utilizo para o Google Chrome é a Xdebug helper (https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc)

# Visualização dos relatórios de performance
O XDebug armazena os relatórios de performance na pasta temporária configurada no php.ini na diretiva sys_temp_dir.
Os arquivos são salvos, por padrão, com a nomenclatura cachegrind.out.*

Para visualizar os relatórios, utilizamos o software KCacheGrind e clicando File -> Open e selecionando o relatório cachegrind.out.* desejado.

Clicando no botão % Relative no menu superior, podemos alternar a exibição entre tempo relativo (percentual) e tempo absoluto (quantidade de milisegundos gastos em cada método).

Clicando sobre o título da coluna Self, temos uma lista ordenadas pelos nossos maiores inimigos, os métodos que mais consomem tempo de execução do nosso script.

Pronto, agora você está pronto para vencer seu inimigo.
