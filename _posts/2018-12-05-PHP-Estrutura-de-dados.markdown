---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg"
title: "PHP - Standard PHP Library (SPL)"
subtitle: Estrutura de dados e Tipos Abstratos de Dados
date: 2018-12-05 12:00:25 -0300
categories: deselvolvimento
excerpt: Quem já estudou ou pesquisou por algoritmos provavelmente já ouviu falar 
         em Estrutura de Dados (ED), são modos genéricos de armazenar informações 
         de forma eficiente ajudando a melhorar o desem
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>

Quem já estudou ou pesquisou por algoritmos provavelmente já ouviu falar 
em Estrutura de Dados (ED), são modos genéricos de armazenar informações 
de forma eficiente ajudando a melhorar o desempenho e entendimento de um 
algoritmo. Tipos Abstratos de Dados (TAD), em um contexto de linguagem, 
denota um conjunto de dados e as operações possíveis de serem realizadas 
sob esses dados, estrutura de dados é uma forma concreta de um Tipo 
Abstrato de dados.

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __Tipos Abstratos de Dados__
Temos alguns TADs já conhecidos e bastante utilizados como Fila, Pilha, 
Lista, Árvore, etc, esses Tipos Abstratos de Dados podem ser 
divididos em dois grupos, lineares e não lineares.  

Os TDA's lineares são estruturas onde os dados são arranjados 
sequencialmente, estes possuem operações de adicionar ou remover 
um item que podem seguir modos diferentes, vamos tomar como exemplo 
uma ``Fila`` e uma ``Pilha``, vamos imaginar uma fila em um caixa de um 
restaurante, quando uma pessoa entra na fila ela se posiciona atrás de 
todas as outras pessoas, no final da fila, se uma outra pessoa chegar, 
ela entra atrás desta, e assim por diante, seguindo esta lógica, as 
pessoas que estavam nas primeiras posições vão sendo atendidas e assim 
a fila vai "andando" e os "itens" vão sendo processados, esta forma de 
inserir e remover um item é chamado de __FIFO__ (First In, First Out), 
Primeiro a entrar, primeiro a sair.

<div class="img-container">
    <img src="https://i.ibb.co/m9PtY1R/fila.png">  
</div>

<br>

Agora imaginamos no mesmo restaurante alguém na cozinha lavando os pratos 
utilizados, a pessoa lava e coloca um sobre o outro ao lado da pia, 
assim vai formando-se uma pilha de pratos lavados, um em cima do outro, 
quando um prato é limpo é colocado no topo da pilha, caso precise ser 
utilizado um novo prato pra servir, como seria problemático retirar o 
último prato lá de baixo, podendo virar todos os outros, é retirado um 
prato também do topo da pilha, esta forma de inserir e remover itens 
é chamado de __LIFO__ (Last In, First Out), último a entrar, primeiro a 
sair, o último item colocado na pilha, no topo, quando for preciso 
utilizar, será o primeiro a ser retirado.  

<div class="img-container">
    <img src="https://i.ibb.co/dkFpGzC/pilha.png">  
</div>

<br>

Em relação aos TDA's do tipo árvore existem diversos tipos, o que 
iremos ver é uma Heap, que é uma árvore binária balanceada.  
Uma árvore binária é uma estrutura formada por ``nós`` onde são 
"guardados" os valores e cada ``nó`` pode ser "ligado" à no máximo dois 
``nós`` filhos, nesta estrutura os dados são dispostos de forma 
hierárquica, o primeiro nó da árvore é chamado ``raiz`` da árvore, os 
últimos ``nós`` são chamados ``nós folhas``.  
Existem outros conceitos relacionados a árvores como grau, altura, 
percorrer uma árvore, entre outros, não iremos entrar em detalhes aqui.  

<div class="img-container">
    <img src="https://i.ibb.co/cDQFxRk/arvore.png">  
</div>

<br>

#### __SPL__ 
A maioria das linguagens de programação fornecem bibliotecas que 
implementam essas abstrações, restando apenas usarmos. Desde a versão 5 
o PHP tem *built in* a biblioteca Standard PHP Library (__SPL__). A SPL 
é um conjunto de *interfaces* e classes que fornecem algumas soluções de 
estrutura de dados pronta paras serem utilizadas, vamos ver o 
funcionamento de algumas delas.  

