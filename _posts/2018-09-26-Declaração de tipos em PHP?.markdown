---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/1371/1*6h-IqXpvLbQemIRedaCrMg.png"
title: "PHP - Declaração de tipos em PHP"
subtitle: Typed Properties
date: 2018-09-26 12:00:25 -0300
categories: deselvolvimento
excerpt: Desde o PHP 5 existe uma característica chamada Indução de tipos (*Type 
         Hinting*), sendo possível definir tipos de parâmetros de funções e 
         métodos, mas era somente possível definir os tipos...
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>

Desde o PHP 5 existe uma característica chamada Indução de tipos (*Type 
Hinting*), sendo possível definir tipos de parâmetros de funções e 
métodos, mas era somente possível definir os tipos array e objetos com 
nome de classe. 

<br>

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __O que temos__  

<br>

##### __PHP 7__  
O PHP 7 trouxe muitas mudanças, além do desenpenho também possibilitou a 
definição de outros tipos escalares nos parâmetros de funções, *Scalar 
typehinting*, permitindo declarar os tipos *int*, *float*, *string* e 
*bool*. Além disso, podemos definir o tipo de retorno das 
funções e métodos \[direi apenas funções, mas as regras valem para 
métodos também\].  

```php
<?php

public function funcao(int $idade): int
{
    ...
    return idade + 1;
}

```  

<br>

Com o PHP 7.1 temos *Nullable Types*, representado pelo sinal de 
interrogação '?' antes do tipo declarado, assim podemos também definir 
que um parâmetro pode receber o tipo definido ou *null*, isso vale 
igualmente para o retorno da função.  

```php
<?php

public function funcao(?string $nome): ?string
{
    ...
    if (...) {
        return null;
    }
    return "Nome é " . $nome;
}

```  

<br>

A partir da versão 7.1, o PHP também aceita o tipo void e a versão 7.2 
trouxe mais algumas mudanças nesse sentido, disponibilizando o *Object 
typehinting*, assim podemos definir o tipo *object* para parâmetro e 
retorno de funções, deixando-os mais genéricos.  

```php
<?php

public function funcao(object User): object
{
    ...
    
    return new ObjectQualquer();
}

```  

<br>

#### __O que vem pela frente__  

<br>

Hoje, 26/09/18, temos a noticia que a RFC Typed Properties 2.0 foi 
aprovada, esta RFC introduz a possibilidade de declarar o tipo de 
propriedades de classes, prevista para entrar na versão 7.4 do PHP.  

Com isso poderemos definir o tipo das propriedade das classes.  

```php
<?php

class Pessoa
{
    private int idade;
    private string nome;
    
    public function __construct(int idade, string nome)
    {
        ...
    }
}

```  

<br>

O suporte à declaração de tipos é independente da visibilidade da 
propriedade, também de propriedades *static*. Segundo a RFC, a declaração de 
tipos para propriedades permitirá todos os tipos já suportados 
menos o *void* e *callable*.  

Tipos suportados:  
- bool
- int
- float
- string
- array
- object
- iterable
- self
- parent
- qualquer nome de classe ou interface  

e também poderemos definir como *Nullable Types*  

```php
<?php

class User
{
    public ?string nome = null;
}

```  

<br>

##### __E como fica a conversão de tipos?__  
De acordo com a RFC, como acontece com os parâmetros e retorno de 
funções, os tipos das propriedades também serão afetados pela directiva 
*strict_types*. Caso a diretiva esteja setada como 1, o valor a ser 
atribuido para a propriedade deve satisfazer exatamente o tipo 
declarado, caso contrário seguirá a regra padrão de coerção de tipos do 
PHP.  

```php
<?php

declare(strict_types=1);

class User
{
   public int idade;
}

$user new User();

$user->idade = "42"; // Dará uma exception TypeError

```  

<br>

##### __Em caso de herança__  
Caso uma propriedade da classe pai for privada será possível alterar o 
tipo dela, outros casos não serão permitidos.  

```php

class A {
    private bool $a;
    public int $b;
    public ?int $c;
}

class B extends A {
    public string $a; // legal, because A::$a is private
    public ?int $b;   // ILLEGAL
    public int $c;    // ILLEGAL
}

```
<span class="reference-down">Exemplo retirado do texto da RFC PHP RFC: Typed Properties 2.0</span>

<br>

A RFC cita duas opções para a sintaxe de declaração de tipos em 
propriedades de classes, sendo o primeiro similar ao que já vem sendo 
usado na declaração de parâmetros e o segundo ao que é usado na 
declaração do retorndo de funções.  

```php

public int $teste;
public $teste: int;

```  

Se a opção com ``:``, ``public $teste: int;`` for usada, em casos de 
definição de valores padrão, também sâo citadas duas alternativas:

```php

public $teste: int = 42;
public $teste = 42: int;

```  

Os autores da RFC expressam sua preferência pela primeira opção, citando algumas 
situações nas quais a segunda alternativa ficaria um pouco confusa, como em 
casos que o valor padrão for uma expressão mais complexa como uma 
matriz grande, a definição do tipo ficaria muito longe do
nome da propriedade prejudicando a legibilidade do código.  

```php
<?php

class User
{
    public $teste = [...
                     ...
                     ...
                     ...
                    ]: int;
}

```
<span style="margin-top: 10px;">Exemplo usando a segundo alternativa.</span>


<br>

#### __Conclusão__  

Na página da RFC Typed Properties 2.0 você pode encontrar a definição 
completa da proposta, inclusive o caso do tipo *callable* e algumas 
questões de tipos e referência que possuem algumas situações 
particulares que não foram citadas.  

Ao meu ver essa definição de tipos permitirá favorecer a consistência do 
código e facilitar o tratamento de erros além de melhorar a legibilidade 
entre outro benefícios.  

Agora apenas esperar por mais esse update no PHP.  

##### __Referências:__
- https://wiki.php.net/rfc/typed_properties_v2
- https://wiki.php.net/rfc/void_return_type
- https://wiki.php.net/rfc/nullable_types
- https://wiki.php.net/rfc/scalar_type_hints_v5
- https://wiki.php.net/rfc/object-typehint
