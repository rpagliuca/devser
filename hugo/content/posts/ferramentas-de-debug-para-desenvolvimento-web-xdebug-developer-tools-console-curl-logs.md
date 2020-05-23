---
title: "Ferramentas de debug para desenvolvimento web: Xdebug + Developer Tools Console + Curl + Logs"
date: "2017-02-21"
aliases:
- /2017/02/ferramentas-de-debug-para-desenvolvimento-web-xdebug-developer-tools-console-curl-logs/
categories:
- PHP
---

Quem migra do desenvolvimento Desktop para o desenvolvimento Web com PHP, usualmente sente falta das ferramentas de debug disponíveis nas IDEs de C++, Java, .NET, etc

Dessa forma, abordarei neste artigo uma visão geral sobre as ferramentas disponíveis para os desenvolvedores web, e em quais circustâncias cada uma delas pode ser útil.

OBS - A partir de agora, assumo um ambiente Linux + Apache + PHP, mas muito disso pode ser reaproveitado em outros ambientes.

# Logs do Apache + PHP

Logs do Apache e do PHP são seus melhores amigos, já que eles gravam de forma definitiva todos os erros que possam ter estourado em seu código, independentemente da página a ser testada emitir um redirect para outra página, por exemplo.

## Logs do Apache

Os logs do Apache2 estão ativados por padrão, e ficam armazenados nos seguintes caminhos:

* /var/log/apache2/error.log
* /var/log/apache2/access.log
* /var/log/apache2/other_vhosts_access.log

Como normalmente utilizamos VirtualHosts dentro do Apache, alguns dos arquivos anteriores estarão vazios, sendo substituídos pelos caminhos de logs configurados no próprio arquivo de configuração do VirtualHost, conforme exemplos de diretivas a seguir:

{{<highlight apache>}}
# File: /etc/apache2/sites-enabled/my_virtual_host.conf
ServerName myvirtualhost
ErrorLog /var/www/my_virtual_host/log/error.log
CustomLog /var/www/my_virtual_host/log/access.log combined
 ...
{{</highlight>}}

##  Logs do PHP

A maneira mais fácil de investigar o seu ambiente PHP é criar um novo arquivo chamado **phpinfo.php** com o seguinte conteúdo e colocá-lo no diretório público de seu VirtualHost.

Abrindo a url **http://myvirtualhost/phpinfo.php** (ou outra URL, conforme sua configuração de VirtualHost e também do arquivo /etc/hosts), teremos acesso a todas as diretivas de configuração do PHP. As que nos interessam, neste instante, são as seguintes:

* Loaded Configuration File: /etc/php/5.6/apache2/php.ini
* log_errors: true
* error_reporting: 32767
* error_log: no value

