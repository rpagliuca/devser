---
title: "Serverless do zero"
date: 2020-05-23T09:18:17-03:00
categories:
- Serverless
- Golang
---

# Introdução
Antes desse artigo, eu nunca havia programado nenhuma aplicação serverless do zero. Entretanto, já tive algum contato com aplicações desse tipo:

1. Há uns 2 anos instalei, na AWS, uma aplicação de redimensionamento de imagens com AWS Lambda Functions (https://aws.amazon.com/pt/solutions/implementations/serverless-image-handler/).
2. Ah, também já instalei Lambda Functions para limpar o cache do CloudFront, que é chamado em um dos stages do AWS Code Pipeline de um de nossos projetos.

Mas minha experiência terminava aí.

Para mudar isso, hoje vou criar uma aplicação *serverless* do zero na minha conta pessoal da AWS.

Minha missão: ao acessar uma URL pública, quero exibir um JSON contendo a cotação do Bitcoin.

# Como começar

Acessei a minha conta pessoal no console da AWS (https://console.aws.com), e entrei na página do serviço Lambda.

Cliquei no botão `Create Function`, depois em `Author from scratch`, dei o nome para a minha nova função de `bitcoinQuote` e escolhi o Runtime `Go 1.x`. Antes de confirmar a criação, o console da AWS me avisou que criaria um novo `IAM Role` com permissões de adicionar logs no `CloudWatch Logs`, mas que eu poderia mudar isso se eu quisesse.

Tudo bem, eu topei o *role* padrão. No futuro, se eu precisar dar acesso a outros recursos para a minha `Lambda Function`, como por exemplo a algum bucket do S3, eu precisarei vincular mais permissões a esse `IAM Role`.

Depois de confirmar a criação do `bitcoinQuote`, me deparei com a tela abaixo.

![](/images/serverless-do-zero/094138.png)

A primeira coisa que eu fiz foi dar uma fuçada nas seções `+ Add trigger` e `+ Add destination`. Achei interessante, e percebi que provavelmente vou precisar adicionar um trigger do `API Gateway` para expor a minha função para o mundo externo. Mas por enquanto ainda não vou mexer em nada disso.

Rolei a tela para baixo, e vi que existe uma seção para upload de código (`Function code`), e também uma seção de configuração de variáveis de ambiente.

Tenho **certeza** de que em um workflow produtivo de desenvolvimento serverless, não faria sentido usarmos nenhuma dessas funcionalidades manuais via `AWS Console`, já que tudo seria realizado automaticamente via chamadas de API, por alguma ferramenta de CI/CD.

Mas para essa exploração inicial, será útil.

Além desses blocos de configuração que já comentei, existem algumas outras seções que não parecem relevantes para mim neste primeiro momento, como Tags, AWS X-Ray, VPC, Concurrency, Asynchronous invocation e Database proxies.

Bem, acho que já explorei o bastante essa tela de configuração, para entender suficientemente como tudo se encaixa.

# Esqueleto inicial em Go

Pesquisando na documentação da AWS sobre como integrar um programa em Go ao serviço Lambda, encontrei esse artigo: https://docs.aws.amazon.com/lambda/latest/dg/golang-handler.html.

Lá encontro extamente o que eu queria, um esqueleto mínimo:

```
package main

import (
  "fmt"
  "context"
  "github.com/aws/aws-lambda-go/lambda"
)

type MyEvent struct {
  Name string `json:"name"`
}

func HandleRequest(ctx context.Context, name MyEvent) (string, error) {
  return fmt.Sprintf("Hello %s!", name.Name ), nil
}

func main() {
  lambda.Start(HandleRequest)
}
```


Adaptando esse esqueleto, adicionando apenas algumas mudanças básicas, escrevi o meu primeiro programa:

```
package main

import (
  "encoding/json"
  "fmt"
  "context"
  "github.com/aws/aws-lambda-go/lambda"
)

type InputEvent struct {
  Name string `json:"name"`
}

type Output struct {
  Success bool `json:"success"`
  Message string `json:"message"`
}

func HandleRequest(ctx context.Context, inputEvent InputEvent) (string, error) {
  data := Output{
    Success: true,
    Message: fmt.Sprintf("Hello %s!", inputEvent.Name),
  }
  json, err := json.Marshal(&data)
  return string(json), err
}

func main() {
  lambda.Start(HandleRequest)
}
```

A ideia é receber um JSON via input, e responder com outro JSON.

# Deploy da aplicação

No meu ambiente local, rodei os seguintes comandos:

```
# Compilar main.go para o binário bitcoinQuote
go build .
# Criar bicoinQuote.zip contendo apenas o binário
zip bitcoinQuote.zip bitcoinQuote
```

Criei um arquivo .ZIP (`zip bitcoinQuote.zip main.go`)contendo apenas o arquivo main.go (código acima), e enviei pelo formulário `Function code` do console da AWS.

*IMPORTANTE*: No formulário de upload, precisei alterar o nome do `Handler` para bitcoinQuote, já que esse é nome do binário dentro do zip.

Em seguida, cliquei em `Save` no topo da página.

# Teste da aplicação

Cliquei na opção `Select a test event` na parte superior da tela, e em seguida em `Configure test events`. Em `EventName`, dei o nome OlaRafael, com o seguinte JSON:

```
{
  "name": "Rafael"
}
```

Depois de salvar, selecionei o evento `OlaRafael` no dropdown superior da tela, e depois no botão `Test`.

![](/images/serverless-do-zero/101920.png)

Tudo certo!

Na imagem abaixo, dá pra ver que ao enviar esse payload de teste contendo `"name": "Rafael"`, a função respondeu corretamente com um JSON contendo `"message": "Hello Rafael!"`.

![](/images/serverless-do-zero/101940.png)

# Configuração do API Gateway

Agora eu quero expor essa função serverless em uma URL pública. Para isso, vou configurar o API Gateway.

No bloco `Designer` da tela de configuração da minha função Lambda, cliquei em `+ Add Trigger`, e escolhi o tipo `API Gateway`. Optei pela opção `Create an API`.

![](/images/serverless-do-zero/102200.png)

Em `Security`, escolhi a opção `Open`, já que meu desejo é tornar minha API pública, sem autenticação. Para me ajudar a avaliar o que está acontecendo, cliquei também em `Additional settings` e habilitei a opção `Enable detailed metrics`.

Para finalizar, cliquei no botão `Add`.

Agora, ao clicar no item `API Gateway` dentro do painel `Designer`, eu consigo enxergar o endpoint que foi criado para a minha API. Para voltar às configurações anteriores (`Function code`, `Environment variables`, etc), é só clicar em `bitcoinQuote` dentro do painel `Designer`.

Agora eu consigo acessar, pelo navegador, a URL gerada para a minha API: `https://ou0kxsbmjf.execute-api.us-east-1.amazonaws.com/default/bitcoinQuote`.

# Criação de scripts para deploy

Foi didático fazer o deploy manualmente fazendo upload do arquivo ZIP na página do `Lambda`, mas tarefas manuais repetitivas são chatas e com grande propensão a erro.

Agora vamos usar o `aws-cli` para fazer o deploy das próximas versões.

O meu `aws-cli` já está configurado na minha máquina local, então vou rodar o comando `aws help` para ver se consigo encontrar o comando necessário para realizar o deploy. Encontrei o comando `aws lambda`. Agora vamos ver o help dele (`aws lambda help`). Dentre diversos comandos disponíveis, um nome em especial me chamou a atenção: `aws lambda update-function-code`.

Após ler o help desse comando, consegui utilizá-lo com sucesso!
`go build . && zip bitcoinQuote.zip bitcoinQuote && aws lambda update-function-code --function-name=bitcoinQuote --zip-file fileb://bitcoinQuote.zip`

# Consultar cotação do Bitcoin

Ótimo, agora vai ser mais prático fazer os deploys durante o próximo desafio: consultar a cotação do Bitcoin.

Uma primeira opção seria já utilizar uma API pronta de Bitcoin. Mas aí não faria muito sentido a gente estar fazendo esta API, só para consultar uma outra, né?

Então vamos para uma segunda opção, fazendo algo diferente: vamos fazer scraping do Google para encontrar a cotação BTC/BRL.

Utilizando a biblioteca *net/http* e *regexp*, é possível obtermos essa informação.

Dica: adicionei o parâmetro *hl=en* na URL de busca do Google para que o HTML retorne em inglês, independentemente da geolocalização do IP do cliente.

```
package main

import (
  "strings"
  "strconv"
  "errors"
  "regexp"
  "io/ioutil"
  "net/http"
  "github.com/aws/aws-lambda-go/lambda"
)

type Output struct {
  Success bool `json:"success"`
  Quote int `json:"bitcoin_price_in_centavos,omitempty"`
  ErrorMessage string `json:"error,omitempty"`
}

func HandleRequest() (Output, error) {
    quote, err := FetchBtcBrlQuote()
    var data Output
    if err == nil {
      data = Output{
        Success: true,
        Quote: quote,
      }
    } else {
      data = Output{
        Success: false,
        ErrorMessage: err.Error(),
      }
    }
    return data, nil
}

func main() {
  lambda.Start(HandleRequest)
}

func FetchBtcBrlQuote() (int, error) {
  response, err := http.Get("https://www.google.com/search?q=btc+brl&hl=en")
  if err != nil {
    return 0, err
  }
  responseBody, err := ioutil.ReadAll(response.Body)
  if err != nil {
    return 0, err
  }
  bodyString := string(responseBody)
  exp, err := regexp.Compile("([0-9.,]+) Brazilian Real")
  if err != nil {
    return 0, err
  }
  matches := exp.FindStringSubmatch(bodyString)
  if len(matches) >= 2 {
    centavos := strings.ReplaceAll(strings.ReplaceAll(matches[1], ".", ""), ",", "")
    centavosInt, err := strconv.Atoi(centavos)
    if err != nil {
      return 0, err
    }
    return centavosInt, nil
  }
  return 0, errors.New("Erro ao localizar cotação BTC/BRL na resposta HTML")
}
```

Após substituir esse código acima, rodei novamente o script de deploy.

Aguardei alguns segundos antes de acessar a URL pública da minha API, e pronto!

![](/images/serverless-do-zero/142736.png)

Tudo funcionando certinho!

# Melhorias futuras

Espero que ao longo desse artigo tenha ficado claro que tudo isso é apenas um teste de conceito.

Então quais seriam as melhorias necessárias no exemplo acima para ficar qualidade de produção?

1. Refatoração do código
2. Testes automatizados
3. Pipeline de build/teste/deploy integrado ao controle de versão (CI + CD)
4. Cache da resposta da API (por alguns minutos, para evitar acessar repetidamente a busca do Google)
5. Setup do ambiente local para facilitar testes funcionais durante o desenvolvimento

Sobre esse último ponto, por curiosidade, para testar esse método no ambiente local, eu substituí a linha `lambda.Start(HandleRequest)` por `fmt.Println(HandleRequest())`, alterando os imports que foram necessários.
