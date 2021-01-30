---
title: "Finanças Decentralizadas (De-Fi) com MakerDAO e DAI"
date: 2021-01-27T20:20:54-03:00
categories:
- Blockchain
- Finanças
draft: true
---

Conjunto de contratos inteligentes que permite emprestar DAI (stablecoin pareada ao dólar americano, 1 DAI = US$ 1) utilizando como garantia diversos tokens ERC20, além de Ether.

Funciona mais ou menos assim: para gerar US$100 (100 DAI), é necessário enviar para o contrato uma quantia superior em garantias. Por exemplo, você pode enviar 0,0066 WBTC (token ECR20 pareado ao Bitcoin), que com a cotação deste exato momento é equivalente a aproximadamente US$200, e gerar US$100 em DAI em troca.

Você ainda continua tendo direito sob seus ativos em garantia. A qualquer momento você pode devolver o DAI emprestado e recuperar os seus tokens originais.  É importante lembrar que existe juros (ou stability fees) sobre a geração de DAI.

Esse tipo de operação é vantajosa caso você precise de liquidez financeira, mas ao mesmo tempo acredite que seus ativos sob garantias irão valorizar mais do que a taxa de juros incidente sobre o DAI.

Caso de uso: você possui US$200 guardados em ETH (Ether), mas precisa de US$100 para realizar um pagamento em um comércio eletrônico. Se você acreditar que a cotação do ETH vai se manter estável em relação ao dólar (US$), você pode simplesmente vender metade do seu ETH (equivalente a US$100) e realizar o seu pagamento. Entretanto, caso você acredite que o ETH irá valorizar em relação ao US$ (mais do que a taxa de juros da emissão de DAI), você pode realizar os passos abaixo:
1) Enviar os seus US$200 para o contrato do MakerDAO como garantia
2) Gerar US$100 em DAI
3) Pagar sua conta no e-commerce
4) Assim que você acumular novamente 100 DAI (o que pode demorar dias, meses, ou anos, indiferente, já que não há prazo para o empréstimo), você devolve o empréstimo (mais os juros!) e recupera seus ativos.
