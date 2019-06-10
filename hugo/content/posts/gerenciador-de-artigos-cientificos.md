---
title: "Gerenciador de Artigos Científicos"
date: "2012-11-15"
aliases:
- /2012/11/gerenciador-de-artigos-cientificos/
categories:
- Redes Complexas
---
O software _[I, Librarian](http://www.bioinformatics.org/librarian/)_ tem sido uma mão na roda para mim há alguns meses no gerenciamento dos artigos científicos que possuo.

Antes dele, precisava ficar renomeando os artigos e organizando-os em pastas no disco rígido, e eu mantinha  um arquivo de texto separado contendo o resumo de todos os artigos, para facilitar a busca por temas e palavras-chaves.

Já com o [_I, Librarian_](http://www.bioinformatics.org/librarian/) tudo fica mais fácil. Eu só preciso salvar no software o arquivo PDF do artigo e fornecer sua identificação [Digital Objetct Identifier](http://www.doi.org) (DOI). Com o número DOI, o próprio software busca online e armazena as metainformações do artigo (autores, palavras-chaves, abstract etc).

Além disso, o software possibilita que os artigos sejam listados e filtrados das mais diferentes formas, garantindo que o artigo desejado seja rapidamente encontrado.

Muito prática também é sua funcionalidade de exportar as referências para vários formatos: texto puro, Bibtext, CSV, EndNote, RIS etc. O que eu faço é exportar toda a minha biblioteca para um arquivo Bibtex, e, quando desejo citar algum artigo, o próprio _[I, Librarian](http://www.bioinformatics.org/librarian/)_ me fornece qual identificação de citação utilizar no meu documento Latex.

Um detalhe a ser levado em conta é que o software utiliza uma plataforma pouco habitual para a maior parte dos usuários: ele roda no navegador - utilizando servidor Apache, banco de dados MySQL e linguagem PHP. No entanto, isso não deve ser um problema, já que o autor do programa disponibiliza um executável de fácil instalação para o [Ubuntu (Linux)](http://www.ubuntu.com/) e para o Windows.
