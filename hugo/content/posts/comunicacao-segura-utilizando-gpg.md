---
title: "Comunicação segura utilizando GPG"
date: 2017-11-28
categories:
- Criptografia
- Linux
---

(Co-publicado em [Time Raposa](http://www.timeraposa.com.br/2017/11/comunicacao-segura-utilizando-gpg/))

Todo time de T.I. deve ter a preocupação com políticas de acessos a senhas, principalmente em se tratando de credenciais de produção.

Algumas estratégias importantes para garantir a segurança dos sistemas de informações são:

* Rodízio constante de senhas
* Senhas diferentes para cada usuário

Mas um problema corriqueiramente encontrado é como comunicar essas senhas com segurança entre diferentes pessoas do time.

Uma solução para esse problema de comunicação é utilizar certificados GPG, e integrá-los com clientes de e-mail, por exemplo o Thunderbird.


Para isso:

1) Crie uma nova credencial GPG usando um dos dois comandos abaixo:

Em versões mais novas do gpg:

```
gpg --full-generate-key
```

Em versões mais antigas do gpg:

```
gpg --gen-key
```

Nessa etapa, é prudente utilizar o maior tamanho de chave possível (no meu caso, foi 4096). Recomendo utilizar seu e-mail pessoal, pois sua identificação não depende diretamente da organização com que você está envolvido. Se for o caso, posteriormente você pode criar chaves específicas (exemplo: e-mail de sua organização) vinculadas à sua chave pessoal.

2) Envie suas credenciais para um servidor público de chaves GPG. O padrão do Debian é o keys.gnupg.net.

```
gpg --send-key IDCHAVE
```

sendo que o ID da sua chave pode ser obtido a partir do comando

```
gpg --list-keys
```

3) Peça para pessoas do seu círculo social (que possam comprovar pessoalmente se tratar de uma chave legítima) assinarem a sua nova chave. Eles devem fazer o seguinte:

```
gpg --recv-key IDCHAVE
```

Em seguida

```
gpg --sign-key IDCHAVE
```

E por último

```
gpg --send-keys IDCHAVE
```

4) Agora que sua chave já foi assinada por uma quantidade razoável de pessoas do seu círculo social, sua chave passou a ter uma “reputação adequada” para ser usada de forma pública.

5) Instale o addon EnigmaMail no Thunderbird, ele já permitirá usar funcionalidades de encriptar e assinar digitalmente seus próximos e-mails usando as chaves geradas anteriormente.

6) Os comandos

```
gpg --encrypt
```

e

```
gpg --decrypt
```

também podem ser utilizados manualmente direto da linha de comando, passando o conteúdo das mensagens pelo pipe. Exemplo:

```
echo "Encriptando mensagem" | gpg --encrypt
```

Dessa forma, temos um canal seguro para envio de senhas e credenciais dos nossos sitemas de informação.
