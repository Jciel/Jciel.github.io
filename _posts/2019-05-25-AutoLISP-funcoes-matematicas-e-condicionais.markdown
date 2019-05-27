---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg"
title: "AutoLISP - Funções matemáticas, condicionais e controle de fluxo"
subtitle: 
date: 2019-05-25 12:00:25 -0300
categories: deselvolvimento
excerpt: ...vamos ver que existem muito mais coisas para explorar e que podem ajudar a desenvolver uma rotina mais robusta 
         e poderosa para atumatizar muitas tarefas dentro do ambiente do AutoCAD....
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>

Depois de ver uma introdução sobre AutoLISP no primeiro texto ([Introdução a AutoLISP]({% post_url 2019-03-14-Introdução a AutoLISP %}))  vamos 
ver que existem muito mais coisas para explorar e que podem ajudar a desenvolver 
uma rotina mais robusta e poderosa para atumatizar muitas tarefas dentro do 
ambiente do AutoCAD.
  

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __Operadores e funções matemáticos__  
Em AutoLisp, assim como na maioria das linguagens de programação, temos os operadores utilizados em cálculos matemáticos 
que realizam as operações básicas de soma (+), subtração (-), multiplicação (*), e divisão (/). Também temos os 
operadores de incremento (+) e decremento (-), apesar de chamar de operadores, em AutoLISP eles tambem são funções.

<br>

Vamos ver alguns exemplos utilizando operadores matemáticos, como vimos no artigo anterior, o AutoLISP utiliza a notação 
prefixa, ou seja, o operador vem antes dos operandos.  


```cl
(+ 22) ; 4
(- 5 2) ; 3
(* 2 3) ; 6
(/ 8 4) ; 2
```

E os operadores de incremento e decremento.  
```cl
(1+) ; 2
(1-) ; 0
```
<br>

Em alguns casos precisamos executar alguns cálculos mais complexos e talvez seja difícil executá-los apenas com as 
operações básicas, então o AutoLISP disponibiliza algumas funções matemáticas prontas, vamos ver alguns exemplos.  

__``abs``__ – Retorna o valor absoluto de um número.
```cl
(abs -9) ; 9
(abs -41.6) ; 41.6
```  


__``atan``__ – Retorna o arcotangente de um valor. Devemos lembrar que o resultado retornado será sempre em radianos e caso 
passado dois valores por parâmetro é retornado o arcotangente do primeiro valor dividido pelo segundo.  
```cl
(atan 6) ; 1.40
(atan 9 3) ; 1.24
```  

__``cos``__ – Retorna o co-seno de um ângulo passado por parâmetros
```cl
(cos 30) ; 0.15
```  

__``exp``__ – Retorna o antilogaritmo natural de um valor, o resultado será sempre um real.
```cl
(exp 3) ; 20.0855

```  

__``expt``__ – Retorna a exponenciação de um valor elevado a outro.
```cl
(expt 3 2) ; 9
```  

__``gcd``__ – Retorna o máximo denominador comum entre dois números.
```cl
(gcd 6 9) ; 3
```  

__``log``__ – Retorna o logaritmo natural de um valor, o resultado será sempre um real
```cl
(log 6) ; 1.79176
```  

__``max``__ – Retorna o maior valor entre dois números.
```cl
(max 9 6) ; 9
```  

__``min``__ – Retorna o menor valor entre dois números.
```cl
(min 6 9) ; 6
```  

__``minusp``__ – Retorna __T__``(true)`` caso o número passado for negativo, caso contrário retorna __nil__``(false)``.
```cl
(minusp 6) ; nil
(minusp -6) ; T
```  

__``rem``__ – Retorna o resto da divisão entre dois valores.
```cl
(rem 10 3) ; 3
(rem 8 4) ; 0
```  

__``sqrt``__ – Retorna a raiz quadrada de um valor, o resultado sempre será um número real.
```cl
(sqrt 9) ; 3.0
```  

__``zerop``__ – Retorna __T__``(true)`` se o número passado for 0, caso contrário retorna __nil__``(false)``.
```cl
(zerop 0) ; T
(zerop 5) ; nil
```  

