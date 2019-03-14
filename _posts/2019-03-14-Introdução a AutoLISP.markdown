---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg"
title: "Introdução a AutoLISP"
subtitle: 
date: 2019-03-14 12:00:25 -0300
categories: deselvolvimento
excerpt: O AutoLISP é um dialeto da linguagem LISP, uma das mais antigas e utilizadas até hoje, o AutoLISP foi incorporado no 
         AutoCAD em 1996 e tem sido amplamente utilizado para customização do ambiente do AutoCAD, permitindo criar "comando" 
         e ferramentas específicas...
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>


O AutoCAD é um software de CAD, Desenho Auxiliado por Cmputador, permite a criação de desenhos técnicos em 2D e modelos 
tridimensionais, utilizado em diversas áreas como engenharias, arquitetura topologia, design etc, é um dos software mais 
utilizados para este fim. Uma característica do AutoCAD é que ele permite o uso de programação em AutoLISP para personalização 
e estensão de suas funcionalidades.  
<br>
O AutoLISP é um dialeto da linguagem LISP, uma das mais antigas utilizadas até hoje, o AutoLISP foi incorporado no 
AutoCAD em 1996 e tem sido amplamente utilizado para customização do ambiente do AutoCAD, permitindo criar "comando" 
e ferramentas específicas para problemas de áreas diversas.

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __AutoLISP__
O AutoLISP, assim como o LISP, é extremamente simples e mantém uma enorme flexibilidade, permitindo a fácil extensão e 
criação de novos meios para resolução de problemas, ele evoluiu e manteve a maioria dos conceitos básicos, tipos de dados 
e capacidade do LISP, além de acrescentar funcionalidades especiais para operações e recursos do ambiente do AutoCAD.  


Quando comparada com outras linguagens mais usuais o AutoLISP pussui uma sintaxe extremamente simples, baseada em 
*S-Expressions*, de maneira simploria é uma forma de representar dados em listas encadeadas.  
Podemos escrever uma expressão em AutoLISP utilizando elementos primitivos como caracteres, números, ou compondo outras 
espressões entre si.  

<br>

Sendo baseada em processamento de listas o AutoLISP se encaiza muito bem em um sistema CAD já que a maioria deste tipo 
de sistema gráfico se baseiam em pontos e vetores em um plano cartesiano. Tomando como exemplo um sistema CAD com 
coordenadas em um plano cartesiano, um ponto nesse plano pode ser considerado uma lista de números, cada número 
representando uma posição nos eixos __X__ __Y__ e __Z__.  


``ponto1 = (2 5 3)`` - coordenada X = 2, coordenada Y = 5, coordenada Z = 3  

Agora imaginem uma linha desenhada nesse plano cartesiano definida por dois pontos, podemos perceber essa linha como 
uma lista contendo duas listas internas descrevendo o ponto inicial e o ponto final da linha.  

