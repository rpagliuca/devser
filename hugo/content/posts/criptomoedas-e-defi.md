---
title: "Finanças Descentralizadas: DeFi, Criptomoedas e Blockchain"
date: 2021-01-27T20:20:54-03:00
categories:
- Blockchain
- Finanças
---

* Este artigo ainda é um trabalho em andamento (<i>work in progress</i>)
* Sugestões e correções são bem vindas

## Introdução a DeFi 

Nos últimos dois anos têm-se falado muito sobre DeFi: <i>Decentralized Finance</i> ou Finanças Descentralizadas.

Esse é nome que se dá para um conjunto de contratos inteligentes (<i>smart contracts</i>), comumente executados na <i>blockchain</i> Ethereum, mas não limitados a ela. Esses contratos fornecem de forma impessoal (M2C: <i>Machine To Consumer</i>) uma gama de serviços financeiros descentralizados vinculados à criptomoeda Ether e aos tokens ERC-20 existentes na mesma rede.

![](/images/defi-1/defi-vending-machine.png)

Aplicações de DeFi trazem produtividade às <i>blockchains</i> e criptomoedas, gerando serviços de valor mensurável financeiramente a todos os usuários do ecossistema.

Para se ter uma ideia do tamanho desse mercado, as aplicações MakerDAO, Uniswap e Compound possuem, juntas, aproximadamente **US$12 bilhões** depositados atualmente em seus contratos inteligentes [[^1], [^2], [^3]] (e crescendo a cada dia!).

Primeiramente, vamos revisar alguns pontos do mercado financeiro tradicional e em seguida compará-lo com as aplicações de DeFi.

## Como funciona o mercado financeiro tradicional?

O mercado financeiro tradicional é intermediado por agentes financeiros. Algumas das funções frequentemente assumidas por esses intermediários são:

1) **market-making** (criação de mercado): conectam compradores e vendedores de serviços financeiros
2) **mediador de confiança mútua**: garantem (ou se esforçam para) que ambas as partes, compradores e vendedores, cumpram o acordado
3) **operadores de <i>pools</i>**: pela lei dos grandes números da probabilidade, o desvio padrão do retorno de um produto financeiro é menor quanto maior for a quantidade de transações independentes entre si. Ou seja, o retorno financeiro do produto é mais previsível utilizando-se de <i>pools</i>, ou carteiras diversificadoras de clientes

Entenda melhor a vantagem de se utilizar <i>pools</i> em relação a transações individuais na figura abaixo. Observe que nas transações em <i>pool</i>, o prêmio de risco pago pelos tomadores de empréstimo neutralizam as suas inadimplências, de modo que o retorno recebido fica cada vez mais próximo à taxa de juros definida, quanto maior a quantidade de participantes.

![](/images/defi-1/empréstimo-com-e-sem-pool.png)

### Transações sem agentes financeiros (antes de DeFi)
Se uma pessoa física deseja vender um serviço financeiro (conceder um empréstimo, por exemplo) sem passar por um agente financeiro, precisa:
1) Encontrar alguém interessado em tomar o seu empréstimo, e que concorde com as condições do serviço, como taxa de juros, prazo e forma de pagamento (**market-making**)
2) Confiar diretamente na outra pessoa (nesse caso **não haveria mediador de confiança mútua**), ou então utilizar a justiça tradicional como mediadora por meio de **contratos**
3) Avaliar o **nível de risco elevado**. Por se tratar de transação entre um único vendedor e um único comprador, a variância (e desvio padrão) da taxa de inadimplência (<i>default</i>) possui alta amplitude. A taxa oscila facilmente entre os dois extremos de 0% (inadimplência nula) e 100% (inadimplência total)

### Desvantagens ao se utilizar agentes financeiros tradicionais
* Custo de intermediação (<i>spread</i>, taxas administrativas)
* Confiança mútua questionável

No vídeo abaixo comento um pouco mais sobre os três papéis dos agentes financeiros (antes de DeFi):
{{<youtube gMsPpVVT3hU>}}