<br>
Além dessas, existem diversas outras que podem ser vistas na documentação oficial [Arithmetic Functions Reference](http://help.autodesk.com/view/OARX/2019/PTB/?guid=GUID-FC14467C-63B9-4400-8C6A-266D21C846AE).  
Todas essas funções podem ser utilizadas para realizar cálculos matemáticos, vamos ver alguns exemplos:  

- Calcular f = (2 + 4) * (3 -5)  
```cl
(* (+ 2 4) (- 3 5)) ; -12
```  

- Calcular volume de esfera de raio 5. ``V = 4 * π r³ / 3``
```cl
(/ (* (* 4 3.14) (expt 5 3)) 3)
```


<br>


#### __Funções lógicas e de comparação__  


Uma das características de um software é poder comparar e verificar resultados e valores podendo assim, tomar alguma 
decisão sobre esses valores, para isso existem algumas funções bastante úteis, essas funções normalmente trabalham com 
dois valores comparando-os e retornando um valor booleano __T__ ``(true)`` ou __nil__ ``(false)``,  vamos ver algumas 
delas.  

<br>

__``=``__ Retorna __T__``(true)`` se __todos__ os valores forem __iguais__.  
```cl
(= 2 2) ; T
(= 2 3) ; nil
```  

<br>

__``/=``__ Compara dois valores e retorna __T__``(true)`` se __ambos__ forem __diferentes__.
```cl
(/= 2 2) ; nil
(/= 2 3) ; T
```  

<br>

__``<``__ Compara dois valores e retorna __T__``(true)`` se o primeiro for __menor__ que o segundo.
```cl
(< 2 6) ; T
(< 6 2) ; nil
```  

<br>

__``<=``__ Compara dois valores e retorna __T__``(true)`` se o primeiro for __menor__ ou __igual__ ao segundo.
```cl
(<= 2 6) ; T
(<= 6 6) ; T
(<= 6 2) ; nil
```  

<br>

__``>``__ Compara dois valores e retorna __T__``(true)`` caso o primeiro for __maior__ que o segundo.
```cl
(> 6 2) ; T
(> 2 6) ; nil
```  

<br>

__``>=``__ Compara dosi valores e retorna __T__``(true)`` caso o primeiro for __maior__ ou __igual__ ao segundo.
```cl
(>= 6 2) ; T
(>= 6 6) ; T
(>= 2 6) ; nil
```  

<br>

__``and``__ Compara duas expressões e retorna __T__``(true)`` se __ambas__ forem __T__``(true)``.
```cl
(and T T) ; T
```  
```cl
(setq um (= 1 1)) ; comparamos dois números e definimos o resultado (T) na variável um.
(setq dois (= 2 2)) ; comparamos dois números e definimos o resultado na variável dois.
(setq tres (= 2 3)) ; ; comparamos dois números e definimos o resultado (nil) na variável tres
(and um dois) ; T
(and um tres) ; nil
(and (= 1 1) (= 2 2)) ; T
```  

<br>

__``or``__ Compara duas ou mais expressões e retorna __T__``(true)`` se pelo menos __uma__ das expressões for __T__``(true)``.
```cl
(or T T nill) ; T
(or (= 1 3) (= 5 6) ; nil
```  

<br>

__``not``__ Retorna __T__``(true)`` se o valor passado for __nil__.
```cl
(not nil) ; T
(not T) ; nil
```  
```cl
(setq v1 (= 1 1)) ; T
(setq v2 (= 3 5)) ; nil

(not v1) ; nil
(not v2) ; T
```  

<br>

Vamor ver alguns exemplos, podem ser executados diretamente no prompt de comandos do AutoCAD, mas antes tente resolver 
sem executar. :)  
```cl
(<= 5 5)

(not (= 5 5))

(and (= 5 6) (< 2 5))

(or (= "a" "a") (< 5 3))

(or (and (/= 5 6) (< 3 8)) (and (/= 6 6) (< 9 5)))

(and (or (< 7 9) (< 9 7)) (or (/= 5 5) (= 6 6)))
```

<br>

#### __Controle do fluxo de execução__  

Outra característica de um software é poder tomar decisões conforme os dados são processados e assim poder escolher as 
próximas operações a serem executadas, para isso o AutoLISP disponibiliza algumas funções para implementação dessas 
funcionalidades, vamos ver algumas das mais importantes e utilizadas.  

<br>


