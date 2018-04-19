---
layout: post
title: "PHP-Lambda functions, Closures, Partials Functions, Currying"
date: 2018-04-18 12:00:25 -0300
categories: deselvolvimento
excerpt: Lambda functions, closures, partials functions, curryng, são conceitos e técnicas existentes a muito tempo na área de ciência da computação, principalmente no estudo do paradigma de desenvolvimento funcional...
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>
<br>

Lambda functions, closures, partials functions, curryng, são conceitos
e técnicas existentes há muito tempo na área de ciência da computação,
principalmente no estudo do paradigma de desenvolvimento funcional.
Muitos destes conceitos vem sendo adotado por linguagens a priori
orientada a objetos, no PHP não é diferente, já a algum tempo podemos
implementar várias destas técnicas.  

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

Desde a versão 5.3 o PHP tem suporte a lambda functions e também
tem suporte a First-Class Functions e High-Order Functions.
First-Class Functions permite tratar uma função como qualquer
outro valor, atribuí-la a uma variável, passá-la como parâmetro,
etc. High-Order Functions significa que uma função pode receber
outra função como argumento e/ou retornar uma função como resultado.
Algumas funções desse tipo que já existem no core do PHP e que são
bastante utilizadas são por exemplo: array_map, array_reduce,
funções de ordenação: usort, uksort.  


<div id="ref-post">
    <a href="{% post_url 2018-02-01-A-vida-com-menos-loops-em-PHP %}">
        <img src="http://i64.tinypic.com/11gn987.jpg">
        <div id="link-title">
            A vida com menos loops em PHP
        </div>
        <div id="link-description">
            O array é umas das estruturas de dados mais conhecidas e
            utilizadas em PHP, um array é na verdade um mapa ordenado,
            uma…
        </div>
    </a>
</div>

<br>

Isso nos permite implementar algumas técnicas de programação
funcional e criar códigos mais expressivos, fáceis de ler e dar
manutenção. Também possibilita uma maior reutilização de códigos,
agilizando o desenvolvimento e deixando o projeto mais consistente.
Vamos ver como funciona e como podemos implementar essas técnicas.  

<br>
<br>

#### __Lambda functions/Funções anônimas__  
Funções anônimas simplesmente permite a criação de uma função sem
nome específico, podendo ser atribuída a uma variável, passada
como argumento para uma função ou como retorno de uma função.  

<script src="https://gist.github.com/Jciel/7ccb4cee9cb89f43018b06d1b5373093.js"></script>

No primeiro exemplo criamos uma função anônima que recebe dois
parâmetros e atribuímos a variável ``$sum``, assim chamamos como uma
função normalmente. No segundo exemplo criamos uma função nomeada
``soma`` que recebe um callback(função) como primeiro parâmetro e mais
dois números como parâmetros, ao chamar essa função passamos como
callback a função anônima ``$sum``, dentro dessa função executamos o
callback passando os dois números restantes.  

<br>
<br>

#### __Closures__   
Uma Closure é semelhante a uma função anônima, a diferença esta no
fato de que uma Closure tem acesso a variáveis do escopo externo.
Para isso o PHP possui a palavra-chave ``use``, possibilitando passar
qualquer variável do escopo externo para dentro da função.  

<script src="https://gist.github.com/Jciel/b9c079958b07f9d46ea888acbc152d15.js"></script>

No primeiro exemplo criamos uma função anônima comum tentando
acessar uma variável do escopo externo, ao tentar executar a
função temos um “PHP Notice” dizendo que a variável ``$name`` dentro
da função não existe. Na segunda função usamos o ``use`` para passar
a varável ``$name`` para dentro da função, assim temos acesso a ela ao
chamarmos a função.  

<br>

Lambda functions/Closures são instancia da classe Closure, esta
classe possui alguns métodos.  

__Closure::call__

Vincula a Closure __temporariamente__ a um objeto/escopo e a chama com
qualquer parâmetro passado.  

<script src="https://gist.github.com/Jciel/2df45cf796819a0d5ec8dca92e53ae05.js"></script>

Na linha 21 e 22 estamos chamando a Closure ``$addAge`` vinculando ao
escopo do objeto ``$person1`` e ``$person2`` respectivamente, e passando o
parâmetro que ela necessita, então ela esta executando como se fosse
um método interno aos respectivos objetos.  

__Closure::bindTo/Closure::bind__

Os métodos bindTo/bind retorna uma nova Closure com um objeto e um
escopo vinculado, __não é temporário__, assim, se o objeto vinculado for
alterado, o resultado da chamada da nova Closure poderá ter seu valor
alterado também. Clojure::bind é uma versão *static* de Closure::bindTo.  

<script src="https://gist.github.com/Jciel/d790d398d3fd6ecfb4eb0da3ba337d70.js"></script>

