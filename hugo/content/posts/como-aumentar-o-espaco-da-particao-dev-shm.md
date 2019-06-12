---
title: "Como aumentar o espaço da partição /dev/shm"
date: 2018-11-06
draft: false
categories:
- Linux
---

(Co-publicado em [Time Raposa](http://www.timeraposa.com.br/2018/11/como-aumentar-o-espaco-da-particao-devshm/))

A partição /dev/shm utiliza o filesystem tmpfs, sistema de arquivos especial, armazenado 100% na memória RAM.

Essa partição é muito importante para facilitar a troca de informações entre diferentes processos do sistema operacional, e também para deixar alguns processos mais rápidos.

No Debian 9 (e em outras distros também), o padrão é que essa partição seja montada com 50% da memória RAM. No meu caso, tenho uma máquina com 16GB de RAM, então quando eu ligo meu sistema, possuo 8GB nessa partição:


```
$ df -h
Filesystem Size Used Avail Use% Mounted on
udev 7.8G 0 7.8G 0% /dev
tmpfs 1.6G 126M 1.5G 8% /run
/dev/sda10 425G 357G 47G 89% /
tmpfs 8.0G 221M 7.8G 3% /dev/shm
tmpfs 5.0M 4.0K 5.0M 1% /run/lock
tmpfs 7.8G 0 7.8G 0% /sys/fs/cgroup
/dev/sda1 496M 58M 439M 12% /boot/efi
tmpfs 1.6G 28K 1.6G 1% /run/user/118
tmpfs 1.6G 88K 1.6G 1% /run/user/1000
```

Entretanto, em alguns casos específicos, eu preciso aumentar provisoriamente o tempo dessa partição. Um exemplo é quando eu utilizo alguns scripts de importação de grandes bases de dados, que utilizam mais de 8GB de memória. Esses scripts que eu utilizo rodam um container da imagem mysql:5 utilizando Docker, utilizando o /dev/shm/mysql como volume do container. E eu utilizo esse container provisório para importar o banco, o que em alguns casos fica 10x mais rápido do que importar diretamente no HD convencional.

Neste momento, preciso importar um banco de dados que utiliza até 12GB de RAM no total, então eu preciso fazer o seguinte:

sudo mount -o remount,size=12G tmpfs /dev/shm

Após rodar esse comando, minha partição /dev/shm foi aumentada para 12GB da memória RAM.

Rodando novamente, o comando df -h, agora tenho o seguinte output:

```
$ df -h
Filesystem Size Used Avail Use% Mounted on
udev 7.8G 0 7.8G 0% /dev
tmpfs 1.6G 126M 1.5G 8% /run
/dev/sda10 425G 357G 47G 89% /
tmpfs 12G 222M 12G 2% /dev/shm
tmpfs 5.0M 4.0K 5.0M 1% /run/lock
tmpfs 7.8G 0 7.8G 0% /sys/fs/cgroup
/dev/sda1 496M 58M 439M 12% /boot/efi
tmpfs 1.6G 28K 1.6G 1% /run/user/118
tmpfs 1.6G 84K 1.6G 1% /run/user/1000
```

Não esqueça de depois de rodar seus scripts, voltar o tamanho /dev/shm para o tamanho original (50% da sua memória RAM), rodando o mesmo comando que utilizou para alterar para 12GB. Caso você esqueça de reduzir o tamanho do /dev/shm, aumenta a probabilidade de sua máquina travar por escassez de memória!

Até a próxima!
