---
title: Anotações sobre o livro "User Story Mapping" de Jeff Patton
date: 2021-08-10T15:41:07-03:00
---
A técnica de se utilizar "histórias" como ganchos para iniciar conversas sobre usuários, requisitos,
funcionalidades e soluções de produtos digitais, substituindo especificações formais e extensas, foi
concebida por Kent Beck na década de 90 como parte de
sua metodologia Extreme Programming.

Mais de uma década depois, buscando conciliar "histórias" de alto nível com outras
extremamente detalhadas e técnicas, Jeff Patton desenvolveu a técnica que
denominou "Story Mapping"
e a apresentou no livro "User Story Mapping: Discover the Whole Story, Build the Right Product".

Na obra, Jeff revela os mecanismos por trás da técnica de
mapeamento de histórias, e revisa a origem e fundamentos do conceito "histórias",
que ao longo dos anos foram corrompidos e mal compreendidos.

Seguem abaixo algumas anotações sobre o livro.

<pre>
User Story Mapping

Autores
    Jeff Patton
    Peter Economy

Índice
    p. 1 - Capa
    p. 2 - Foreword by Martin Fowler
    p. 5 - Foreword by Alan Cooper
    p. 8 - Foreword by Marty Cagan
    p. 12 - Preface
    p. 21 - Read This First
    p. 42 - Chapter 1. The Big Picture
    p. 65 - Chapter 2. Plan to Build Less
    p. 81 - Chapter 3. Plan to Learn Faster
    p. 96 - Chapter 4. Plan to Finish on Time
    p. 113 - Chapter 5. You Already Know How
    p. 138 - Chapter 6. The Real Story About Stories
    p. 148 - Chapter 7. Telling Better Stories
    p. 163 - Chapter 8. It's Not All on the Card
    p. 177 - Chapter 9. The Card is Just the Beggining
    p. 186 - Chapter 10. Bake Stories Like Cake
    p. 194 - Chapter 11. Rock Breaking
    p. 217 - Chapter 12. Rock Breakers
    p. 230 - Chapter 13. Start with Opportunities
    p. 245 - Chapter 14. Using Discovery to Build Shared Understanding
    p. 269 - Chapter 15. Using Discovery for Validated Learning
    p. 288 - Chapter 16. Refine, Define, and Build
    p. 314 - Chapter 17. Stories Are Actually Like Asteroids
    p. 322 - Chapter 18 - Learn from Everything You Build
    p. 335 - The End, or Is It?
    p. 336 - Acknowledgments
    p. 340 - References
    p. 342 - Index
    p. 384 - Last page

---

p. 3 - Foreword By Martin Fowler
    Quebrar trabalho em histórias pequenas tem vantagens para o usuário, que não precisa esperar anos para ver o avanço
    Mas há consequências negativas: fácil de perder o panorama global. Pedaços sem coerência
    Story mapping é uma técnica que preserva esse panorama global
    Jeff Patton criou o método de "story mapping"
    Maior decepção na última década do Ágil: muitos programadores enxergam histórias como um caminho de mão única, sendo que deveriam enxergá-las como gatilhos para iniciar conversação
    Kent Beck concebeu a noção de "história"

p. 5 - Foreword by Alan Cooper
    Opinião similar a de Martin Fowler, que é impossível construir "histórias" soltas e esperar que usuários gostem do produto
    É muito difícil expressar um problema de design em termos de programação, e vice-e-versa
    "Despite protestations to the contrary, Agile development is not a very useful design tool"
    É fácil fazer pequenos produtos que as pessoas amam com times pequenos
    O desafio é fazer produtos grandes que as pessoas amam

p. 8 - Foreword by Marty Cagan
    Diferença entre times bons vs times ruins

p. 12 - Prefácio
    Além de apresentar a técnica de Story Mapping, vai falar sobre os fundamentais de User Stories, pois são frequentemente má utilizadas

    Armadilhas comuns com stories:
        Perda do panorama global
        Perda da noção do tempo total necessário para finalizar tudo
        Perda de documentação, já que histórias são gatilhos para conversas informais
        Times não finalizam dentro do tempo planejado, por falta de documentação explícita e bom planejamento
        User stories escondem muitas partes invisíveis do sistema

    São erros conceituais que nos fazem cair nas armadilhas acima

p. 16 - Quem deve ler esse livro?
    PMs, UX Designers, PO, Business Analysts, Project Managers
    Coaches de Ágil e Lean

p. 18 - Conteúdo dos capítulos
    Capítulos 1 a 4 - Visão de alto nível de Story Mapping
    Capítulo 5 - Exercício prático
    Capítulos 6 a 12 - Fundamentos de "Story"
    Capítulos 13 a 15 - Ciclo de vida de uma "Story" e gestão de backlog
    Capítulos 16 a 18 - Utilização tática de stories (no momento de executar/consumir)

