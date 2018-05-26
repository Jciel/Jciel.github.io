---
layout: post
imageOg: https://cdn-images-1.medium.com/max/1371/1*6h-IqXpvLbQemIRedaCrMg.png
title: "Clojure - Leiningen"
date: 2018-05-22 12:00:25 -0300
categories: deselvolvimento
excerpt: Em projetos de software, sejam grandes ou pequenos, pela facilidade e otimização do tempo, busca-se a reutilização de código, isso inclui também a utilização de bibliotecas de terceiros, gerenciar e manter...
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>

Em projetos de software, sejam grandes ou pequenos, pela facilidade e
otimização do tempo, busca-se a reutilização de código, isso inclui
também a utilização de bibliotecas de terceiros, gerenciar e manter
essas dependências acaba se tornando complicado e tomando muito tempo
com o crescimento do projeto. Assim, para facilitar existem os
gerenciadores de dependências, que agilizam processos de instalação,
remoção, atualização de dependências entre outras atividades.

![](https://cdn-images-1.medium.com/max/1371/1*6h-IqXpvLbQemIRedaCrMg.png)
<div class="img-legend">[Fonte](https://cdn-images-1.medium.com/max/1371/1*6h-IqXpvLbQemIRedaCrMg.png)</div>

<br>

Para quem vem do PHP, análogo ao Composer, em Clojure temos o Leiningen,
um gerenciador de projetos e dependências para Clojure. Vamos ver como
instalar e criar um projeto básico com ele.  
Eu estou utilizando a distribuição linux Manjaro, talvez possa mudar
alguns comandos ou caminhos no sistema que você estiver usando. 

<br>

#### __Instalação__
Primeiramente vamos fazer o download do script de instalação.
<script src="https://gist.github.com/Jciel/9e35549013fdd09e6db5051fa15e1d63.js"></script>

Vamos mover esse script para o diretório ``/bin``, e atribuir propriedade
de executável para ele.
<script src="https://gist.github.com/Jciel/a23a893abbf5aea5b80bad23d5613cd5.js"></script>

Em seguida podemos rodar o comando ``lein`` que ele irá fazer o download
dos pacotes necessários.
<script src="https://gist.github.com/Jciel/c85013ae1c1cbb84b480cd76023a19fc.js"></script>


Pronto, a instalação do Leiningen está feita.
Um recurso que o Clojure oferece para o desenvolvedor é o REPL, sigla
para Read-Eval-PrintLoop. REPL é um prompt onde podemos testar trechos
de código sem precisar criar um projeto inteiro para isso, basta rodar
o REPL. Podemos acessá-lo a partir do Leiningen, executando dentro do
diretório de um projeto, toda a estrutura do mesmo fica disponível,
funções, propriedades, etc.
<script src="https://gist.github.com/Jciel/fd71cf55d37342501c39beedb97e46b5.js"></script>


A partir disto estamos no prompt REPL e podemos testar código Clojure
direto no terminal, para sair basta digitar:
<script src="https://gist.github.com/Jciel/f03d0d8439fef31f499d2ace23fd5376.js"></script>

<br>

#### __Criando um projeto__
Agora vamos criar um projeto inicial, podemos rodar o comando a seguir
para criar um novo projeto chamado "hello".
<script src="https://gist.github.com/Jciel/2070d4c894bf6bc20f31ee2e286740ce.js"></script>


Agora temos um diretório chamado hello, onde está nosso projeto com sua
estrutura, não vamos nos aprofundar muito na estrutura nesse momento,
único destaque é o diretório ``src``, onde fica o código do projeto
criado.

Na raiz do projeto existe um arquivo chamado ``project.clj``, fazendo
novamente um paralelo com o ``composer.json``, esse é o arquivo que o
Leiningen utiliza para organizar as dependências, propriedades do
projeto entre outras coisas.  
Vamos testar, primeiro abra o arquivo ``project.clj`` e coloque o código a
seguir abaixo da linha ``:dependencies``, ainda dentro dos parênteses.
<script src="https://gist.github.com/Jciel/46ec7f2a6ab873c0b4f878a857afee4e.js"></script>

Ficará semelhante a isso
<script src="https://gist.github.com/Jciel/9b8aede194d47473193b12c8de9a04bd.js"></script>


Aqui definimos qual a função principal, que sera executada quando
rodarmos o projeto.

Agora entramos no diretório do projeto e executamos o seguinte comando
```
lein run
```

A saída deverá ser semelhante a esta.
```
Hey Hello, World
```

Se abrirmos o arquivo ``core.clj`` dentro de ``src``, veremos que nossa
função principal foo recebe um parâmetro, que foi o ``Hey`` que mandamos
por linha de comando e imprime a frase ``Hey Hello World``.
<script src="https://gist.github.com/Jciel/8f9f9da49f023235f37221986a0fb2cb.js"></script>

<br>

#### __Instalando dependências__
Para instalar uma dependência no projeto com o Leiningen é bem simples,
similar a gerenciadores de dependências de outras linguagens, no arquivo
``project.clj`` devemos adicionar as dependências que queremos instalar.
Para esse exemplo utilizaremos a biblioteca ``clj-http``, que possibilita
fazermos requisições http, então devemos adicioná-la na propriedade
``:dependencies`` do arquivo, ficando similar ao código a seguir.
<script src="https://gist.github.com/Jciel/8a163db77c2e790dda5c1e1e0db37074.js"></script>

todas as dependências que necessitamos adicionamos nesse formato, após
isto podemos rodar o seguinte comando para o leiningen fazer a
instalação das mesmas.
```
lein deps
```

Após a finalização, a biblioteca estará disponível para usarmos, vamos
abrir nosso arquivo principal ``core.clj`` e modificá-lo usando a biblioteca
que acabamos de instalar para fazer uma requisição a uma API.
<script src="https://gist.github.com/Jciel/af8988fa6241913e7d42c201b7ecb803.js"></script>

Na ``linha 2`` fazemos um require do namespace ``clj-http.client`` e colocando
como álias client, na ``linha 7`` estamos utilizando a função get fazendo
uma requisição para o endpoint passado como parâmetro e imprimindo o
resultado.

<br>

#### __Conclusão__
Essa é a estrutura básica de um projeto Clojure, como instalar
dependências no projeto, a partir disto podemos evoluir para algo mais
complexo, para ver mais recursos do Leiningen acesse o site do projeto
https://leiningen.org.  

Dúvidas, sugestões... podem deixar comentários, e compartilhar caso gostou :D.