<br>

##### __Fila (SplQueue)__
Esta estrutura, como o nome sugere, tem como comportamento o mesmo que 
uma fila do mundo real, cada item inserindo é alocado atrás do último 
já existente, e quando executado uma operação de remover, é retirado o 
primeiro item no momento da fila.  
Usando a ``SplQueue`` da SPL.  

```php
<?php
    $queue = SplQueue();
    $queue->enqueue("pessoa 1");
    $queue->enqueue("pessoa 2");
    $queue->enqueue("pessoa 3");
    $queue->enqueue("pessoa 4");

```  

No exemplo usamos a operação ``enqueue`` que “enfileira” itens. Para 
remover um item usamos a operação ``dequeue``.  

```php
<?php
    echo $queue->dequeue(); // Remove “pessoa 1”
    echo $queue->dequeue(); // Remove “pessoa 2”
```  
Cada vez que executamos essa operação ele remove o item que está atualmente 
na primeira posição da fila.  
A Fila também possui uma operação para verificar se está vazia ou não.  

```php
<?php
    $queue = SplQueue();
    echo $queue→isEmpty(); // true
```
<br>

##### __Pilha (SplStack)__
Podemos imagina uma Pilha como um conjunto de itens um em cima do outro 
formando uma "pilha" de itens. Usamos a ``SplStack`` para trabalhar 
com essa estrutura, uma Pilha possui duas operações básicas, ``push`` para 
empilhar um item e ``pop`` para desempilhar um item.  

```php
<?php
    $stack = SplStack();
    $stack->push("prato 1");
    $stack->push("prato 2");
    $stack->push("prato 3");
    $stack->push("prato 4");
```  

Agora podemos observar o comportamento da Pilha, podemos imaginar 
visualmente a estrutura do exemplo da seguinte forma:  

```php
"prato 4"
"prato 3"
"prato 2"
"prato 1"
```  
Como vimos anteriormente, quando removemos um item de uma Pilha, 
é removido o item que está atualmente no topo da estrutura.  

```php
<?php
    echo $stack->pop(); // Remove "prato 4"
    echo $stack->pop(); // Remove "prato 3"
```

Agora caso inserirmos mais itens na Pilha, observando o comportamento 
descrito anteriormente.  
```php
<?php
    $stack->push("prato 6");
    $stack->push("prato 7");
```
Seguindo o comportamento normal de uma Pilha de inserir um item sempre 
no topo, teremos os seguintes dados: 
 
```php
"prato 7"
"prato 6"
"prato 2"
"prato 1"
```  

A Pilha também possui a operação para verificar se existe está vazia.  
```php
<?php
    $stack = SplStack();
    echo $stack->isEmpty(); // true
```

<br>

##### __Deque__  
A SPL possui a SplDoublyLinkedList, esta estrutura implementa um tipo 
de Lista, a diferença é que a SplDoublyLinkedList  permite a inserção ou 
remoção de dados em ambos os lados, sendo então um ``Deque``, portando 
podemos inserir ou remover um item tanto no que seria o início da lista 
quanto no que seria o final da lista.  


A SplDoublyLinkedList possui quatro operações básicas, inserir no 
início da lista, inserir no final da lista, remover do início da lista 
e remover do final da lista.  

A operação ``unshift`` enfileira um item no início da lista.

```php
<?php
    $doublyLinkedList = new SplDoublyLinkedList();
    doublyLinkedList->unshift("Arroz");
    doublyLinkedList->unshift("Pimentão");
    doublyLinkedList->unshift("Cenoura");
    doublyLinkedList->unshift("Tomate");
```  

Mesmo que coloquemos os itens em uma ordem inversa, quando executado a 
operação com um item, ele é colocado sempre no início da lista. No 
momento esse exemplo tem a seguinte estrutura:  
```php
Tomate
Cenoura
Pimentão
Arroz
```  

A operação ``push`` enfileira um item no final da lista, vamos pegar a 
lista anterior e adicionar um item ao final dela.  

```php
<?php
    doublyLinkedList->push("Feijão");
```