Nesse exemplo estamos usando os métodos bindTo e bind(*static*) para
vincular definitivo a Closure ``$addAge`` nos objetos ``$person1`` e ``$person2``
respectivamente. Esse método retorna uma nova Closure que estamos
atribuindo a ``$closurePerson1`` e ``$closurePerson2``. Aqui podemos perceber
que se alterarmos a propriedade ``$age`` dos objetos e chamarmos
``$closurePerson1`` e ``$closurePerson2`` novamente o resultado retornado é
diferente.  

__Closure::fromCallable__  

Closure::fromCallable permite converter um Callable(função) em uma
Closure. Para isso precisamos passar a função que queremos converter
como parâmetro para esse método. Essa função verifica se o callable
esta no escopo atual, se for do escopo externo vai lançar um erro.  

<script src="https://gist.github.com/Jciel/51ec13ab0d8b269d2ac5667acd57efaf.js"></script>

<br>
<br>

#### __Partial functions__ 
Utilizando Closures podemos criar algo chamando partials functions.
Partials functions são funções que pegam uma função existente e
reduzem a quantidade de parâmetros ligando um ou mais de seus
parâmetros a um valor específico, retornando uma outra função com
esses parâmetros já definidos. As partials functions permite dividir
em funções mais específicas e compartilhar a funcionalidade central
de uma função mais ampla. O objetivo é poder fixar os argumentos
necessários e retornar outra função com esses valores definidos, e
assim, na chamada da nova função, definir apenas os parâmetros
restantes.  

<script src="https://gist.github.com/Jciel/15f802a21d35a3cc41c8dacbed786fc7.js"></script>

No exemplo acima, praticamente criamos um “constrututor” de funções de
multiplicação. Criamos uma função ``makeMultiply`` que recebe um número
como parâmetro ($x) e retorna uma outra função, mas já fixando um
valor da multiplicação como o parâmetro recebido, assim podemos
construir outras funções mais específicas utilizando esta como base.
Por exemplo, se quisermos uma função que multiplica qualquer número
por 2 (linha 11) ou por 5 (linha 12), podemos usa-la para isso.  

<br>

Podemos criar uma função generalista que retorna uma partial function
de qualquer outra função, assim podemos reutilizá-la sempre que
necessário.  

<script src="https://gist.github.com/Jciel/a042854fe0001f9f4915f9ab13c6a67a.js"></script>

<br>

Assim, possibilita criarmos funções generalista e ir criando as
funções mais específicas quando necessário, o mesmo exemplo de cima.  

<script src="https://gist.github.com/Jciel/2b9bc4b19807823db692d869a682ada4.js"></script>


<br>
<br>

#### __Currying__  
Utilizando Closures também podemos fazer currying de funções.
Currying é uma técnica utilizada para transformar uma função que
recebe múltiplos parâmetros em uma cadeia de funções, cada uma lidando
com apenas um parâmetro da função inicial. Após feito o currying,
uma função rebe o primeiro parâmetro e devolve uma função que aceita
o segundo e assim por diante.  

<script src="https://gist.github.com/Jciel/2e8aeb8872d2b184c83ca83c28eb307f.js"></script>

No primeiro exemplo utilizando uma função comum recebendo três
parâmetros, e retornando a soma dos mesmos. No segundo código,
utilizando currying, separamos a lógica em três funções, cada uma
trabalhando com apenas um parâmetro, assim podemos chamá-la como
uma sequência de funções. Embora se pareça com partials function,
currying e partials functions são conceitos e técnicas diferentes.  

<br>
<br>

#### __Conclusão__  
Acabamos de ver algumas técnicas utilizando lambda functions/Closure
que auxiliam na resolução de problemas, reusabilidade de código e
construção de um código mais limpo e expressivo.  
<br>
Cada caso deve ser avaliado e estudado qual a melhor técnica para
resolver determinado problema, os recursos estão disponíveis,
bastando apenas a aplicação da resolução.  

Referências:  
[https://wiki.php.net/rfc/closurefromcallable](https://wiki.php.net/rfc/closurefromcallable)

[https://wiki.php.net/rfc/closure_apply](https://wiki.php.net/rfc/closure_apply)

Livro [PHP 7 Data Structures and Algorithms](https://medium.com/r/?url=https%3A%2F%2Fwww.amazon.com%2FPHP-Data-Structures-Algorithms-Implement%2Fdp%2F178646389X)

Livro [Pro Functional PHP Programming](https://medium.com/r/?url=https%3A%2F%2Fwww.amazon.com%2FPro-Functional-PHP-Programming-Optimization-ebook%2Fdp%2FB075ZRK5MN%2Fref%3Dsr_1_1%3Fs%3Dbooks%26ie%3DUTF8%26qid%3D1523840834%26sr%3D1-1%26keywords%3DPro%2BFunctional%2BPHP%2BProgramming)
