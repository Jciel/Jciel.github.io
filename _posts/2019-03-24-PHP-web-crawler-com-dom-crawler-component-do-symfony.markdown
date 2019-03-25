---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg"
title: "Web Crawler básico com Dom Crawler Component do Symfony"
subtitle: 
date: 2019-03-24 12:00:25 -0300
categories: deselvolvimento
excerpt: Um Crawler, Web Crawler ou também conhecido como Spider é um mecanismo automatizado que possui a capacidade de 
         navegar entre sites e fazer a “leitura” destas páginas para captura de informações ou outras tarefas necessárias, é a 
         base de muitas ferramentas...
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>


Já trabalhei um tempo com um sistema onde era preciso criar web crawlers para capturar algumas informações de sites 
da web, recentemente comecei a brincar um pouco com um componente do framework Symfony chamado DomCrawler, este 
componente é basicamente um parser de HTML que facilita a manipulação do DOM de uma página web possibilitando a captura 
de dados da página de forma fácil e simples.  

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __DOM... Crawler… What the fu** is this?__
De maneira simples vamos entender primeiramente estes conceitos.  
O código HTML de uma página é formado por tags que formam uma estrutura de árvore com uma dentro de outras e assim 
por diante, o DOM (Document Object Model) fornece uma representação estruturada no formato de uma árvore do documento 
HTML para o browser contendo os nós, que seriam as tags html e cada nó possuindo suas propriedades.  

<div class="img-container">
    <img src="https://upload.wikimedia.org/wikipedia/commons/8/8b/Simpe_HTML_page_DOM.svg">  
</div>  

<div class="img-legend">Wikipedia</div>  

<br>

Um Crawler, Web Crawler ou também conhecido como Spider é um mecanismo automatizado que possui a capacidade de navegar 
entre sites e fazer a “leitura” destas páginas para captura de informações ou outras tarefas necessárias, é a base de 
muitas ferramentas de buscas hoje em dia como, por exemplo o Google, onde uma das tarefas é indexar os links dos sites 
em seu banco de dados. Existem diversos objetivos para criar um web crawler como para analisar os links e verificar se 
estão ativos ou não, para análise de dados, ou também para um trabalho da faculdade onde eu fiz a captura de informações 
para treinar um algoritmo de classificação de textos.  :P  

<div id="ref-post">
    <a href="{% post_url 2018-06-23-Clojure-Classificacao-de-comentarios-com-machine-learning %}">
        <img src="https://cdn-images-1.medium.com/max/1371/1*6h-IqXpvLbQemIRedaCrMg.png">
        <div id="link-title">
            Clojure Classificacao de comentarios com machine learning
        </div>
        <div id="link-description">
            Classificação de textos consiste em associar cada texto a uma determinada classe 
            baseada em seu conteúdo, por exemplo, podemos associar um comentário 
            sobre um produto...
        </div>
    </a>
</div>
<br>
também é possível capturar informações de arquivos de textos, PDFs, etc.

 

<br>

#### __DomCrawler__  
O DomCrawler é um componente do Framework Symfony que facilita a navegação no DOM de documentos HTML e XML, ele fornece 
alguns métodos para manipulação destes documentos e captura de informações, iremos utilizá-lo para criar nosso web 
crawler. Por trás dos panos este componente utiliza a classe DOMElement do PHP para criar os nós do DOM para podermos 
navegar entre eles.  

#### __Guzzle__  
Outra ferramenta que utilizaremos será o Guzzle, responsável por criar e manipular as requests e responses para links 
que queremos capturar as informações.

<br>

#### __Let's Code__  

Primeiramente dentro do diretório onde irá ficar nosso web crawler vamos iniciar o composer.

```sh
composer init
```

Irei apenas dar enter em todas as opções que o composer solicitar deixando todas com o valor default, após isto iremos 
instalar as ferramentas necessárias, o Guzzle, DomCrawler e Css Selector. O Css Selector utilizaremos junto com o 
DomCrawler.  

