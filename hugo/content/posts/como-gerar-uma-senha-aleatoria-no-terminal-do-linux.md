---
title: "Como gerar uma senha aleatória no terminal do Linux"
date: 2019-06-18T21:36:28-03:00
categories:
- Linux
---

Gerar uma senha aleatória no terminal do Linux não é só possível, como altamente recomendado, pois
assim você garante que a senha gerada, em nenhum momento, passe pelas mãos de terceiros.

Execute o comando abaixo no terminal:

```
cat /dev/random | stdbuf -o0 tr -cd '[:graph:]' | stdbuf -o0 head -c 32
```

Se você quiser entender melhor o que significa cada parte do comando, assista o vídeo a seguir:

{{<youtube u4SclZyo4Pg>}}
