---
title: "Backup em Mídia Blu-Ray com Redundância"
date: 2020-12-13T04:32:47-03:00
draft: true
---

## Introdução

Mídias ópticas, como discos Blu-Ray ou DVD, são uma alternativa economicamente viável para estratégias de backup *off-site*, pessoais ou de pequena escala, em comparação a drives de fita magnética.

Entretanto, tais mídias são conhecidas por deteriorar em contato com umidade, luminosidade, entre outros fatores externos (saiba mais em https://www.clir.org/pubs/reports/pub121/sec4/).

Para mitigar os riscos de perda de dados, o software DVDisaster foi desenvolvido, implementando ECC (*error correction code*) com algoritmo Reed-Solomon.

## Ferramentas utilizadas
* Gravador externo ASUS SBW-06D2X-U
* Mídia Blu-ray 25GB
* Sofware DVDisaster (fork: https://github.com/rpagliuca/dvdisaster)
    Permite aumentar a redundância e resiliência de uma única mídia (parte da mídia é reservada para ECC)
* Software Parchive (fork: https://github.com/Parchive/par2cmdline)
    Permite criar redundância em um conjunto de mídias (mídias adicionais são utilizadas para ECC)

## Visão geral
Utilizaremos a combinação de softwares DVDisaster + Parchive.

O papel do DVDisaster é aumentar a redundância/resiliência de cada uma das mídias individualmente, enquanto
o papel do Parchive é aumentar a redundância/resiliência do conjunto de mídias, utilizando, para isso, mídias adicionais.

Vamos para um exemplo prático:

Temos 500GB de arquivos para fazer backup.

Utilizando mídias Blu-ray de 25GB (as mais comuns e baratas), precisamos de 20 mídias para armazenar todos os dados,
sem nenhum tipo de redundância.

Nesse caso, temos 2 problemas:

1) se uma ou mais mídias for levemente danificada, podemos perder todas as informações ali contidas.
2) se uma ou mais mídias não forem localizadas (desaparecerem), também perdemos todas as informações ali contidas.

Nosso desejo então é:

* Resolver o problema 1, aumentando a redundância individual de cada mídia. Mesmo em caso de pequenos danos, ou deterioração, queremos ainda conseguir ler o conteúdo da mídia.
* E também resolver o problema 2, gravando mídias adicionais para funcionar como redundância, caso uma ou mais mídias não forem localizadas.

O papel do DVDisaster é resolver o problema 1, enquanto o software Parchive resolve o problema 2.

Mas isso vem com um custo:

Se configurarmos o DVDisaster para redundância alta, podemos ocupar apenas 74,7% da capacidade de cada mídia.

E com o Parchive, TODO...

## Comandos para executar o DVDisaster

* `git clone https://github.com/rpagliuca/dvdisaster`
* `cat README`
* `cat INSTALL`
* `./configure`
* `./make`
* `./dvdisaster`

## Comandos para executar o Parchive

* `git clone https://github.com/rpagliuca/par2cmdline`
* `cat README` 
*  `./automake.sh`
*  `./configure`
*  `make`
*  `make check`
*  `sudo make install`
*  `par2 --version # par2cmdline version 0.8.1`

## Voltando ao exemplo anterior

Temos 500GB de dados para fazer backup, e mídias Blu-Ray com 25GB de capacidade.

O que vamos fazer:

1. Criar um único arquivo TAR contendo os 500GB de arquivos
2. Utilizar o comando `split` do Linux para quebrar o TAR em arquivos de 18,5GB (ou seja, 74% da capacidade do Blu-Ray, já antevendo o espaço de redundância/ECC), resultando em 28 arquivos
3. Utilizar o comando par2create com redundância 20%

{{<highlight sh>}}

# Generate example file to be backed up
dd if=/dev/urandom of=my_file bs=1024 count=1000000

# Calculate md5sum of newly created file. Keep the result somewhere, as it will be used to test integrity later.
md5sum my_file

# Generate PAR2 files with 30% redundancy. You have to change the number 6, according to your own calculations to keep the size below the 50MB net media capacity (take consideration of the 30% that DVDisaster will use in each media by its own). In my example: 1000000 * 0.3 / 50000 = 6 (you should always round up)
par2create -u -n6 -r30 my_file

# Split file according to media size (here we simulate 50MB media capacity)
split my_file -b50000000

# Move every splitted file into each's own folder
COUNT=0; for f in `ls -1 x*`; do COUNT=$(($COUNT+1)); mkdir media$COUNT; mv $f media$COUNT; done

# MANUAL STEP: Move all par files manually as remainder media

# Generate one ISO file from each older
for f in `ls -1 media* -d`; do genisoimage -f -J -joliet-long -r -allow-lowercase -allow-multidot -o $f.iso $f; done

# Expand ISO with DVDisaster redundancy. The ISO files should increase inplace by 30% each
for f in `ls -1 ~/tmp/hello-par/isos-original/media*.iso`; do ./dvdisaster -mRS02 -i $f -c -n 30%; done

# MANUAL STEP: Simulate some chaotic data loss. In my example, I've deleted 2 ISO files, and changed a few characters inside another 2 of them.
# In my case, I've corrupted media15.iso and media20.iso
/./home/rafael/src/dvdisaster/dvdisaster -i media15.iso -e media15.iso --fix
/./home/rafael/src/dvdisaster/dvdisaster -i media20.iso -e media20.iso --fix

# Install application to extract ISO
sudo apt-get install p7zip-full

# Now extract all the ISO files
for f in `ls -1 media*.iso`; do echo $f; 7z x $f; done

# Concatenate all the splitted files back
cat x* > my_file

# Verify integrity of my_file. It should show errors.
par2verify my_file

# Another test would be calculate the md5sum of the file. It should be different from original one
md5sum my_file

# Fix my_file
par2repair *.par2 my_file

# Now run the final md5sum test. It should be the same as the original one
md5sum my_file

{{</highlight>}}
