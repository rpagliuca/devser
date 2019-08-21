---
title: "Comparando a perfomance de Go, .NET e PHP"
date: 2019-08-21T11:18:05-03:00
---

## Cenários comparados

Utilizando o programa [wrk](https://github.com/wg/wrk), calculei a capacidade de requisições por segundo das seguintes variações de ambiente:

| Cenário  | Linguagem | Servidor | Framework | Docker | Imagem Docker                              |
|----------|-----------|----------|-----------|--------|--------------------------------------------|
| Go       | Go        | Embutido | Não       | Não    | N/A                                        |
| .NET     | .NET      | Embutido | Não       | Sim    | microsoft/dotnet:2.2-aspnetcore-runtime    | 
| PHP-1    | PHP       | Embutido | Não       | Não    | N/A                                        |
| PHP-2    | PHP       | Nginx    | Não       | Sim    | N/A                                        |
| PHP-3    | PHP       | Nginx    | Symfony   | Sim    | richarvey/nginx-php-fpm                    |
| PHP-4    | PHP       | Nginx    | Symfony   | Sim    | (private)                                  |
| Static   | N/A       | Nginx    | Não       | Sim    | nginxdemos/hello                           |

Todos os ambientes foram configurados para retornar uma única linha contendo repetições da string "Hello, world!".

Os cenários Go, PHP-1, PHP-2 e Static estão rodando o mínimo de código possível que permita
servir a resposta HTTP esperada.

Já os cenários .NET, PHP-3 e PHP-4 estão estruturados como projetos web MVC, sendo que o .NET está utilizando
apenas bibliotecas padrões do .NET Core, e os PHP-3 e PHP-4 estão rodando o framework Symfony.

Os ambientes acima foram disponibilizados no GitHub: https://github.com/rpagliuca/go-vs-dotnet-vs-php.  

## Resumo dos resultados

Os resultados foram os seguintes:

| Cenário  | Requests por segundo |
|----------|----------------------|
| Go       | 30437                |
| .NET     | 6933                 |
| PHP-1    | 135                  |
| PHP-2    | 3691                 |
| PHP-3    | 353                  |
| PHP-4    | 19                   |
| Static   | 9798                 |

O Go venceu com folga, com 30.000 requests por segundo, já que além de ser uma linguagem compilada diretamente
para código nativo, o servidor web
está embutido no binário gerado.

O cenário Static (Nginx servindo uma página estática index.html) veio logo atrás, com quase 10.000 requests por segundo,
sendo que a diferença provavelmente se dá pela necessidade de o servidor Nginx ler a página index.html a partir
do filesystem.

O cenário .NET não ficou muito longe, com quase 7.000 requests por segundo. Esse resultado me surpreendeu positivamente,
já que é um projeto bem mais complexo, estruturado no formato MVC, e rodando no runtime .NET Core dentro do Docker.

Por último, o PHP conseguiu apenas 50% da performance do .NET, mas apenas quando utilizando o cenário sem frameworks. Já quando
utilizamos o Symfony, a performance
caiu mais 10 vezes com a imagem richarvey/nginx-php-fpm, e ainda mais com uma imagem privada.

Esse último ponto serve como alerta: construir uma imagem Docker do zero, pode ser mais desafiador do que aparenta.
Sempre faça benchmarks comparando suas imagens privadas contra outras imagens confiáveis disponíveis no Dockerhub.
O resultado pode te surpreender (como me surpreendeu!).

## Resultados completos:

### Go
<pre>
Running 1s test @ http://localhost:19319
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    38.31us   67.33us   1.97ms   97.27%
    Req/Sec    30.62k     4.13k   32.91k    90.91%
  Latency Distribution
     50%   29.00us
     75%   31.00us
     90%   36.00us
     99%  257.00us
  33471 requests in 1.10s, 24.32MB read
Requests/sec:  30436.84
Transfer/sec:     22.12MB
</pre>

### .NET

<pre>
Running 1s test @ http://172.24.0.2:5000
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   158.51us  198.57us   4.70ms   98.15%
    Req/Sec     6.97k   561.31     7.49k    90.00%
  Latency Distribution
     50%  135.00us
     75%  143.00us
     90%  152.00us
     99%  652.00us
  6939 requests in 1.00s, 5.28MB read
Requests/sec:   6933.43
Transfer/sec:      5.28MB
</pre>

### PHP-1
<pre>
Running 1s test @ http://localhost:19319
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   219.61us   61.79us 786.00us   95.71%
    Req/Sec   136.80      5.14   144.00     80.00%
  Latency Distribution
     50%  222.00us
     75%  242.00us
     90%  266.00us
     99%  409.00us
  140 requests in 1.04s, 20.37KB read
  Socket errors: connect 0, read 140, write 0, timeout 0
Requests/sec:    135.02
Transfer/sec:     19.65KB
</pre>

### PHP-2
<pre>
Running 1s test @ http://172.27.0.2
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   369.61us  540.17us   6.59ms   93.66%
    Req/Sec     3.72k   383.31     4.17k    80.00%
  Latency Distribution
     50%  216.00us
     75%  302.00us
     90%  508.00us
     99%    3.16ms
  3697 requests in 1.00s, 2.98MB read
Requests/sec:   3691.39
Transfer/sec:      2.97MB
</pre>

### PHP-3
<pre>
Running 1s test @ http://172.26.0.2
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     3.23ms    2.86ms  28.50ms   94.91%
    Req/Sec   354.30     83.73   400.00     90.00%
  Latency Distribution
     50%    2.49ms
     75%    2.86ms
     90%    3.59ms
     99%   19.99ms
  353 requests in 1.00s, 302.66KB read
Requests/sec:    352.56
Transfer/sec:    302.27KB
</pre>

### PHP-4
<pre>
Running 1s test @ http://172.28.0.2
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    50.26ms   10.27ms  92.37ms   94.74%
    Req/Sec    19.00      3.16    20.00     90.00%
  Latency Distribution
     50%   47.57ms
     75%   48.16ms
     90%   51.25ms
     99%   92.37ms
  19 requests in 1.00s, 16.44KB read
Requests/sec:     18.98
Transfer/sec:     16.43KB
</pre>

### Static
<pre>
Running 1s test @ http://172.23.0.2
  1 threads and 1 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   169.02us  472.01us   8.00ms   96.21%
    Req/Sec     9.85k   787.34    10.70k    80.00%
  Latency Distribution
     50%   88.00us
     75%  104.00us
     90%  138.00us
     99%    2.19ms
  9861 requests in 1.01s, 8.25MB read
Requests/sec:   9797.95
Transfer/sec:      8.19MB
</pre>
