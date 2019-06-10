---
title: "Compilador para computador SAP-1"
date: 2017-04-18
draft: false
aliases:
- /2017/04/compilador-para-computador-sap-1/
categories:
- Microcontroladores
---
A pedidos do amigo Antonio Souza, segue código-fonte do compilador para computador SAP-1 utilizado no artigo <a href="{{<ref "posts/simulando-um-computador-sap-1-simple-as-possible-1.md">}}">Simulando um computador SAP-1 (Simple As Possible 1)</a>.

Desenvolvi esse compilador em PHP, para poder disponibilizá-lo online juntamente ao referido artigo. A parte principal do compilador é dada pelas primeiras 25 linhas do código. O restante se refere simplesmente ao próprio formulário HTML para receber um texto e convertê-lo em binário.

O script abaixo encontra-se funcionando no endereço http://devser.com.br/misc/sap-compiler.

{{<highlight php>}}
<?php
if (isset($_POST['code']) and $_POST['code']) {
    $uniqid = uniqid('', true);
    $output_atmel = "out/$uniqid.sap.tmp";
    $content = $_POST['code'];
    $content = str_replace(array('LDA', 'ADD', 'SUB', 'OUT', 'HLT', ' '), array('0', '1', '2', 'E0', 'F0', ''), $content);
    $lines = explode("\n", $content);
    $output_content = "";
    foreach ($lines as $line) {
        $line = trim($line);
        if ($line AND strpos($line, ";") === FALSE) {
            $parts = explode(":", $line);
            $address = $parts[0];
            $data = $parts[1];
            $formatted_line = '$A' . str_pad($address, '4', '0', STR_PAD_LEFT) . ', ' . str_pad($data, '2', '0', STR_PAD_LEFT) . "\n";
            $output_content .= $formatted_line;
        }
    }
    file_put_contents($output_atmel, $output_content);
    system("srec_cat -output $output_atmel.hex --Intel $output_atmel --Needham_Hexadecimal");
    unlink($output_atmel);
    header('Content-disposition: attachment; filename="SAP.hex"');
    readfile("$output_atmel.hex");
    unlink("$output_atmel.hex");
} else {
    header('Content-type: text/html; charset=utf-8');
?>
<html>
<head>
<title>Compilador SAP-1</title>
<style>
textarea{
    width: 600px;
    height: 400px;
}
</style>
</head>
<body>
<form method="post">
<textarea name="code">
; Programa exemplo
; Linhas iniciadas com ponto e vírgula são comentários
 
; Repare que toda linha de código deve começar com o número da linha e em seguida dois pontos, sempre em hexadecimal.
; O SAP-1 trabalha com uma memória de 16 posições. Ou seja, podemos ter da linha 0 até a linha F.
 
; Instruções
0: LDA A
1: ADD B
2: OUT
3: HLT
 
; Dados
A: 4
B: 6
 
; Eu gosto de iniciar os dados na posição A da memória, mas tanto faz. Poderia começar da 4, já que a última utilizada por uma instrução foi a linha 3
</textarea>
<input type="submit" value="Compilar"/>
</form>
</body>
</html>
<?php
}
?>
{{</highlight>}}
