---
title: "Machine Learning + Bitcoin: Previsão de candles"
date: 2020-05-23T17:59:22-03:00
categories:
- Machine Learning
- Golang
draft: true
---
# Introdução
Existe certa controvérsia sobre a eficácia da análise técnica de *trading*, já que a teoria da eficiência de mercados estabelece que o preço atual de um mercado sempre reflete todo o conhecimento existente.

Um candle é um recurso visual utilizado em gráficos de séries temporais de preços de um determinado mercado, que representa as seguintes informações de um determinado período de tempo: preço de abertura, preço de fechamento, preço mínimo, preço máximo.

Podemos tentar construir um modelo preditivo de aprendizado de máquina que consiga estimar o preço do próximo candle utilizando os dados do passado, o que indicaria que a teoria da eficiência de mercados não pode ser aplicada a algum mercado.

# Objetivo
Criar um modelo de previsão do próximo candle do mercado de compra e venda de Bitcoins, a partir dos dados de candles anteriores.

# O que vamos fazer

1. Obter dados históricos de candles de 1 hora do par BTC/USD de alguma corretora de criptomoedas, para o ano de 2019 (em 1 ano, existem 8760 candles de 1 hora)
2. Começando do mais recente para o mais antigo, cada candle será utilizado como atributo alvo, e os 100 candles anteriores serão utilizados como atributos preditivos
3. Vamos separar, aleatoriamente, 10% para validação, 20% para teste, e 70% para treinamento

# Operação

## Caderno interativo do Kaggle

Para executar os algoritmos de tratamento de dados, treinamento e teste do modelo, criei um caderno interativo no Kaggle.

## Obtenção dos dados históricos

Felizmente, no Kaggle já existe um dataset curado pelo usuário Zielak contendo candles de período de 1 minuto entre dezembro de 2014 e abril de 2020. Com esses dados, consigo reconstruir os candles de duração de 1 hora para o ano de 2019.

Script no Kaggle

https://www.linkedin.com/pulse/how-use-machine-learning-time-series-forecasting-vegard-flovik-phd-1f/
