---
title: "Diagrama de Bifurcação do Mapa Logístico"
date: "2012-11-17"
---

Ontem, desenvolvi em [Octave](https://www.gnu.org/software/octave/) (alternativa ao Matlab) uma rotina que desenha o diagrama de bifurcação do mapa logístico.

Em posts anteriores já falei um pouco sobre o mapa logístico, um sistema caótico que é muito estudado em grande parte por sua extrema simplicidade:

$$x\_{n+1} = x\_n r (1 - x\_n)$$

O diagrama de bifurcação representa quais os possíveis valores de _x_ (eixo vertical) para cada valor de _r_ (eixo horizontal), desconsiderando as primeiras iterações (o período transiente). Ou seja, para saber quais os possíveis valores de _x_ para _r_ = 3.5, por exemplo, trace uma linha imaginária vertical sobre o ponto _r_ \= 3.5 (eixo horizontal) e veja quais pontos de _x_ (eixo vertical) a reta corta.

Perceba que conforme se aumenta o valor de _r_, o número de possíveis valores de _x_ dobra, até que, para valores de _r_ mais próximos de 4, uma infinidade de valores existem, adquirindo, assim, comportamento caótico naquela região. Segue o diagrama:

[![](http://rs.anoluz.net/wp-content/uploads/2012/11/logistc_bifurcation-1024x704.png "Diagrama de Bifurcação do Mapa Logístico")](http://rs.anoluz.net/wp-content/uploads/2012/11/logistc_bifurcation.png)

Código da rotina:

```
clear all
X = \[\];
x0 = 0.6;
r\_min = 1;
r\_max = 4;
dr = 0.001;
n = (r\_max - r\_min)/dr;
for r=r\_min:dr:r\_max
    r   
    x = \[ x0 \];
    for i=1:1000
        next\_x = r\*x(end)\*(1-x(end));
        x = \[ x next\_x \];
    end
    X = \[ X ; x(100:end) \];
end
hold on
for i=1:n
    r = r\_min + dr\*(i-1)
    x = X(i, :);
    plot(x.\*0+r, x, '.');
end
hold off
pause
```