## DeFi possibilita a compra e venda de serviços financeiros sem o intermédio de agentes financeiros
1) **market-making**: aplicações descentralizadas conseguem armazenar, na <i>blockchain</i>, a lista de compradores e vendedores para um determinado produto financeiro, e executar o *match-making* de forma automatizada
2) **mediador de confiança mútua**: <i>smart contracts</i> são contratos imutáveis, auditáveis, construídos na forma de algoritmos, nos quais todos os termos do produto financeiro são especificados explicitamente, sem qualquer tipo de subjetividade.
3) **criadores de <i>pools</i>** minimizadoras de de risco: <i>smart contracts</i> possibilitam a criação de <i>pools</i> descentralizadas

### Empréstimos sem garantia?

O leitor atento percebeu que quando descrevi as transações financeiras (antes de DeFi), omiti sobre a necessidade de avaliação de crédito e a possível utilização de garantias (colaterização) nesse processo.

Mas é importante deixar claro que, hoje, a maior parte das soluções de DeFi depende disso.

### Os 4 C's

O mercado de empréstimo tradicional costuma se fundamentar nos 4 C's (em inglês):
* **Collateral (garantia)**: o tomador do empréstimo disponibiliza algum bem como garantia, como imóveis ou outros direitos
* **Capital**: o tomador do empréstimo comprova possuir capital suficiente (proporcionalmente ao empréstimo a ser tomado), por meio de extratos bancários e demais documentos de propriedade
* **Capacity (capacidade/potencial)**: o tomador do empréstimo é avaliado pela sua capacidade/potencial de gerar receita durante a vigência do empréstimo. É o caso de empreendedores que recebem crédito apresentando um modelo de negócio robusto
* **Character (caráter)**: o tomador do empréstimo é avaliado pela sua "honra", ou, em outras palavras, pelo seu histórico de respeito a contratos e pelo seu histórico de pagamentos

### DeFi e o primeiro C

A avaliação de crédito tradicional costuma se basear em uma combinação dos 4 C's.

As primeiras aplicações de empréstimo de DeFi, entretanto, se baseiam principalmente no primeiro C (*collateral*). Mas já há avanço em direção às outras formas de avaliação de crédito, também. Se você quiser saber um pouco mais sobre isso, recomendo esse [artigo da Coindesk](https://www.coindesk.com/aave-unsecured-borrowing-defi).

No momento, aplicações de empréstimos DeFi baseados em garantia (*collateral*) satisfazem, na prática, apenas os casos de uso de *longing* e *shorting* de ativos financeiros (basicamente, apostar na valorização ou desvalorização de tokens ERC-20 ou outras criptomoedas).

Mas no futuro, com a popularização da tokenização de uma varidade de bens físicos (como imóveis ou veículos), os casos de uso de fluxo de caixa (capital de giro) também serão satisfeitos.

#### Alguns ativos que já foram tokenizados e fazem parte de DeFi
* Dólar americano (US$) por meio de tokens [DAI](https://etherscan.io/token/0x6b175474e89094c44da98b954eedeac495271d0f), [USDC](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48), entre outros
* Real brasileiro (R$) por meio do token [CBRL](https://etherscan.io/token/0xa6fa6531acdf1f9f96eddd66a0f9481e35c2e42a) e outros
* Ouro (XAU) por meio do token [PAX](https://etherscan.io/token/0x45804880de22913dafe09f4980848ece6ecbaf78)
* Além de criptomoedas nativas, como Ether (ETH) e Bitcoin (BTC)

#### Lista de aplicações de DeFi em destaque

Ficou curioso? Compartilho algumas aplicações DeFi que valem ser exploradas:

* [Uniswap](https://uniswap.org)
* [Compound](https://compound.finance)
* [MakerDAO](https://makerdao.com)
* Para mais aplicações de DeFi: [DeFi Pulse](https://defipulse.com/)

[^1]: Liquidez de US$3,47 bilhões reportada em https://info.uniswap.org em 30/01/2021 
[^2]: Liquidez estimada em US$3,2 bilhões, calculada como a diferença entre total fornecido (US$5,7 bilhões) e total emprestado (US$2,5 bilhões), reportados em https://compound.finance/markets em 30/01/2021 
[^3]: Colaterização de US$5 bilhões estimada a partir das informações: "Total DAI Supply = 1,644,933,712.48 DAI", "Collateralization = 308.19%", reportadas em https://oasis.app/borrow em 30/01/2021 