```sh
composer require guzzlehttp/guzzle
composer require symfony/dom-crawler
composer require symfony/css-selector
```  

Após a terminada a instalação vamos criar o arquivo do web crawler chamado ``webcrawler.php`` na raiz do projeto, não irei 
utilizar nenhuma estrutura mais complexa aqui, criarei tudo no mesmo arquivo visto que os métodos e conceitos podem ser 
aplicado posteriormente em uma aplicação mais complexa.  
Primeiramente vamos fazer o require do autload do diretório vendor para termos acesso às classes das ferramentas que 
utilizaremos.  


```php
<?php

require_once "./vendor/autoload.php";
```

As informações que iremos capturar são as críticas e notas dadas por usuários em um determinado filme em um site sobre 
cinema, primeiramente precisaremos fazer uma requisição para o link para pegar a página em formato texto(html) com o 
Guzzle.  

```php
$client = new \GuzzleHttp\Client();

$response = $client->get("http://www.adorocinema.com/filmes/filme-141110/criticas/espectadores/");

$html = $response->getBody()->getContents();

var_dump($html);
```

Primeiro criamos um Http Client para podermos fazer requests para uma página web, após isso utilizamos este objeto para 
realizar uma requisição do tipo ``GET`` para o link que queremos, o método ``get`` do  Client do Guzzle retorna uma 
``ResponseInterface`` então podemos utilizar os métodos ``getBody`` e ``getContents`` para termos o html da página.  
Podemos testar entrando no diretório do projeto e executando o seguinte comando:  
``php webcrawler.php`` ao final será apresentado o html da página.  


Este html passamos na criação do Crawler, esta classe fornece métodos para manipulação do DOM html.
```php
$crawler = new \Symfony\Component\DomCrawler\Crawler($html);
```

<br>

A partir desse ponto podemos utilizar os métodos do Symfony Crawler para percorrer os nós da página e capturar as 
informações necessárias.  
Como comentei anteriormente, iremos capturar as críticas e a nota dada por usuários em um determinado filme, primeiro 
temos que analisar a página, então vamos acessar o link utilizado no método get  do client http, vamos abrir a 
ferramenta de desenvolvedor do browser, navegando pelas tags podemos perceber que todas as informações que queremos 
estão dentro de várias divs com class ``review-card-review-holder``  

<div class="img-container">
    <img src="https://i.ibb.co/D5pmq73/exemple-html.png">  
</div>  


Então usaremos o método ``filter`` para filtrar apenas essas divs que nós queremos, o ``CSS Selector`` que instalamos 
permite utilizarmos uma sintaxe similar ao jQuery para acessar determinado nó do DOM, podemos filtrar por qualquer 
atributo como classe, id, name ou qualquer outro.  

```php
$contentContainer = $crawler->filter('div[class="review-card-review-holder"]');
```

O método filter retorna um novo objeto ``Crawler`` contendo apenas os elementos correspondentes aquela query que passamos no 
``filter``, o site faz paginação mostrando apenas 10 comentários por páginas, para verificarmos se pegamos as dez divs 
podemos usar o método ``count``.

```php
var_dump($contentContainer->count()) ; 10
```

<br>

Analisando novamente a estrutura da página vemos que a nota dada pelos usuários estão em tags ``span`` com 
class ``stareval-note`` dentro das divs que já filtramos e o texto da crítica está em divs com class ``content-txt review-card-content``. 


<div class="img-container">
    <img src="https://i.ibb.co/9T100XG/exemple-html.png">  
</div>  

Primeiro vamos capturar essas informações de uma maneira não muito legal para entendermos o que esta acontecendo, 
primeiramente vamos capturar as informações das notas. 

```php
$rates = $contentContainer->filter('span[class="stareval-note"]');
```

