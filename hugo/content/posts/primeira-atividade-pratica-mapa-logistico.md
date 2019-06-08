---
title: "Mapa logístico"
date: "2011-12-11"
---

[![Exemplo de gráfico do mapa logístico](http://rs.anoluz.net/wp-content/uploads/2011/12/mapa_logistico_sample-150x150.jpg "Exemplo de gráfico do mapa logístico")](http://rs.anoluz.net/wp-content/uploads/2011/12/mapa_logistico_sample.jpg)Me foi proposta uma tarefa: modelar em MATLAB o mapa logístico e obter seu gráfico. Tem tudo a ver com criptografia, que é uma área sobre a qual gostaria de aprender um pouco mais.

Nos dias seguintes, pesquisei um pouco sobre o modelo e consegui concluir o que me foi proposto. Segue ao lado um dos gráficos gerados no QtOctave (http://www.ohloh.net/p/qtoctave), alternativa de código livre para o MATLAB.

Segue o código para QtOctave da função que gerou o gráfico do mapa logístico:

function mapa\_logistico(r, x0, n)
x(1) = x0;
for i = 1:(n-1)
x(1+i) = r \* x(i) \* (1-x(i));
endfor;
plot(1:n, x)
endfunction;

Além disso, desenvolvi uma rotina em PHP que gera o gráfico do mapa logístico dinamicamente no próprio navegador utilizando as condições iniciais do visitante.

- O aplicativo do mapa logístico está disponível em: [http://mural.anoluz.net/mapa\_logistico](http://mural.anoluz.net/mapa_logistico).

Além disso, defini uma lista de tópicos que merecem atenção:

- Segurança da informação

- Composição
- Conceitos
- Cifragem em blocos ou bits
- Chave simétrica
- Chave assimétrica
- Chave privada e pública