Agora nossa lista está completa:  
```php
Tomate
Cenoura
Pimentão
Arroz
Feijão // Item inserido no final da lista com push
```  

Para desenfileirar temos duas operações, ``shift`` para remover do 
início da lista e ``pop`` para remover do final da lista.  

```php
<?php
    doublyLinkedList→shift(); // Remove Tomate
    doublyLinkedList→shift(); // Remove Cenoura
    
    doublyLinkedList→pop(); // Remove Feijão
    doublyLinkedList→pop(); // Remove Arroz
```

<br>

##### __Heap (Árvore Binária)__

A classe SplHeap da SPL é uma classe abstrata que implementa uma série 
de funcionalidades de uma Heap, sendo uma classe abstrata para usar uma 
estrutura Heap da SPL é preciso usar as classes concretas ``SplMaxHeap`` 
e ``SplMinHeap``. Para inserir um item usamos a operação ``insert``.  

##### __SplMaxHeap__
A SplMaxHeap para definir a posição do valor a ser inserido na árvore 
ela mantém sempre o maior valor como raiz da árvore, assim apresentando 
os valores com uma ordenação decrescente.  

```php
<?php
    $tree = new SplMaxHeap();
    $tree->insert(1);
    $tree->insert(15);
    $tree->insert(27);
    $tree->insert(9);
    $tree->insert(6);
    $tree->insert(21);
    $tree->insert(23);
    $tree->insert(3);
```

Vamos ver os dados.  
```php
<?php
    foreach ($tree as $item) {
        echo $item . PHP_EOL;
    }
```
A saída apresentada será ``27, 23, 21, 15, 9, 6, 3, 1``.


##### __SplMinHeap__
A ``SplMinHeap`` ao contrário da ``SplMaxHeap`` mantém o menor valor 
como raiz da Árvore, apresentando os valores com ordenação crescente.  
```php
<?php
    $tree = new SplMinHeap();
    $tree->insert(1);
    $tree->insert(15);
    $tree->insert(27);
    $tree->insert(9);
    $tree->insert(6);
    $tree->insert(21);
    $tree->insert(23);
    $tree->insert(3);
```  
```php
<?php
    foreach ($tree as $item) {
        echo $item . PHP_EOL;
    }
```
A saída apresentada será ``1, 3, 6, 9, 15, 21, 23, 27``.  

<br>

Sendo a ``SplHeap`` uma classe abstrata podemos criar nossa própria 
classe concreta extendendo a ``SplHeap`` e implementando seu método 
``compare`` que é utilizado para definir a posição do valor a ser 
inserido na árvore.  

```php
<?php
class Person
{
	private $age;

	public function __constructor(int $age)
	{
		$this->age = $age;
	}

	public function getAge(): int
	{
		return $this->age;
	}
}

class PersonHeap extends SplHeap
{
	public function compare($personX, $personY)
	{
		return $personX->getAge() <=> $personY→getAge();
	}
}
```

Criamos uma classe ``Person`` para representar algumas entidades 
``pessoas`` que iremos inserir na estrutura, também criamos nossa classe 
``PersonHeap`` extendendo a ``SplHeap``, notem o método ``compare``, 
ele deve retornar ``-1`` caso o primeiro valor seja menor que o 
segundo, ``1`` caso contrário e ``0`` se ambos forem iguais.  
O segundo parâmetro do método ``compare`` é o valor a ser inserido no 
momento, no exemplo anterior a estrutura apresentará os valores em 
ondem decrescente, para mostrar em ordem crescente basta alterar a 
ordem de comparação no método ``compare``, ficando da seguinte maneira:  

```php
<?php
	public function compare($personX, $personY)
	{
		return $personY->getAge() <=> $personX->getAge();
	}
```
Assim quando formos percorrer os valores, será em ordem crescente.  

<br>

```php
<?php
    $person1 = new Person(15);
    $person2 = new Person(5);
    $person3 = new Person(26);
    $person4 = new Person(39);
    $person5 = new Person(3);
    
    $personHeap = new PersonHeap();
    
    $personHeap→insert($person1);
    $personHeap→insert($person2);
    $personHeap→insert($person3);
    $personHeap→insert($person4);
    $personHeap→insert($person5);
    
    
    foreach ($personHeap as $person) {
        echo $person->getAge();
    }
```
Seguindo o primeiro exemplo, de forma descendente, a saída será ``39, 26, 15, 5, 3``.  

