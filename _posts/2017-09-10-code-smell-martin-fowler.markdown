---
layout:      post
title:       "CodeSmell"
subtitle:    "CodeSmell de Martin Fowler em Português"
description: "CodeSmell de Martin Fowler em Português"
date:        2017-09-10 08:58:00
author:      "Wander Costa"
header-img:  "/img/post-bg-codesmell.jpg"
thumb-img:   "/img/thumb/thumb-codesmell.jpg"
tags:
- code smell
- refactoring
---

This is a post written in Portuguese, as a free translation of [Martin Fowler][martinfowler]'s article [CodeSmell][codesmell]. Translated on September 10, 2017.

Este é um post escrito em Português, como uma tradução livre do artigo [CodeSmell][codesmell] de [Martin Fowler][martinfowler]. Traduzido em 10 de Setembro de 2017.

> #### CodeSmell ######
> Martin Fowler<br>
> 9 de Fevereiro de 2006
>
>
> Um **code smell** (ou **cheiro no código**, em Português) é uma indicação superficial que geralmente corresponde a um problema maior no sistema. O termo foi criado por Kent Beck enquanto me ajudava com o meu livro [Refactoring][refactoring].
> 
> A definição acima contém alguns pontos sutis. Em primeiro lugar, **smell** (ou **cheiro**, em Português) é, por definição, algo que é rápido de ser detectado - ou **fareijável** como já escrevi recentemente. Um método longo é um bom exemplo disso - apenas olhando para o código e meu nariz já se contrai se eu vejo mais de uma dúzia de linhas de java.
> 
> O segundo é que os cheiros nem sempre indicam um problema. Alguns métodos longos são bons. Você deve olhar mais a fundo para ver se há algum problema por trás - os cheiros não são inerentemente ruins por conta própria - eles são muitas vezes um indicador de um problema e não o próprio problema.
> 
> Os melhores cheiros são aqueles fáceis de detectar e que na maioria das vezes levam você a problemas realmente interessantes. Classes de dados (classes só com dados e sem comportamento) são bons exemplos disso. Você olha para elas e se pergunta qual comportamento deveria estar nesta classe. Então você começa a refatorar para mover esse comportamento para ela. Muitas vezes, perguntas simples e refatorações iniciais podem ser o passo vital para transformar objetos anêmicos em algo que realmente tem classe.
> 
> Uma das coisas interesantes sobre cheiros é que são fáceis até mesmo para pessoas inexperientes encontrá-las, mesmo que não saibam o suficiente para avaliar se há um problema real ou para corrigi-los. Ouvi falar de desenvolvedores líderes que escolhem um "cheiro da semana" e pedem às pessoas que busquem o cheiro e os tragam para os membros seniors da equipe. Identificar um cheiro de cada vez é uma boa maneira de gradualmente ensinar as pessoas da equipe a serem melhores programadores.

[pic]:https://martinfowler.com/mf.jpg
[martinfowler]:https://martinfowler.com
[codesmell]:https://martinfowler.com/bliki/CodeSmell.html
[refactoring]:https://martinfowler.com/books/refactoring.html