__``if``__ É uma função condicional usada para avaliar uma expressão (condição), caso a condição seja verdadeira 
__T__``(true)`` a expressão logo a seguir é executada, caso contrário, é evitado a expressão seguinte e executado a 
próxima.  


Nesse pequeno pseudocódigo, caso a condição seja __T__``(true)`` é executada a __operação 1__, caso contrário é 
executada a __operação 2__.  
```cl
(if (condição)
    (operação 1)
    (operação 2))
```  

Vamos ver alguns exemplos.  
```cl
(if (< 1 2)
   (setq a 5)
   (setq a 10)) ; Qual o valor de a?
```  
```cl
(if (and (= 2 2) (>= 6 6))
    (setq b 9)
    (setq b 3)) ; Qual o valor de b
```  
```cl
(setq idade (getint "\nDigite sua idade: "))
(if (> idade 18)
    (print "Voce é maior de idade")
    (print "Voce é menor de idade"))
```  

<br>

Como vimos, a função if executa apenas uma expressão em cada caso, mas e se precisasse executar varias expressões? Para 
isso temos a função __progm__.  

A função progm é muito utilizada junto a função if e permite que seja possível a execução de varias expressões agrupadas 
como se fosse uma única expressão.  
```cl
(if (T)
    (progm
        (expressão 1)
        (expressão 2)
        (expressão 3))
    (progm
        (expressão 4)
        (expressão 5)
        (expressão 6)))
```  

```cl
(setq strA (getstring "Digite a primeira palavra: "))
(setq strB (getstring "Digite a segunda palavra: "))
(if (= (strlen strA) (strlen strB))
    (progn
        (print "As duas palavras tem o mesmo tamanho")
        (print "O tamanho das palavras é: ")
        (print (strlen strA)))
    (progn
        (print "As palavras tem tamnhos diferentes")
        (print "O tamanho da primeira palavra é: ")
        (print (strlen strA))
        (print "O Tamanho da segunda palavra é: ")
        (print (strlen strB))))
```  

<br>


__``cond``__ É uma função condicional que avalia qual das opções é verdadeira e executa uma expressão correspondente, 
__cond__ permite que seja testada múltiplas condições.  
```cl
(cond
    ((<condição>) (expressão 1))
    ((<condição>) (expressão 2))
    ((<condição>) (expressão 3))
    ((<condição>) (expressão 4)))
```
```cl
(initget 1 "E D C B")
(setq direcao (getkword "\nEscolha a direção da cota [Esquerda/Direita/Cima/Baixo] "))
(cond
    ((= direcao "e") (print "Voce escloheu Esquerda"))
    ((= direcao "d") (print "Voce escloheu Direita"))
    ((= direcao "c") (print "Voce escloheu Cima"))
    ((= direcao "b") (print "Voce escloheu Baixo")))
```  

<br>


#### __Conclusão__  
Funções condicionais provavelmente são uma das coisas mais importante e utilizadas para desenvolver uma rotina AutoLISP 
mais robusta que permite a própria rotina decidir o que fazer de acordo com uma dado digitado pelo usuário por exemplo 
ou um dado criado a partir de um cálculo matemático, podendo ser utilizada diversas funções matemáticas já existentes 
para esse cálculo, isso abre diversas possibilidades que veremos nos próximos textos.

<br>
<br>

Se ainda não leu o primeiro texto sobre AutoLISP segue o link.  
<div id="ref-post">
    <a href="{% post_url 2019-03-14-Introdução a AutoLISP %}">
        <img src="https://upload.wikimedia.org/wikipedia/commons/0/0e/Cartesian-coordinate-system.svg">
        <div id="link-title">
            <strong>Introdução a AutoLISP</strong>
        </div>
        <div id="link-description">
            O AutoLISP é um dialeto da linguagem LISP, uma das mais antigas e utilizadas até hoje, o AutoLISP foi incorporado no 
            AutoCAD em 1996 e tem sido amplamente utilizado para customização do ambiente...
        </div>
    </a>
</div>

<br>




##### __Referências:__
- GAÁL, José Alberto. Curso de AutoLISP. Campinas, SP: Desecad, 1997. 251 p.
- KRAMER, William. Programando em AUtoLISP. São Paulo, SP: Makron Books, s/d, 274 p.
- AUTODESK. AutoCAD Developer Documentation. 2019. Disponível em: <https://help.autodesk.com/view/OARX/2019/PTB>