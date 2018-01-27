---
layout: post
title: "O que é Clojure"
date: 2017-12-22 14:39:38 -0300
categories: deselvolvimento
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

Quando trabalhava com projetos de estruturas metálica automatizei algumas tarefas usando uma forma de script interno do AutoCAD chamado AutoLISP, mas só funciona no ambiente do software, limitando assim, sua utilização para algo mais geral, algum tempo atrás me deparei com Clojure, percebi que compartilhavam da mesma sintaxe e que existiam grandes empresas como o Nubank que a utilizam e resolvi me aprofundar um pouco mais, segue uma pequena introdução para iniciarmos conhecendo um pouco mais sobre essa nova linguagem.

![clojure logo](https://cdn-images-1.medium.com/max/1371/1*6h-IqXpvLbQemIRedaCrMg.png)

Clojure é uma linguagem de programação dinâmica, de propósito geral com ênfase em programação funcional criada por Rich Hickey. Clojure roda sob a JVM (máquina virtual do Java), compila para bytecode como o Java faz, ainda existe a possibilidade de compilar para a CLR (plataforma .NET) e
ClojureScript que compila para Javascript.
A linguagem possui total interoperabilidade com o Java, isso quer dizer que não é necessário reescrever todo o sistema existente em Java para Clojure, é possível invocar código Java diretamente no código Clojure.

Clojure é um dialeto Lisp, umas das linguagens de programação mais antigas.
Clojure é homoiconic, quer dizer que ela trata o proprio código como dado, sua sintaxe é baseada em S-Expressions (simbolic expression), o código é parseado como uma estrutura de dados pelo interpretador e após compilada
para execução.

> Clojure é um dialeto da linguagem Lisp, e compartilha com o Lisp a filosofia de código-como-dados e um poderoso sistema de macros. Clojure é predominantemente uma linguagem funcional e possui um rico conjunto de estruturas de dados imutáveis e persistentes. Quando o estado mutável é necessário, Clojure oferece um sistema de memória transacional de software e um sistema de agente reativo que garante projetos limpos, corretos e multithread. [Rich Hickey — tradução livre]

Como vimos, Clojure é uma linguagem funcional. O paradigma funcional enfatiza a avaliação de funções (como avaliação de funções matemáticas) e evita estado, diferentemente da programação imperativa por exemplo, que incentiva a alteração de estados, no paradigma funcional a unidade básica
do código é a função, diferente do paradigma OOP onde podemos dizer que a unidade básica são os objetos.
Algumas outras características do paradigma funcional e consequentemente de Clojure são:

* __Imutabilidade__: Evita alteração de estado, não “existe” variável;
* __High-Order function__: Funções que aceitam outras funções como parâmetros e/ou retornam funções como resultado;
* __First-Class Functions__: Funções podem ser usada como valores, atribuídas a uma variável;

Como faz parte da família de dialetos da linguagem Lisp, trazendo muitas de suas características, temos que mudar a forma de pensar ao programar em Clojure, por utilizarmos uma sintaxe diferente do qual estamos acostumados na maioria das linguagens mais populares, e principalmente por entrarmos no mundo do paradigma de programação funcional que já é matéria de estudos a um longo tempo.

Em breve mais relatos sobre essa jornada de aprendizado dessa nova tecnologia, vlw! :)