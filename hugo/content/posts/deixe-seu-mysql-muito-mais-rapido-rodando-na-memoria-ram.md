---
title: "Deixe seu MySQL muito mais rápido rodando na memória RAM"
date: 2017-05-16
draft: false
aliases:
- /2017/05/deixe-seu-mysql-muito-mais-rapido-rodando-na-memoria-ram
---
Muitas vezes, o gargalo de uma importação de banco, ou execução de queries SQL é o tempo de escrita e leitura no disco rígido. Sabendo disso, uma maneira bacana de deixar o MySQL rapidão é deixar o banco inteiro armazenado na memória RAM, para nunca precisarmos acessar o disco rígido.

Claro que esse armazenamento é volátil, portanto é recomendado apenas para ambiente de desenvolvimento, onde muitas vezes um script ou importação de dump é executado múltiplas vezes por dia, tomando preciosos minutos do desenvolvedor.

A ideia é rodar todo o MySQL direto na memória RAM, e o passo a passo é bem simples:

Assumindo sistema operacional Linux com distribuição Debian ou Ubuntu, rode como sudo todos os comandos a seguir:

1. `service mysql stop`
2. `cp -r /var/lib/mysql /var/lib/mysql_bkp`
3. `chown mysql:mysql -R /var/lib/mysql_bkp`
4. `rm -rf /var/lib/mysql`
5. `cp -R /var/lib/mysql_bkp /dev/shm/mysql`
6. `chown mysql:mysql -R /dev/shm/mysql`
7. `ln -s /dev/shm/mysql /var/lib`
8. `service mysql start`

OBS – A cada vez que reiniciar o computador a RAM será zerada, e por isso deverão ser repetidos os passos acima (exceto os passos 2 e 3, pois o backup será preservado).

## Explicação

Estamos reaproveitando a partição /dev/shm, que é uma partição existente por padrão no Debian, do tipo tmpfs (partição montada diretamente sobre a RAM). Se você copia qualquer arquivo para essa partição, esse arquivo só é escrito na RAM, em vez de ser gravado no HD. Ao mover a pasta do mysql para essa partição e criar um link simbólico apontando para a partição /dev/shm, todos os bancos do MySQL são armazenados diretamente na RAM. Além disso, no passo 2 criamos um backup, já que todos os dados da partição /dev/shm são perdidas ao desligar o computador e são necessários para restauração do banco a partir do HD.
