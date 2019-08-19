---
title: "Hello world e SHA-256 em C"
date: 2019-06-20T18:19:27-03:00
---

{{<youtube FHa6x_NPZKg>}}

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

Comandos para compilar e executar a rotina acima, no terminal do Linux:

{{<highlight bash>}}
gcc -lcrypto main.c
./a.out "My string"
{{</highlight>}}
