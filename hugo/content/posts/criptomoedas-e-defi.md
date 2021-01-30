---
title: "Finanças Descentralizadas: DeFi, Criptomoedas e Blockchain"
date: 2021-01-27T20:20:54-03:00
categories:
- Blockchain
- Finanças
draft: true
---

## Introdução a DeFi 

Nos últimos dois anos têm-se falado muito sobre DeFi: <i>Decentralized Finance</i> ou Finanças Descentralizadas.

Esse é nome que se dá para um conjunto de contratos inteligentes (<i>smart contracts</i>), comumente executados na <i>blockchain</i> Ethereum, mas não limitados a ela. Esses contratos fornecem de forma impessoal (M2C: <i>Machine To Consumer</i>) uma gama de serviços financeiros descentralizados vinculados à criptomoeda Ether e aos tokens ECR20 existentes na mesma rede.

Aplicações de DeFi trazem produtividade às <i>blockchains</i> e criptomoedas, gerando serviços de valor mensurável financeiramente a todos os usuários do ecossistema.

## Como funciona o mercado financeiro tradicional?

O mercado financeiro tradicional é intermediado por agentes financeiros. Algumas das funções frequentemente assumidas por esses intermediários são:

1) **market-making** (criadores de mercado): conectar compradores e vendedores de serviços financeiros
2) **mediador de confiança mútua**: garantir (ou se esforçar para) que ambas as partes (compradores e vendedores) recebam o acordado
3) **criadores de <i>pools</i>** minimizadoras de <i>premium</i> de risco

### Transações sem agentes financeiros (antes de DeFi)
Se uma pessoa física desejasse vender um serviço financeiro (conceder um empréstimo, por exemplo) sem passar por um agente financeiro, precisaria:
1) Encontrar alguém interessado em tomar o seu empréstimo, e que concorde com as condições do serviço, como taxa de juros, prazo e forma de pagamento (**market-making**)
2) Confiar diretamente na outra pessoa (nesse caso **não haveria mediador de confiança mútua**), ou então utilizar a justiça tradicional como mediadora por meio de **contratos**
3) Avaliar o **<i>premium</i> de risco elevado**. Por se tratar de transação entre um único vendedor e um único comprador, a variância (e desvio padrão) da taxa de insolvência (<i>default</i>) possui alta amplitude. A taxa oscila facilmente entre os dois extremos de 0% (insolvência nula) e 100% (insolvência total)

### Desvantagens ao se utilizar agentes financeiros tradicionais
* Custo de intermediação (<i>spread</i>, taxas administrativas)
* Confiança mútua questionável

## DeFi possibilita a compra e venda de serviços financeiros sem o intermédio de agentes financeiros
1) **market-making**: aplicações descentralizadas conseguem armazenar, na <i>blockchain</i>, a lista de compradores e vendedores para um determinado produto financeiro, e executar o *match-making* de forma automatizada
2) **mediador de confiança mútua**: <i>smart contracts</i> são contratos imutáveis, auditáveis, construídos na forma de algoritmos, nos quais todos os termos do produto financeiro são especificados explicitamente, sem qualquer tipo de subjetividade.
3) **criadores de <i>pools</i>** minimizadoras de <i>premium</i> de risco: <i>smart contracts</i> possibilitam a criação de <i>pools</i> descentralizadas

### Empréstimos sem garantia?

O leitor atento percebeu que quando descrevi as transações financeiras (antes de DeFi), omiti sobre a necessidade de avaliação de crédito e a possível utilização de garantias (colaterização) nesse processo.

Mas é importante deixar claro que, hoje, a maior parte das soluções de DeFi depende disso.

O mercado de empréstimo tradicional costuma se fundamentar nos 4 C's (em inglês):
* **Collateral (garantia)**: o tomador do empréstimo disponibiliza algum bem como garantia, como imóveis ou outros direitos
* **Capital**: o tomador do empréstimo comprova possuir capital suficiente (proporcionalmente ao empréstimo a ser tomado), por meio de extratos bancários e demais documentos de propriedade
* **Capacity (capacidade/potencial)**: o tomador do empréstimo é avaliado pela sua capacidade/potencial de gerar retorno a partir do empréstimo. É o caso de empreendedores que recebem crédito apresentando um modelo de negócio robusto
* **Character (caráter)**: o tomador do empréstimo é avaliado pela sua "honra", ou, em outras palavras, pelo seu histórico de respeito a contratos e pelo seu histórico de pagamentos

As primeiras aplicações de empréstimo de DeFi se baseiam principalmente no 1º C (*collateral*), mas já há avanço quanto às outras formas de avaliação de crédito, também. Se você quiser saber um pouco mais sobre esse assunto, recomendo esse [artigo da Coindesk](https://www.coindesk.com/aave-unsecured-borrowing-defi).

No momento, aplicações de empréstimos DeFi baseados em garantia (*collateral*) satisfazem, na prática, apenas os casos de uso de *longing* e *shorting* de ativos financeiros (basicamente, apostar na valorização ou desvalorização de tokens ERC20 ou outras criptomoedas).

Mas no futuro, com a popularização da tokenização de uma varidade de bens físicos (como imóveis ou veículos), os casos de uso de fluxo de caixa (capital de giro) também serão satisfeitos.

## Lista de aplicações de DeFi em destaque

Ficou curioso? Compartilho algumas aplicações DeFi que valem ser estudadas:

* [UniSwap](https://uniswap.org)
* [Compound](https://compound.finance)
* [MakerDAO](https://makerdao.com)

(Irei publicar novos artigos sobre cada uma dessas aplicações em breve!)