Também é possível verificar se a estrutura está vazia.  

```php
<?php
    $personHeap = new PersonHeap();
    $personHeap->isEmpty(); //true
```

<br>

##### __SplObjectStorage__
A SplObjectStorage fornece um modelo de dicionário, onde recebe um objeto 
e cria um hash do mesmo para fazer referência a este objeto internamente, 
permite apenas valores únicos e trabalha bem com grandes quantidades 
de objetos.  

```php
<?php
    class Person
    {
        private $name;
    
        public function __construct(string $name)
        {
            $this->name = $name;
        }
        
        public function getName(): string
        {
            return $this->name;
        }
    }

    $person1 = new Person("joao");
    $person2 = new Person("carlos");
    $person3 = new Person("jose");
    $person4 = new Person("maria");
    $person5 = new Person("julieta");
```

Criamos uma classe ``Person`` e algumas instâncias dessa classe, para 
inserir um objeto na estrutura usamos a operação ``attach``.  

```php
<?php
    
    $splObjectStorage = new SplObjectStorage();
    
    $splObjectStorage->attach($person1);
    $splObjectStorage->attach($person2);
    $splObjectStorage->attach($person3);
    $splObjectStorage->attach($person4);
    $splObjectStorage->attach($person5);
```

Temos algumas operações interessantes, para verificar a quantidade de 
itens e se a estrutura contém um determinado objeto.  


```php
<?php
    
    var_dump($splObjectStorage->count()); // Retorna a quantidade de itens, no exemplo 5
    
    var_dump($splObjectStorage->contains($person1)); // retorna true se contém um determinado objeto
```

Usamos ``detach`` para remover um determinado objeto.  

```php
<?php
    
    $splObjectStorage->detach($person1);
    
    var_dump($splObjectStorage->count()); // 4
```

<br>

##### __SplFixedArray__  
A SPL também contém uma classe chamada SplFixedArray que implementa 
algumas funcionalidades de arrays, algumas diferenças entre a 
SplFixedArray e o array normal do PHP é que o SplFixedArray é definido 
um tamanho fixo, permitido alterar posteriormente, e suporta apenas 
índice numérico inteiro.  

```php
<?php

    $fixedArray = new SplFixedArray(2);
    
    $fixedArray[0] = 10;
    $fixedArray[1] = 20;
    
    $fixedArray[2] = 30; // PHP Fatal error: .....
```

Podemos alterar o tamanho do array.  

```php
<?php
    $fixedArray->setSize(5);
    
    $fixedArray[2] = 30;
    $fixedArray[3] = 40;
    $fixedArray[4] = 50;
```

É possível também criar uma SplFixedArray a partir de um array comum.  

```php
<?php
    $arr = [10, 20, 30, 40];

    $fixedArray = SplFixedArray::fromArray($arr);
    
    var_dump($fixedArray);
```

<br>

#### __Conclusão__
Como é bem pouco comentado a ideia é mostrar que existe também a 
possibilidade de usarmos estrutura de dados em PHP e com a facilidade que 
temos já abstraído várias estruturas com a SPL facilitando o uso, não 
entrei em detalhes muito a fundo tanto das estruturas quanto de 
algoritmos, quem for pesquisar mais sobre vai ver que estrutura de dados 
e algoritmos estão interligados e tem bastante assunto teórico sobre. 
Aprender sobre estrutura de dados ajuda também a modelagem de software 
melhorando questões de algoritmos e mesmo entendimento de código de software 
entre outras melhorias que o aprendizado pode trazer.  
Outro link interessante sobre estrutura de dados em PHP que vale a pena 
dar uma olhada: https://medium.com/@rtheunissen/efficient-data-structures-for-php-7-9dda7af674cd

<br>


##### __Referências:__
- http://php.net/manual/pt_BR/spl.datastructures.php
- livro PHP 7 Data Structures and Algorithms: Implement linked lists, stacks, and queues using PHP
- https://www.youtube.com/watch?v=8tXgHtuj2Ko