``linha = ((0 0) (3 4)``  

<div class="img-container">
    <img src="https://i.ibb.co/PMq3y1m/linha.png">  
</div>

<br>
<br>

#### __Sintaxe__  

A sitaxe do AutoLISP, como citado, é simples, uma expressão é escrita iniciando-se com um ``(`` logo após temos uma  
função, tudo o que vier em seguida são parâmetros para esta função e terminamos com um ``)``.  

``
(função parâmetro [...parâmetros])
``

Toda expressão é enviada para o interpretador do AutoLISP para ser avaliada e saber qual a natureza da expressão, se o  
primeiro elemento da lista for um nome de uma ``subr`` ou uma função, esta será executada e o restante da lista é usada  
como parâmetros, se o primeiro elemento não for uma função ou ``subr```é apresentado uma mensagem de erro.  

Uma das vantagens é que podemos executar uma expressão diretamente no prompt de comando do AutoCAD, isso nor permite  
testar trechos de códigos e vê-los funcionando ou falhando antes de escrevê-los em um código principal.  

<div class="img-container">
    <img src="https://i.ibb.co/DRSpxG4/command.png">  
</div>  

O AutoLISP manteve a notação prefixada, ou seja, o operador vem primeiro e em seguida os operandos, aplicando isso em 
uma operação matemática temos:  

<script src="https://gist.github.com/Jciel/fd8cfbbd402c04d547a84b39965995db.js"></script>

Transfoemando isso para a notação infixa a qual esmos acostumados seria algo como o seguinte

``(2 * 2)`` e ``((45 * PI) / 180)`` 

<br>

Existem alguns caracteres especiais em AutoLISP:  

``(`` e ``)`` - Define o inicio e fim de uma expressão;  
``"`` - Define um nome de comando do AutoCAD ou um texto qualquer;  
``;`` - Define uma linha de comentário;  
Letras maiúsculas e minúsculas não tem diferença em AutoLISP.  


Algumas características da linguagem:  

``Átomo`` é o elemento de dado básico do AUtoLISP, tipos de dados complexos são construídos usando conjunto de Átomos;  
``Symbols`` refere-se a qualquer 'nome' em um código AutoLSIP, seja já existente no interpretador ou definido pelo usuário;  
``Variável`` é um símbolo utilizado para guardar alguma informação relevante em um código AutoLISP;  


<br>


#### __Tipos de dados__  
Variáveis podem guardar diversos tipos de dados, seguem alguns tipos básicos.  

__``inteiro``__ são números sem casas decimais;  
__``real``__ são números que possuem uma parte decimal;  
__``Nothing (nil)``__ o mesmo que nulo;  
__``sub-rotina (subr)``__ são funções já definidas no interpretador do AutoLISP e disponíveis para serem utilizadas;  
__``ponteiro de arquivo (file)``__ é um ponteiro para um arquivo, utilizado para manipulação de arquivos de texto (txt);  
__``Entity name (ename)``__ é um ponteiro para o banco de dados do desenho, utilizado para acesso as propriedades de uma 
entidade no desenho;  
__``lista``__ é um conjunto de dados, uma lista pode conter tipos diferente de dados inclusive outras listas, existem dois 
tipos de listas, lista simples e lista de associação;  
__``funções``__ são trechos de códigos definido pelo usuário;  


<br>


#### __Ambiente do VisualLISP__  

O AutoCAD possui um editor chamado VisualLISP, nele é possǘiel escrever código AutoLISP com destaque de sintaxe entre 
outras coisas, podemos abri-lo usando o comando __``vlisp``__.  

<div class="img-container">
    <img src="https://i.ibb.co/HrQ1PyF/vlisp-Initial.png">  
</div>  


Na parte superior temos o editor e na inferior temos um console onde podemos executar trechos de código e ver os 
resultados desses testes.  
Temos também algumas ferramentas na parte superior, nas barras de ferramentas, por exemplo o ``Load active edit window``

<div class="img-container">
    <img src="https://i.ibb.co/Q6cwL0W/load-Active-Edit-Window.png">  
</div>  

que faz o "load" para o AutoCAD de todo o código escrito na janela atual deixando disponível para executar no AutoCAD 
aberto no momento, e também temos o ``Load selection``

<div class="img-container">
    <img src="https://i.ibb.co/VJy91VB/load-Selection.png">  
</div>  

que faz "load" apenas do trecho selecionado do código atual.  

<br>



#### __Variáveis__  

Variávei são simbolos onde podemos "guardar" alguma informação a ser utilizada, uma variável é definida com a subr ``setq``.  

<script src="https://gist.github.com/Jciel/7d623aec0af67d49e5d70d0f38724495.js"></script>

<br>

#### __Funções__  

Uma função é um trecho de código bem definido que resolve um problema específico, podemos dividir um problema grande em 
funções menores que resolvem uma parte desse problema para no final conseguir a solução completa. Toda função ou subr 
retorna um valor, esse retorno é o resultado da avaliação da última expressão da função.  
Para criarmos uma função em AutoLISP usamos a subr ``defun``, a criação de uma função segue o seguinte exemplo:  

<script src="https://gist.github.com/Jciel/64c0ea1673d5f89ed0fd778c6ae9071a.js"></script>

<br>

__Exemplos:__  

<script src="https://gist.github.com/Jciel/8274f7f015b20aac175a1a6dd3615bf5.js"></script>

<br>

Variáveis em funções podem assumir três estados diferentes de determinado por sua existência e localização na lista de 
parâmetros da função:

__``Variável associada``__, __``Variável global``__ e __``Variável local``__  

A área onde definimos os parâmetros de um função pode ser dividida em duas partes  

``(defun teste(<parâmetros> / <variáveis locais>) ...``  

Variáveis locais são os parâmetros da função e ficam do lado esquerdo da ``/``, variáveis locais ficam do lado direito 
da ``/``, o resto são variáveis globais. Sempre defina variáveis locais para evitar sobrescrita de outros nomes de 
variáveis, funções ou que outra função manipule uma variável alterando completamente o resultado final.  
Uma variável local só existe dentro do copro da função, seu valor não é visto fora dela.  

<script src="https://gist.github.com/Jciel/01c9ade5b12a1006f8962e1101ef8d44.js"></script>

<br>

Para executar uma função criada no AutoLISP após fazer "load" do código, podemos chamar ela no prompt de comando coloando 
o nome da função entre parenteses.  

<div class="img-container">
    <img src="https://i.ibb.co/h2RGjrV/execu-o-Fun-o.png">  
</div>  

Para chamarmos essa função no formato de um comando do AutoCAD, apenas com o nome da função, devemos colocar o prefixo 
``c:`` na hora de definir o nome.  

<script src="https://gist.github.com/Jciel/292d07f92b94baf61c5e16fccd7c9dfb.js"></script>

<br>

Desta forma criamos um "comando" novo e podemos executar apenas digitando o nome da função.  

<div class="img-container">
    <img src="https://i.ibb.co/C9PJSvn/execu-o-Fun-o2.png">  
</div>  


<br>

Alguns exemplos de funções.  

Função que retorna o comprimento de um círculo de diâmetro 25.  
<script src="https://gist.github.com/Jciel/1f536fb75ea5ec180ca2ef34225fcca9.js"></script>

<br>

Função que retorna a área de um triângulo b=5, h=7.  
<script src="https://gist.github.com/Jciel/4a3ae02dac3bd71fa82e1caeb10b8b78.js"></script>

<br>

Função que retorna a área de um círculo de diâmetro 20.  
<script src="https://gist.github.com/Jciel/f128d8d8608bdb250da4484d6bd28c23.js"></script>


<br>


#### __Conclusão__  

Fica aqui uma introdução a AutoLISP que pode ser uma poderosa ferramenta para quem trabalha com AutoCAD possibilitando a 
criação de rotinas para automatização de tarefas repetitivas bem como para criação de ferramentas que não existem prontas 
para problemas específicos da área, inclusive muitos dos comando do AutoCAD são rotinas AutoLISP, também é possível a 
manipulação de entidades em um desenho podendo assim capturar informações sobre as mesmas e/ou alterar essas informações, 
esses e outros assuntos tentarei abordar em outros textos.  
Comentem se gostaram, compartilhem se conhecem alguém a quem possa ser útil este texto e até o próximo.


<br>

##### __Referências:__
- GAÁL, José Alberto. Curso de AutoLISP. Campinas, SP: Desecad, 1997. 251 p.
- KRAMER, William. Programando em AUtoLISP. São Paulo, SP: Makron Books, s/d, 274 p.
- AUTODESK. AutoCAD Developer Documentation. 2019. Disponível em: <https://help.autodesk.com/view/OARX/2019/PTB>
