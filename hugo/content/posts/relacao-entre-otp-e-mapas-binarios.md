---
title: "Relação entre OTP e Mapas Binários"
date: "2012-11-14"
---

Em \[1\], o autor mostra a relação entre o One-Time Pad e Mapas Binários, provando que a cifragem com OTP pode ser entendida como a obtenção das condições iniciais de um par de mapas binários (da família de Séries Generalizadas de Luröth - GLS).

Com essas informações, percebo agora que, para uma cifra ser resistente aos principais ataques, os seguintes requisitos devem ser satisfeitos:

- Amplo keyspace (intervalo de condições iniciais)

- Torna inviável ataques de força bruta

- Utilização de um mapa binário ou equivalente GLS na geração da keystream

- Garante que a operação seja análoga ao OTP

- Keystream sensível às condições iniciais

- A mensagem (ou até mesmo parte da mensagem) deve ser recuperada se e somente se as condições iniciais forem exatamente as mesmas

- Keystream sensível à mensagem

- Garante que o keystream gerado por uma mensagem pura conhecida (known plaintext) não possa ser determinado e aplicado na decodificação de outra mensagem

Meu principal objetivo atual é fazer com que meu sistema de criptografia satisfaça esse último requisito.

_Referências_

\[1\] Nagaraj, Nithin (2012-11-01). One-Time Pad as a nonlinear dynamical system . _17_, 4029-4036. [http://dx.doi.org/10.1016/j.cnsns.2012.03.020](http://dx.doi.org/10.1016/j.cnsns.2012.03.020)
