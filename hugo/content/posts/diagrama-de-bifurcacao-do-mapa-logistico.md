---
title: "Diagrama de Bifurcação do Mapa Logístico"
date: "2012-11-17"
aliases:
- /2012/11/diagrama-de-bifurcacao-do-mapa-logistico/
---

Ontem, desenvolvi em [Octave](https://www.gnu.org/software/octave/) (alternativa ao Matlab) uma rotina que desenha o diagrama de bifurcação do mapa logístico.

Em posts anteriores já falei um pouco sobre o mapa logístico, um sistema caótico que é muito estudado em grande parte por sua extrema simplicidade:

$$x_{n+1} = x_n r (1 - x_n)$$

O diagrama de bifurcação representa quais os possíveis valores de _x_ (eixo vertical) para cada valor de _r_ (eixo horizontal), desconsiderando as primeiras iterações (o período transiente). Ou seja, para saber quais os possíveis valores de _x_ para _r_ = 3.5, por exemplo, trace uma linha imaginária vertical sobre o ponto _r_ = 3.5 (eixo horizontal) e veja quais pontos de _x_ (eixo vertical) a reta corta.

Perceba que conforme se aumenta o valor de _r_, o número de possíveis valores de _x_ dobra, até que, para valores de _r_ mais próximos de 4, uma infinidade de valores existem, adquirindo, assim, comportamento caótico naquela região. Segue o diagrama:

[![](/images/logistc_bifurcation.png "Diagrama de Bifurcação do Mapa Logístico")](/images/logistc_bifurcation.png)

Código da rotina:

```
clear all
X = [];
x0 = 0.6;
r_min = 1;
r_max = 4;
dr = 0.001;
n = (r_max - r_min)/dr;
for r=r_min:dr:r_max
    r   
    x = [ x0 ];
    for i=1:1000
        next_x = r*x(end)*(1-x(end));
        x = [ x next_x ];
    end
    X = [ X ; x(100:end) ];
end
hold on
for i=1:n
    r = r_min + dr*(i-1)
    x = X(i, :);
    plot(x.*0+r, x, '.');
end
hold off
pause
```
