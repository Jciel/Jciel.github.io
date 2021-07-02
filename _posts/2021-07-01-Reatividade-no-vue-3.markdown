---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg"
title: "Vue - Reatividade no VueJS 3.0"
subtitle: "Diferença entre reactive, ref, toRef e toRefs"
date: 2021-07-01 12:00:25 -0300
categories: deselvolvimento
excerpt: "O VueJS na sua versão 3.0 mudou um pouco como definir a forma de reatividade do dados que são criados e 
          utilizados nos componentes, vamos ver um pouco como está funcionando com essas mudanças..."
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>

O VueJS na sua versão 3.0 mudou um pouco como definir a forma de reatividade do dados que são criados e utilizados nos 
componentes, vamos ver um pouco como está funcionando com essas mudanças.   

<br>

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __ES6 Proxy__   
O ES6 trouxe uma nova funcionalidade na linguagem chamada __Proxy__.

> Também em Design Patterns em Orientação a Objetos temos um padrão chamado Proxy, esse padrão estrutural fornece 
> uma interface de acesso, um controlador de acesso, um interceptor, a um objeto.  

E é exatamente isso que essa nova funcionalidade nos fornece, uma maneira de adicionar um comportamento customizados ao 
acessar ou modificar um objeto. Duas das funções principais são ``get`` e ``set`` utilizadas para recuperar o valor de 
uma propriedade e alterar/definir uma propriedade e seu valor respectivamente.  

```typescript
const obj = {
    name: "Marcelo",
    age: 20
}

const objProxy = new Proxy(obj, {
    // target é o objeto original ao qual foi criado o proxy (obj)
    // propertie é a propriedade que estamos tentandop acessar (name ou age)
    get: (target, propertie) => {
        console.log(`Acessando a propriedade ${propertie}`)
        return target[propertie]
    },
    
    // target é o objeto original ao qual foi criado o proxy (obj)
    // propertie é a propriedade que estamos tentandop acessar (name ou age)
    // value é o novo valor que será atribuida a propriedade propertie
    set: (target, propertie, value) => {
        console.log(`Alterando o valor da propriedade ${propertie}`)
        target[propertie] = value
    }
})
```
As funções ``get`` e ``set`` serão executadas toda vez que tentarmos acessar ou alterar uma propriedade a partir do 
proxy criado, e podemos definir qualquer tipo de lógica para esse Proxy.   

```typescript
objProxy.name // Acessando a propriedade name Marcelo

objProxy.age = 25 // Alterando o valor da propriedade age 25
```
O funcionamento do __Proxy__ do ES6 é a base do funcionamento da reatividade no Vue 3 implementada com algumas funções 
fornecidas pela ``Composition API`` do VueJS 3.0.   

<br>

#### __reactive__   

Utilizado quando precisamos adicionar reatividade em um objeto natural qualquer que tivermos, ``reactive`` cria um wrapper 
proxy para esse objeto, a partir dai podemos trabalhar apenas com o objeto proxy para termos um comportamento reativo.      
```typescript
import { reactive } from "vue"

const state = {
	userName: "Marcelo",
	userAge: 23
}
const stateProxy = reactive(state)

// OU

const statePx = reactive({
	userName: "Marcelo",
	userAge: 23
})
```
Desta forma o ``stateProxy`` estará com comportamento reativo e qualquer alteração dos dados será refletido onde for 
utilizado esse objeto, __uma alteração feita pelo proxy resultará em uma alteração no objeto original__.   

<br>

#### __ref__  
Cria um objeto ``ref`` com comportamento reativo a partir de um valor primitivo ou array, esse objeto tem um única 
propriedade chamada ``value`` que contém o valor definido, se um objeto for passado como valor, será criado um proxy 
com a função ``reactive`` e esse proxy será atribuído ao ``value`` da ``ref`` que criamos.   
```typescript
import { ref } from "vue"

const age = ref(23)

console.log(age.value) // 23
console.log(age)    // RefImpl {
                    //    __v_isRef: true
                    //    _rawValue: 23
                    //    _shallow: false
                    //    _value: 23
                    //    value: 23
                    //    __proto__: Object
                    // }

const state = ref({ age: 23 })
console.log(state.value)    // Proxy {
                            //		[[Handler]]: Object
                            //		[[Target]]: Object
                            //		   age: 23
                            //		__proto__: Object
                            //		[[IsRevoked]]: false
                            // }

console.log(state.value.age)    // 23
```

<br>

#### __toRef__   
Podemos utilizar ``toRef`` para criar um ``ref`` a partir de uma propriedade de um objeto proxy ``reactive``, ou seja, 
que foi criado com a função ``reactive``, esse novo ``ref`` mantém a conexão com o proxy original, então caso haja uma 
alteração em qualquer um dos objetos essa alteração será refletida no outro objeto.   
```typescript
import { reactive, toRef } from "vue"

const objProxy = reactive({ userName: "Marcelo", userAge: 23 })

const userName = toRef(objProxy, "userName")
console.log(userName.value) // Marcelo
console.log(userName)   // ObjectRefImpl {
                        //	__v_isRef: true
                        //	_key: "userName"
                        //	_object: Proxy        - Referência ao objeto proxy de origem -
                        //		[[Handler]]: Object
                        //		[[Target]]: Object
                        //		[[IsRevoked]]: false
                        //	value: "Marcelo"
                        //	__proto__: Object
                        // }

userName.value = "João"
console.log(objProxy.userName)  // João
```

<br>

#### __toRefs__   
``toRefs`` é semelhante ao ``toRef``, com ``toRefs`` podemos passar um objeto proxy ``rective`` e irá ser criado um 
__objeto simples__ com cada propriedade existente do proxy sendo um ref.   

> Se aplicarmos destructuring a um proxy reactive perdemos a reatividade desses dados.  

Mas utilizando ``toRefs`` podemos aplicar destructuring e manter a característica de reatividade 
dos dados.   
```typescript
import { reactive, toRefs } from "vue"

const objProxy = reactive({ userName: "Marcelo", userAge: 23 })

const refsObject = toRefs(objProxy)
console.log(refsObject) // {
                        //    userAge: ObjectRefImpl
                        //	    __v_isRef: true
                        //	    _key: "userAge"
                        //	    _object: Proxy {userName: "Marcelo", userAge: 23}
                        //	    value: 23
                        //
                        //    userName: ObjectRefImpl
                        //	    __v_isRef: true
                        //	    _key: "userName"
                        //	    _object: Proxy {userName: "Marcelo", userAge: 23}
                        //	    value: "Marcelo"
                        // }

const { userName, userAge } = toRefs(objProxy)
console.log(userName)   // ObjectRefImpl {
                        //	 __v_isRef: true
                        //	 _key: "userName"
                        //	 _object: Proxy {userName: "Marcelo", userAge: 23}
                        //	 value: "Marcelo"
                        // }

console.log(userAge)    // ObjectRefImpl {
                        //		__v_isRef: true
                        //		_key: "userAge"
                        //		_object: Proxy {userName: "Marcelo", userAge: 23}
                        //		value: 23
                        // }
```

<br>
<br>

##### __Referências:__
- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Proxy
- https://v3.vuejs.org/api/basic-reactivity.html
- https://v3.vuejs.org/api/refs-api.html

