---
title: "Hello world e SHA-256 em C"
date: 2019-08-19T20:21:00-03:00
categories:
- C
---

{{<youtube -zPMxKuXdr8>}}

<br/>

No vídeo de hoje, eu mostro como implementar um simples "Hello, world!" utilizando a linguagem de programação C.

Para os mais curiosos, vou além e ensino também como utilizar a biblioteca OpenSSL para calcular
o hash SHA-256 de uma string.

Segue código final da rotina (arquivo **main.c**):

{{<highlight c>}}
// File: main.c

#include <stdio.h>
#include <openssl/sha.h>
#include <string.h>

int main(int argcount, char **args) {
    if (argcount != 2) {
        printf("Usage: %s <string>\n", args[0]);
        return 1;
    }
    printf("INPUT: %s\n", args[1]);
    unsigned char *hash = SHA256(args[1], strlen(args[1]), NULL);
    printf("SHA256: ");
    for (int i = 0; i < 32; i++) {
        printf("%02x", hash[i]);
    }
    printf("\n");
    return 0;
}
{{</highlight>}}

Dependências no Debian:

{{<highlight bash>}}
sudo apt-get install libssl-dev
{{</highlight>}}

Comandos para compilar e executar a rotina acima, no terminal do Linux:

{{<highlight bash>}}
# Atenção: a ordem dos parâmetros do comando a seguir é importante!
gcc main.c -lcrypto
./a.out "My string"
{{</highlight>}}
