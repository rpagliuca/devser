---
title: "Criptografia com Caos - Baptista"
date: "2012-04-01"
---

Implementei recentemente o meu primeiro sistema de criptografia simétrica utilizando caos. A fim de experimentação, optei pelo [sistema proposto por Baptista](https://encrypted.google.com/url?sa=t&rct=j&q=baptista%20cyrptography%20with%20chaos&source=web&cd=1&cad=rja&ved=0CCIQFjAA&url=http%3A%2F%2Fadsabs.harvard.edu%2Fabs%2F1998PhLA..240...50B&ei=2_yKUNqCEYGk8ASdzIC4CA&usg=AFQjCNFc2ZHwJXBLhtRdMD5dHieyCo3FLA) em 1998. Ou seja, fiz a implementação computacional de um modelo de criptografia descrito por ele em seu artigo.

O sistema de Baptista foi escolhido devido a sua extrema simplicidade, mas ele peca nos seguintes quesitos:

1. A mensagem cifrada é bem maior do que a mensagem pura, pois cada caractere (1 byte) será representado após a cifragem por um número inteiro maior que 256, ou seja, com no mínimo 2 bytes.
2. Alguns autores apontaram falhas de segurança ao se utilizar o mapa logístico como sistema caótico.
