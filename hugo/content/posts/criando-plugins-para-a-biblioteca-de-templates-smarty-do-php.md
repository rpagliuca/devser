---
title: "Criando plugins para a biblioteca de templates Smarty do PHP"
date: 2017-03-16
draft: false 
aliases:
- /2017/03/criando-plugins-para-a-biblioteca-de-templates-smarty-do-php/
categories:
- PHP e DB
---
# Introdução ao Smarty

A biblioteca de template Smarty é mais uma ferramenta para auxiliar a separação de camadas de uma aplicação PHP. Mais especificamente, essa biblioteca normalmente é utilizada para fazer a separação entre arquivos das camadas Controller e View.

O Smarty processa arquivos de template, tipicamente com extensão .tpl, injetando variáveis PHP e retornando o conteúdo renderizado. Um arquivo simples de template do Smarty tem a seguinte forma:

{{<highlight html>}}
<!DOCTYPE html>
<html>
<body>
<h1>{$titulo}</h1>
<p>Olá, {$nome}</p>
</body>
</html>
{{</highlight>}}

Ao renderizar o template acima, o Smarty irá substituir as tags {$titulo} e {$nome} para os valores correspondentes designados com as chamadas aos métodos $smarty->assign(‘titulo’, …) a partir do controller.

Reaproveitando templates
Muitas vezes nos vemos repetindo muitas e muitas vezes o mesmo trecho de código HTML. Para casos mais simples, podemos dividir um único arquivo de template em vários diferentes, assim poderemos reaproveitar o código. Por exemplo:

{{<highlight html>}}
{* Template header.tpl *}
<!DOCTYPE html>
<html>
<body>
{{</highlight>}}

{{<highlight html>}}
{* Template footer.tpl *}
</body>
</html>
{{</highlight>}}

{{<highlight html>}}
{* Template principal *}
{include file="header.tpl"}
<h1>{$titulo}</h1>
<p>Olá, {$nome}</p>
{include file="footer.tpl"}
{{</highlight>}}

# Por que plugins?
Em determinadas ocasições, a tag `{include file=""}` pode não ser suficiente para renderizar algum conteúdo dinamicamente. Nesses casos, podemos lançar da mão de plugins, isto é, extensões para o Smarty.

No exemplo acima, poderíamos criar um plugin implementando uma nova tag chamada `{header}` e outra chamada `{footer}`,
de forma que o template principal seria dado por:

{{<highlight html>}}
{* Template principal *}
{header}
<h1>{$titulo}</h1>
<p>Olá, {$nome}</p>
{footer}
{{</highlight>}}

A conversão do header e do footer para plugins em vez de subtemplates nesse caso específico não é vantajosa, mas em casos mais complexos isso é interessante pois nos permite executar código PHP toda vez que as tags {header} e {footer} sejam incluídas no template do Smarty.

Suponha, por exemplo, que no {header} você queira sempre exibir uma mensagem customizada de acordo com a hora do dia (“bom dia", “boa tarde", “boa noite"). Esse é um caso simples do papel do plugin.

# Criando um plugin


{{<highlight php>}}
$smarty->registerPlugin('function', 'header', function () {
    $hora = date('H:i');
    if ($hora < 6) {
        $frase = "Boa noite";
    } elseif ($hora < 12) {
        $frase = "Bom dia";
    } elseif ($hora < 19) {
        $frase = "Boa tarde";
    } else {
        $frase = "Boa noite";
    }
    $smarty->assign('frase', $frase);
    return $smarty->fetch('header.tpl');
});
{{</highlight>}}
