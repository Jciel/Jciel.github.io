---
layout: post
title: "PHP-Memoization"
date: 2018-03-06 16:58:25 -0300
categories: deselvolvimento
excerpt: Memoization é uma técnica para otimizar chamadas de funções de alto custo de processamento, com isso podemos guardar o resultado de chamadas de funções para uma lista de determinados parâmetros evitando um recalculo de valores já processados...
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

Memoization é uma técnica para otimizar chamadas de funções de alto custo de processamento, com isso podemos guardar o resultado de chamadas de funções para uma lista de determinados parâmetros evitando um recalculo de valores já processados, é fácil imaginar memoization como o cache do resultado de uma função a partir dos seus parâmetros.  

<div class="img-container">
	<figure>
		<img src="{{ site.baseurl }}/images/nate-grant.jpg">
		<figcaption>
		Imagem de Nate Grant em Unsplash
		</figcaption>
	</figure>
</div>

<br>

Memoization está muito ligado ao conceito de função pura e transparência referencial.  
 
>A transparência referencial significa que dado um conjunto de parâmetros de entrada, a função sempre devolverá o mesmo resultado para esse conjunto parâmetros, essa função depende exclusivamente de seus parâmetros de entrada.  

>Uma função pura não deve gerar nenhum efeito colateral(side effects), não deve alterar qualquer estado da aplicação, e deve ter transparência referencial.  

Sabendo que, para uma determinada lista parâmetro de entrada teremos sempre a mesma saída, então podemos criar uma estrutura chave-valor com base nesse parâmetro e salvar este resultado, na próxima chamada dessa função com esses mesmos parâmetros não necessitará fazer todo o processamento novamente pois já temos o resultado no ‘cache’, bastando apenas recuperar esse valor já calculado.  

<br>

__Implementação__

Iremos ver duas formas de fazer isso, salvando o cache em arquivos temporários no sistema e deixando o cache na memória.  

Iremos definir o path para os arquivos de cache  

<script src="https://gist.github.com/Jciel/e85f284760f6e3efd35500859da3bbc3.js"></script>  

<br>

Como vimos acima, funções puras não deve causar side effects, mas precisaremos ler e escrever de um arquivo, então criaremos duas funções separadas para essas tarefas.  

<script src="https://gist.github.com/Jciel/b82b4ad4c0ce527068d97b447f01d972.js"></script>

<script src="https://gist.github.com/Jciel/d47aecd20bb8f5a0dcf685e484ea720a.js"></script>

A função ``getValue`` fica responsável por recuperar o valor de um arquivo quando for chamada, e a função ``storeValue`` guarda o valor em um arquivo de cache quando necessário.  

<br>

Agora criaremos nossa função memoize, ela retorna uma Closure, uma versão ``"memoizada"`` da função callback que for passada, que armazena o valor de retorno em um arquivo de cache e usa o valor do cache caso já exista.

<script src="https://gist.github.com/Jciel/763fbff2cebe9a29340cc3ea2be8a49b.js"></script>


Como visto, uma função pura sempre retorna a mesma saída para a mesma entrada. Na função ``memoize`` criamos um hash único a partir dor parâmetros da função callback. Chamando a função ``getValue`` passando esse hash para verificarmos se já temos o resultado calculado para essa lista de parâmetros, caso já tivermos, retornamos esse valor sem precisar fazer o processamento novamente, senão chamamos a função callback com seus parâmetros e salvamos em cache esse resultado retornando o mesmo.  

<br>

Vamos usar como exemplo um algoritmo de recuperar n-ésimo número na sequência fibonacci, para isso criaremos duas funções, uma simples e outra utilizando a função ``memoize`` que acabamos de criar e vamos comparar o tempo de execução. Como esse algoritmo não é tão dispendioso em relação ao tempo, vamos adicionar um tempo de espera nas funções (unsleep).

<script src="https://gist.github.com/Jciel/8ee1fedaed9775985955acf960c1416f.js"></script>

<script src="https://gist.github.com/Jciel/356fa085857ee47c9e141929e148af37.js"></script>

<br>

Iremos criar uma função para nos ajudar a medir o tempo de execução. Iremos passar as funções ``fib`` e ``fibMemoize`` para essa função que que irá executar e retornar o tempo decorrido. Essa função de captura do tempo arredonda para baixo o tempo final, então são tempos aproximados.

<script src="https://gist.github.com/Jciel/9a7fec5a09ffa3d21f0814da8c074aa2.js"></script>

<br>

Agora vamos comparar o tempo de processamento para alguns parâmetros

<script src="https://gist.github.com/Jciel/f47c6c53df8098856fe951875d22f39a.js"></script>

Quando chamamos a função ``fib`` passando o 6 como parâmetro, executamos duas vezes e ele retornou com pouca diferença de tempo, quando executamos com a função ``memoize`` a primeira vez demorou 1.0021s e guardou em cache esse valor, na segunda vez demorou 0.1003s pois já tínhamos esse valor em cache então só precisou recupera-lo. Executamos passando o 10 como parâmetro e demorou 0.4006s pois até o 6 já tínhamos o valor, então o calculo só precisou ser feito a partir desse ponto. Ao executar passando o 8 como parâmetro retornou em tempo =~0, pois como já executamos essa função até o 10, já temos o 8 calculado também.  
Agora, se olharmos no diretório de arquivos temporários do sistema, existem 10 arquivos onde ficaram guardados os resultado das chamadas da função para cada parâmetro.

<br>

Nos exemplos acima vimos como armazenar o resultado em cache no disco, isso permite um cache persistente e que pode ser acessado por múltiplos processos, mas como sabemos, leitura e escrita em disco são bastante lentas, também o espaço, assim como permissão de leitura/escrita em disco pode ser um problema e as vezes precisamos armazenar esse resultado somente durante uma execução/sessão, então o ideal seria criar esse cache em memória, vamos ver como podemos fazer isso.  

O PHP permite uma maneira de criar variáveis que são restritas a uma determinada função que são chamadas variáveis estáticas, o que precisamos fazer é alterar a função ``memoize`` e criar uma variável estática que servirá de cache para guardar os valores já processados.

<script src="https://gist.github.com/Jciel/326ec80d630cad55a5b4085fca73ac51.js"></script>


Chamando a função com os mesmos exemplos acima.

<script src="https://gist.github.com/Jciel/de328f27dd62d1c9b4ea9deeb9f2994e.js"></script>

Assim temos uma execução relativamente mais rápida por utilizar a memoria RAM, então não temos o tempo de I/O em disco.  

<br>

__Conclusão__  
Memoization pode ser uma boa ideia para otimizar alguns trechos de códigos mais custosos, mas tem algumas desvantagens, como observamos no primeiro exemplo, a função cria um arquivo temporário para cada parâmetro e/ou lista de parâmetros diferentes, isso pode criar uma necessidade de mais memória em disco ou teríamos que criar uma rotina para limpar esses arquivos em um período de tempo caso necessário.  

Se usado a memória RAM também podemos ter algum problema de consumir uma grande quantidade para salvar os resultados da função na variável cache.  

Então o mais ideal seria usar essa técnica pontualmente e fazer alguns testes de consumo dos recursos em alguns casos de grande uso.  

Os códigos de exemplos foram rodados usando o PHP 7.2  


Referência: Livro [Pro PHP Functional Programming](https://www.amazon.com/Pro-Functional-PHP-Programming-Optimization/dp/1484229576)