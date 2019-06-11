---
title: "Tirando par ou ímpar pelo Whatsapp"
date: 2019-06-11T12:17:48-03:00
categorias: criptografia
aliases:
- /posts/jogando-par-ou-impar-pelo-whatsapp
---
Você está conversando com aquele seu camarada pelo Whatsapp, combinando a jogatina da noite,
tentando decidir (sem sucesso) se vão jogar CS ou Minecraft. Você quer jogar CS, e o cara quer jogar Minecraft.

Perdidos 20 minutos de discussão, vocês se dão conta de que seus antepassados já desenvolveram uma solução ***infalível***
para resolver esse tipo de impasse:

## Par ou ímpar!

Por que não?! E o desenrolar da disputa acontece em seguida:

![](/images/par-ou-impar-full.png)

***Você se sente roubado***! A mensagem do seu camarada contendo o número `3` demorou muito para chegar,
vários segundos depois que você já tinha mandado a sua. Quem garante que ele não esperou para olhar o seu número `2`
antes de responder com o número `3`?
Com certeza ele ganhou na malandragem.

***Calma***! Esse tipo de problema é bem comum em comunicação assíncrona. Sendo assim, os estudiosos de criptografia
já desenvolveram uma solução há muitas decádas.

Apresento a vocês, o protocolo de ***commitment*** (compromisso).

## Commitment

Usando conceitos de criptografia, mais especificamente funções de *hash*, podemos tirar *par ou ímpar* pelo
Whatsapp de forma totalmente justa. O protocolo funciona assim:

1. A primeira pessoa envia uma mensagem contendo um ***commitment***, um compromisso, como por exemplo, o *hash* do número que escolheu. Ela ainda não manda o número
escolhido, para evitar que a outra parte consiga roubar, e sim, apenas o *hash* do número. Para o protocolo
ser ainda mais seguro, a mensagem deve ser concatenada junto a um *salt* aleatório.
2. A segunda pessoa manda, em texto cru, o número jogado.
3. A primeira pessoa revela o seu número, que pode ser validado pela outra parte por meio do ***commitment***, a partir da mensagem original (incluindo o *salt*) utilizada para gerar o ***commitment***
do primeiro passo.

## Par ou ímpar justo pelo Whatsapp

![](/images/par-ou-impar-full-2.png)

Se você não entendeu 100% o que aconteceu nesse chat, siga comigo.

Quem conhece funções de *hash*, sabe que é computacionalmente impraticável obter uma colisão. Quem não conhece,
vale a pena dar uma estudada, pois essas funções são muito úteis para resolver diversas classes de problemas
de sistemas de informação (https://pt.wikipedia.org/wiki/Fun%C3%A7%C3%A3o_hash).

```
60fa0ee9fb90bb1d
c3cd9f101b53831a
bb2e9c97a714ef1d
27008da5c02fa1dc
df02965b47a7f16d
f96501d749d51180
606f170bc0deec78
8d5f170f326e16b7
```

Quando eu enviei para o meu camarada um *commitment* contendo o *hash* sha512 acima, 
ele teve a certeza de que eu não conseguiria roubar, pois eu demoraria mais
do que a idade do Universo para conseguir adulterar a minha mensagem e obter um *hash* idêntico.

Nesse *hash* está a garantia de que eu não roubei no par ou ímpar.

Depois que o meu camarada escolheu o número `5`, eu pude revelar para ele meu número `3`. E ele conseguiu validar
que eu não roubei.

Validação do meu ***commitment***: 

```
echo -n "Número: 3, Salt: Dq2VibypZm7" | sha512sum
```

Rodando o comando acima no Linux, você vai ter certeza de que eu não roubei!

E se você não está utilizando o Linux, ainda assim pode validar a minha mensagem, utilizando
qualquer site que calcula o hash SHA-512 (por exemplo, https://emn178.github.io/online-tools/sha512.html).
É só gerar o SHA-512 do seguinte conteúdo:

```
Número: 3, Salt: Dq2VibypZm7
```

Problema resolvido.
***Go go go!***