p. 21 - Read This First
    O objetivo de utilizar "stories" não é escrever melhores histórias
    O objetivo de desenvolver produtos não é construir produtos
    Telefone sem fio: http://cakewrecks.com/
    NASA: sistema métrico vs imperial
    Documentos compartilhados não são entendimento compartilhado
    Para avaliar entendimento: todos devem externalizar sua interpretação e ideias, e os outros devem fazer perguntas

    Não há documentos perfeitos; não é nem culpa do escritor, nem do leitor
    Documente o mínimo para começar, e em seguida passe para conversas, palavras e imagens para construir o entendimento compartilhado
    Se você está utilizando histórias no desenvolvimento, e não estão conversando usando palavras e imagens, vocês estão fazendo errado
    Documentos auxiliares são menos formais, mas existem igualmente
    O mais importante não é o que está escrito, e sim o conteúdo que os documentos sinalizam
    Grave vídeos de você explicando a história e as anotações

p. 35 - Software Isn't the Point

    Output vs outcome
    Velocity mede output

    Sempre há mais para ser criado, do que o tempo ou recurso que temos disponível
    Minimize output (saída), maximize outcome (resultados)

    Choosers (quem escolhe/compra seu produto) vs users (usuário final do produto)

p. 42 - Chapter 1. The Big Picture
    "I love Agile development! Every few weeks we see more working software. But it feels like I've lost the big picture."
    Contar histórias; e não, escrever histórias
    Story mapping é um pattern, e não uma invenção. Várias empresas e times chegaram a esse mesmo padrão

    Ideia original de histórias: priorizar entendimento compartilhado em vez de documentos compartilhados

    História sobre o Gary e o "flat backlog" para criar app de colaboração de composição de músicas comerciais

    Talk and doc: enquanto conversa, escreva cartões para externalizar ideias

    Valor de utilizar cartões: comunicação sem utilizar palavras. Reordenar cartões, etc

    Foque em largura (horizontal, visão geral) em vez de profundidade (detalhes)

p. 65 - Chapter 2. Plan to Build Less

    História sobre Globo.com
    Possui deadlines ancorados a eventos externos, como eventos esportivo
    Atrasar não é uma opção, portanto eles são muito bons em entregar no prazo
    Não são mais rápidos que a média: e sim, espertos sobre fazer menos
    Times estavam prestes a cair na "flat backlog trap": faltava a dependência entre os times

p. 75

    Story mapping normalmente evidencia quantidade enorme de backlog
    Trabalhe em releases limitados, que façam sentido
    Foque em outcomes, não output
    
    MVP não é o "pior produto que você pode lançar"

p. 80

    MVP é o menor produto que você pode criar para comprovar ou desprovar uma hipótese

p. 82 - Chapter 3. Plan to Learn Faster

    Uma das dificuldades em ser product owner é assumir a ideia de outra pessoa e ajudá-la a ter sucesso, ou então provar que provavelmente não será

    Perguntas-chave:
    * Qual é a grande ideia?
    * Quem são os clientes?
    * Quem são os usuários?
    * Por que eles querem isso?
    * Por que estamos construindo isso?

p. 91 - Como não fazer

    Mostra exemplo de "releases" de um automóvel: primeiro apenas o pneu, depois chassi, depois o casco e por último o volante 

    Essa estratégia é fracassada, pois somente no 4º release temos algo utilizável

    Alternativa: skate, patinete, bicicleta, motocicleta, carro

p. 96 - Chapter 4. Plan to Finish on Time

    Estimativas são incertas

    Mas quebrando em pedaços pequenos, conseguimos medir os pedaços que já concluímos, e como compensar as estimativas

    Sinalize os riscos no story map

    Mona Lisa: progressão geral vs incrementos

p. 113 - Chapter 5. You Already Know How

p. 138 - Chapter 6. The Real Story About Stories

    Ideia original de "histórias" concebida por Kent Beck, autor do Extreme Programming, na década de 90

    Reclamação mais comum de times é "requisitos ruins"

    Todos podem ler o mesmo documento, mas têm um entendimento diferente dele

    Requisitos descrevem "o que" queremos, e não "por que" queremos. Aumenta o risco de construir o produto errado

    "Stories get their name from how they're supposed to be used, not from what you're trying to write down"

    Card -> Conversation -> Confirmation (acceptance criteria agreement)

    Conversa != monólogo

    Acceptance criteria (story tests): lista de itens para validar o resultado

p. 148 - Telling Better Stories

    Originalmente stories (e não user stories)

    Ideia de template:
    "As a [type of user]
    I want to [do something]
    So that I can [get some benefit]"

    Cuidado para não virar um zumbi de template!

    Relato de ThoughtWorks e backlog grooming em time de 25 pessoas

    Template é uma ótima prática de aprendizado, mas não necessariamente uma boa prática definitiva

p. 158

    Sobre o que conversar nas histórias:
    * Quem
    * O que
    * Por que
    * Contextualize o mundo fora do software
    * O que pode dar errado 
    * Perguntas e hipóteses
    * Alternativas de soluções
    * Como
    * Duração

p. 163 - Chapter 8. It's Not All on the Card

    Pessoas diferentes do time == conversas diferentes

    Product Owner ou Product Manager
    !=
    Project Manager
    !=
    Business Analyst
    !=
    User Experience Designer
    !=
    Developer
    !=
    Tester

    Experiência de utilizar as paredes do escritório como "grandes telas" que irradiam cultura e informação permanentemente
</pre>