Informações detalhadas sobre as diretivas acima podem ser obtidas no manual do PHP: [http://php.net/manual/pt_BR/errorfunc.configuration.php#ini.error-log](http://php.net/manual/pt_BR/errorfunc.configuration.php#ini.error-log) A parte importante é que os logs de erros estão ativados, e que tudo (incluindo NOTICES) serão exibidas. O fato de error_log ser **null** implica que os logs do PHP vão ser redirecionados para os logs de erro do Apache2. Simples, assim.

## Visualização de logs

Algumas ferramentas que estão disponíveis em qualquer máquina Linux são ótimas para visualizar logs.

###  Tail

O programa **tail**, quando chamado sem nenhum parâmetro exceto o nome do arquivo, apenas exibe as últimas linhas do log.

Exemplo:

`tail /var/www/my_virtual_host/log/error.log`

Tudo fica mais interessante ao se adicionar o parâmetro **-f** ao comando tail, pois dessa forma, o terminal se tornará um monitor em tempo real do log, isto é, novos registros serão exibidos instantaneamente.

`tail -f /var/www/my_virtual_host/log/error.log`

### Vim

Os programas **vim** ou mesmo o **vi** também são úteis para navegar em logs, principalmente aqueles com dezenas de megabytes ou gigabytes, já que eles só armazenam na memória parte do conteúdo do arquivo, ao contrário de outros editores que tentam ler todo o conteúdo do arquivo para a memória logo ao carregar o arquivo.

Principais comandos para navegação nos logs:

* **G** -> Ir para o final do log
* **gg** -> Ir para o início do log
* **/** -> Buscar por uma palavra
* **n** -> Ir para próxima ocorrência da busca

### Colortail

Uma versão estendida do **tail**, que conta com output colorido conforme regex que pode ser customizado por você mesmo.

Este programa pode ser instalado no Debian Jessie com o seguinte comando:

`sudo apt-get install colortail`

Link para o meu regex personalizado **(/etc/colortail/conf.colortail)**: [https://gist.github.com/rpagliuca/75f675869cf8e1ceede5c1f17edfc24e](https://gist.github.com/rpagliuca/75f675869cf8e1ceede5c1f17edfc24e)

Usando o regex personalizado, seu log ficará parecido com esse: [![](/images/Screenshot-from-2017-02-24-081510.png "Colortail com regex personalizado para erros do PHP")](/images/Screenshot-from-2017-02-24-081510.png)

# Developer Tools Console

Principalmente utilizado para debugar Javascript, Ajax, e HTTP Requests. Está disponível tanto no navegador Chrome quanto no Firefox apertando a tecla F12. Vou comentar algumas funcionalidades importantes do Developer Tools do navegador Chrome, mas as ferramentas do Firefox são similiares.

As funcionalides a seguir estão todas contidas na aba **Network/Rede**:

## 1) Configuração:

1.1) **Filter**: Permite definir quais tipos de request serão listados na aba Network. Normalmente pode não ser interessante logar requests para arquivos JS, CSS ou Img, por exemplo.

1.2) **Preserve Log**: Útil para páginas com redirect, ou com ajax seguido de submissão de form. Preserva o log de todos os request, até que sejam manualmente apagados.

1.3) **Disable Cache**: Desabilita cache de imagens e javascript, crucial para debug de arquivos recém alterados.

## 2) Requests:

2.1) **Initiator**: Informação sobre quem disparou o request. Pode ter sido um form da página, uma linha de javascript, etc. No caso de javascript, é apresentado um **backtrace** do disparo. Essa informação é fundamental para debugar ajax.

2.2) **Headers** (Response Headers/Request Headers): Permite visualizar se o formulário ou ajax a ser testado está enviando e recebendo corretamente as informações esperadas, via GET ou POST.

2.3) **Cookies**: Lista todos os cookies dispníveis para o navegador a partir da URL atual. Com essa informação, é possível até inspecionar o arquivo de de sessão do PHP dentro do servidor.

2.4) **Replay XHR** (para Ajax): Muito útil, mas no Chrome funciona apenas com ajax. Permite reproduzir um request qualquer. É muito útil para testar ações repetitivas, como submeter formulários. Para suprir essa falta de **Replay** para requests GET e POST em geral, utilizamos o item a seguir.

2.5) **Copy -> Copy as cURL**: Supre muito bem a falta de um **Replay** genérico no Chrome. Utilizando essa ação, um comando de request do tipo cURL é criado e pode ser colado diretamente no terminal. Dessa forma, ações repetitivas podem ser muito facilmente testadas.

# Xdebug

O Xdebug é um plugin para o PHP que possibilita debug interativo do de scripts php a partir do interpretador PHP, permitindo **breakpoints**, **step over**, **step into**, visualização de valores de variáveis, etc.

No Debian Jessie, ele pode ser instalado via repositório:

`apt-get install php5-xdebug`

As seguintes linhas devem ser adicionadas ao **php.ini **para permitir o modo debug via Xdebug:

{{<highlight ini>}}
xdebug.remote_enable=on
xdebug.remote_handler=dbgp
xdebug.remote_host=localhost
xdebug.remote_port=9000
xdebug.remote_log=/tmp/xdebug.log
{{</highlight>}}

Depois desta etapa, é necessário reiniciar o Apache:

`sudo service apache2 restart`

Acesse novamente a página **phpinfo.php** que você criou no início deste tutorial, para conferir se os novos parâmetros de xdebug foram lidos corretamente.

Por último, é necessário configurar a sua IDE para aguardar conexões do Xdebug. Essa configuração depende muito da IDE, então optei por apenas encaminhar o leitor a instruções adicionais:

Para quem utiliza o Netbeans: [https://netbeans.org/kb/docs/php/debugging_pt_BR.html#howDebuggerWorks](https://netbeans.org/kb/docs/php/debugging_pt_BR.html#howDebuggerWorks) [https://blogs.oracle.com/netbeansphp/entry/path_mapping_in_php_debugger](https://blogs.oracle.com/netbeansphp/entry/path_mapping_in_php_debugger)

Para quem utiliza o Vim: [https://github.com/joonty/vdebug](https://github.com/joonty/vdebug)
