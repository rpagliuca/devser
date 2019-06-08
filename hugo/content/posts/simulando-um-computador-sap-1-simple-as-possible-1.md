---
title: "Simulando um computador SAP-1 (Simple As Possible 1)"
date: "2013-10-15"
---

Nada melhor do que um modelo simplificado para nos ajudar a entender algo bastante complicado. A arquitetura de computador SAP-1 (Simple As Possible 1 - "Tão Simples Quanto Possível 1") é um exemplo disso. É um computador bem simples, que basicamente só consegue somar, subtrair e acender LEDs para exibir o resultado do cálculo de forma binária.

Os detalhes técnicos e diagramas esquemáticos da arquitetura podem ser obtidos em [http://www.ic.unicamp.br/~ducatte/mc542/2012S2/sap-1.pdf](http://www.ic.unicamp.br/~ducatte/mc542/2012S2/sap-1.pdf).

Além disso, informações mais detalhadas sobre os principais componentes desse circuito podem ser lidas no documento [Relatório\_SAP-1.pdf](http://rs.anoluz.net/wp-content/uploads/2013/10/Relatório_SAP-1.pdf)

Valeu a pena ter tocado o projeto até o final. Para mim, um computador agora não parece mais um artefato tão místico quanto costumava ser. Eu até fui além de simplesmente simular o SAP-1 e cheguei a construir um compiladorzinho pra ele!

Segue vídeo da simulação desse tipo de computador usando o software Proteus. Primeiro eu dou uma visão geral do computador, depois mostro a execução de uma rotina programada de modo manual (via chaves) e depois usando um modo mais prático: compilador "caseiro" + memória ROM.

http://www.youtube.com/watch?v=qIYe6vY4N0M&feature=youtu.be

Para quem quiser se aventurar, seguem dois projetos separados do Proteus, prontos para serem simulados. O primeiro é pra quem quiser programar manualmente através das chaves. Já o segundo, só roda se carregar um arquivo .HEX contendo o programa dentro da memória EEPROM.

[Arquivo de projeto 1: SAP-1 - Programação manual (via chaves)](http://rs.anoluz.net/wp-content/uploads/2013/10/SAP-Programacao_Manual.zip)

[Arquivo de projeto 2: SAP-1 - Programação automática (compilador + ROM)](http://rs.anoluz.net/wp-content/uploads/2013/10/SAP-Programacao_ROM.zip)

Para programar, utilize o compilador online: [http://rs.anoluz.net/misc/sap-compiler/](http://rs.anoluz.net/misc/sap-compiler/)

Siga os passos a seguir para carregar o arquivo SAP.hex do compilador online dentro da memória ROM:

[![](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-1-300x187.jpg "sap-programacao-1")](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-1.jpg)

Passo 1: Procure a memória ROM (destacada acima) e dê dois cliques sobre ela.

[![](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-2-300x187.jpg "sap-programacao-2")](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-2.jpg)

Passo 2: Clique no ícone da pastinha amarela.

[![](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-3-300x187.jpg "sap-programacao-3")](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-3.jpg)

Passo 3: Selecione o arquivo SAP.hex que você salvou a partir do compilador online.

[![](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-4-300x187.jpg "sap-programacao-4")](http://rs.anoluz.net/wp-content/uploads/2013/10/sap-programacao-4.jpg)

Passo 4: Selecione o botão "Play" no canto inferior esquerdo da tela. O resultado da cálculo será exibido em forma binária nos LEDs.

Dúvidas? Estou à disposição.
