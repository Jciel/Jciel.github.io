---
layout: post
title: "A vida com menos loops em PHP"
date: 2018-02-01 14:39:38 -0300
categories: deselvolvimento
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

O array é umas das estruturas de dados mais conhecidas e utilizadas em PHP, um array é na verdade um mapa ordenado, uma estrutura que relaciona valores a chaves, e para manipular/usar os elementos de um array frequentemente utilizamos loops como for, foreach, entre outros, mas existem funções que podemos usar e que deixam nosso código mais legível e facilitam o entendimento, mas que poucas vezes são usados, vamos dar uma olhada em algumas dessas funções.  

<div class="img-container">
	<img src="http://i64.tinypic.com/11gn987.jpg">
</div>

<br>

__array_seach__  
Começando com algo mais simples, array_seach pesquisa por um valor no array passado e retorna a chave desse valor caso exista.
<script src="https://gist.github.com/Jciel/52d90dacc47f7e60d740f21e79d4e0b0.js"></script>
  

__array_diff__  
Compara o primeiro array passado como parâmetro com os demais e retorna a um array com os valores que existem no primeiro e não existem em nenhum dos demais.
<script src="https://gist.github.com/Jciel/2a667a40d9dd7aef964aa8f48959137d.js"></script>


__array_count_values__  
Array_count_values retorna um array com a quantidade que cada valor aparece, com o valor como chave e sua quantidade como valor, vamos ver um exemplo, digamos que queremos ver quantas vezes cada palavra aparece na seguinte frase: “Vou viajar, mas para viajar preciso de passagem, mas para passagem, preciso de dinheiro”.
<script src="https://gist.github.com/Jciel/793d5c25c909c883c782f33dd7abce2f.js"></script>


__array_walk__  
Array_walk aplica uma determinada função callback em cada elemento do array passado, array_walk recebe o array como referência, o primeiro parâmetro da função callback será o valor, o segundo a chave do valor, array_walk aceita um parâmetro opcional que será passado como terceiro parâmetro para a função callback.
<script src="https://gist.github.com/Jciel/8b3ce5518cab82fc4402fa8d62ae060d.js"></script>


__array_column__  
Esta função retorna um array com os valores de uma determinada coluna.  
Também podemos definir uma coluna para ser as chaves do array retornado.
<script src="https://gist.github.com/Jciel/d0aa896e0ac462c827d4525bb2b4d0c3.js"></script>


__array_filter__  
Aplica uma função callback em todos os elementos de um array, se essa função retornar true o elemento é adicionado no array de retorno, as chaves são preservadas.
<script src="https://gist.github.com/Jciel/b8690af162a993218bcd598673d22731.js"></script>


__array_map__  
Talvez uma função já conhecida, array_map aplica uma função callback em cada elemento do array retornando um novo array com os elementos depois de aplicada a função. Podemos passar mais de um array, mas o número de parêmtros que a função callback aceita deve ser o mesmo que o de arrays passado para a função array_map.
<script src="https://gist.github.com/Jciel/910fc5eb916ae6270d6ffd0ec9bea17f.js"></script>


__array_reduce__  
Outra função que talvez já ouviram falar, array_reduce reduz um array a um único valor aplicando uma função callback a cada elemento do array, o resultado da função aplicada entra como parâmetro para a próxima chamada com o próximo elemento.  
Vamos ver um exemplo, queremos calcular a soma de todos os produtos daquela tabela que utilizamos no exemplo de array_column.
<script src="https://gist.github.com/Jciel/be961121738a6067272d892ef655cc6d.js"></script>


__Concluindo…__  
As funções callbacks podem ser passadas diretamente no formato de função anonima. Os códigos acima foram rodados no PHP 7.2.  
Dei alguns exemplos simples de uso dessas funções, mas podemos usá-las em diversas situações mais complexas, onde provavelmente usaríamos loops, talvez deixando mais complexas ainda.  
Com essas funções, [entre outras](https://secure.php.net/manual/pt_BR/ref.array.php), podemos evitar de usar loops e deixar nossos códigos menos imperativos, mais simples e fáceis de ler e consequentemente de dar manutenção.  