Assim podemos percorrer como um array e pegar as informações, aqui irei apenas imprimir na tela, mas poderia ser 
executado alguma funcionalidade para salvar no banco, processar essa informação ou qualquer outra ação.  

```php
foreach ($rates as $rate) {
    var_dump($rate->textContent);
}
```  

Da mesma forma ocorre para o texto da crítica.

```php
$texts = $contentContainer->filter('div[class="content-txt review-card-content"]');
```
```php
foreach ($texts as $text) {
    var_dump($text->textContent);
}
```
Dentro do foreach estamos manipulando objetos da classe ``DOMElement`` então utilizamos a propriedade `` textContent`` 
para pegarmos a informação necessária.  
Como podemos ver, as informações estão separadas e não ficou muito bom esse código, precisando executar um merge ou 
alguma outra forma para juntar o texto com sua respectiva nota, então vamos refatorar utilizando alguns métodos do 
Crawler do Symfony.  

<br>

Como vimos o método ``filter`` retorna um novo Crawler e esta classe contém o método ``each`` similar a um ``map``, 
simplificando o conceito do método each é que ele permite executar uma determinada função em cada item de um conjunto 
retornando um novo conjunto com os elementos após a aplicação da função.  


```php
$data = $crawler->filter('div[class="review-card-review-holder"]')->each(function ($contentContainer) {
    /** @var \Symfony\Component\DomCrawler\Crawler $contentContainer */
    $rate = $contentContainer->filter('span[class="stareval-note"]')->text();
    $text = $contentContainer->filter('div[class="content-txt review-card-content"]')->text();

    return [
        "rate" => $rate,
        "text" => $text
    ];
});
```

Após executar o ``filter`` para recuperarmos os nós que contém as informações que queremos, executamos o método ``each`` 
passando uma função que será executada em cada um desses nós, dentro dessa função executamos um ``filter`` novamente para 
pegarmos o nó onde temos a nota e o texto e após utilizamos o método ``text`` para pegarmos efetivamente o conteúdo que 
precisamos, assim retornamos um array contendo o texto e sua respectiva nota, no final temos um array ``$data`` com a 
seguinte estrutura.  

```php
[
    [
        "rate" => valor-nota
        "text" => texto
    ],
    [
        "rate" => valor-nota
        "text" => texto
    ],
    [
        "rate" => valor-nota
        "text" => texto
    ],
    ......
]
```

Assim temos uma estrutura fácil de manipular contendo as informações necessárias.  

<br>

O código final ficou desta maneira.  

```php
<?php

require_once "./vendor/autoload.php";

$client = new \GuzzleHttp\Client();

$response = $client->get("http://www.adorocinema.com/filmes/filme-141110/criticas/espectadores/");

$html = $response->getBody()->getContents();


$crawler = new \Symfony\Component\DomCrawler\Crawler($html);


$data = $crawler->filter('div[class="review-card-review-holder"]')->each(function ($contentContainer) {
    /** @var \Symfony\Component\DomCrawler\Crawler $contentContainer */
    $rate = $contentContainer->filter('span[class="stareval-note"]')->text();
    $text = $contentContainer->filter('div[class="content-txt review-card-content"]')->text();

    return [
        "rate" => $rate,
        "text" => $text
    ];
});


var_dump($data);
```


<br>

O componente DomCrawler do Symfony também contém classes especiais para captura e manipulação de links, imagens e 
formulários, além de permitir algumas querys mais complexas para percorrer o DOM html com
[Expression Evaluation](https://symfony.com/doc/current/components/dom_crawler.html#expression-evaluation), também permite capturar 
informações sobre os atributos das tags entre outras funcionalidades. É uma ferramenta que vale a pena dar uma olhada.  


<br>

##### __Referências:__
- https://symfony.com/doc/current/components/dom_crawler.html
- https://developer.mozilla.org/pt-BR/docs/DOM/Referencia_do_DOM
- https://linux.ime.usp.br/~cef/mac499-06/monografias/andre/WebCrawler.html
