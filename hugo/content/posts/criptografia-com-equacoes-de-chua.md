---
title: "Criptografia Com Equações de Chua"
date: "2012-05-11"
aliases:
- /2012/05/criptografia-com-equacoes-de-chua/
categories:
- Criptografia
---

Após o recente sucesso na implementação do modelo proposto por Baptista, optei agora por desenvolver um sistema mais sofisticado, utilizando agora as equações de Chua normalizadas.

\\[\\begin{aligned} \\dot{x_1} & = \\alpha(x_2 - f(x_1))\\\\ \\dot{x_2} & = x1 - x2 + x3\\\\ \\dot{x_3} & = -\\beta x_2 \\end{aligned} \\]

Este outro modelo foi originalmente proposto por [Vaidya](http://scholar.google.com.br/scholar?q=+vaidya+Implementation+of+chaotic+cryptography+with+chaotic+synchronization&btnG=&hl=pt-BR&as_sdt=0) em 1998, e aperfeiçoado alguns anos depois por [C. H. Oliveira](http://scholar.google.com.br/scholar?q=cleber+henrique+oliveira+cryptography+with+chaos&btnG=&hl=pt-BR&as_sdt=0).

Algumas das vantagens desta nova implementação em comparação à anterior são:

- Cada caractere da mensagem pura necessita de 1 byte para ser armazenado após a cifragem. Ou seja, o tamanho da mensagem não é afetado pelo processo da cifragem.
- O novo sistema é mais rápido, já que necessita de um número pequeno de ciclos para cada caracter a ser cifrado.

Esta implementação necessitou de um pouco mais de atenção por se basear em um sistema de equações diferenciais.

Finalizada mais esta etapa da pesquisa, na próxima etapa implementarei um sistema de criptografia simétrica utilizando um modelo caótico jamais antes utilizado para este fim.
