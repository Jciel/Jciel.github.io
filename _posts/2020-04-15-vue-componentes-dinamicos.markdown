---
layout: post
imageOg: "https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg"
title: "Vue - Componentes dinâmicos"
subtitle: ""
date: 2020-04-15 12:00:25 -0300
categories: deselvolvimento
excerpt: "VueJS já se consolidou como um ótimo framework para criação de interfaces web e SPA’s, o foco do Vue é a criação de 
          componentes independentes e reutilizáveis, cada componente contém seu próprio layout, estilo e lógica. Nesse artigo 
          tentarei mostrar um pouco mais sobre componentes dinâmicos..."
author:
  name: {{ author.name }}
  email: {{ author.email }}
---

<br>

VueJS já se consolidou como um ótimo framework para criação de interfaces web e SPA’s, o foco do Vue é a criação de 
componentes independentes e reutilizáveis, cada componente contém seu próprio layout, estilo e lógica. Nesse artigo 
tentarei mostrar um pouco mais sobre componentes dinâmicos, então assume um conhecimento inicial de Vue como criação de 
componentes, Single File Components (SFC), comunicação entre componentes entre outros conhecimentos básicos sobre o 
framework. 

![](https://cdn-images-1.medium.com/max/2000/1*oT0-Tqda8sGMdKH0SHPbOw.jpeg)
<div class="img-legend">Nate Grant em Unsplash</div>

<br>

#### __Contexto e definição do exemplo__  
Imaginamos uma aplicação onde o usuário inicia um fluxo de cancelamento de uma conta de usuário em três etapas, certo. 
Então temos uma tela inicial, a primeira tela do fluxo de cancelamento, nessa primeira tela desse fluxo o usuário 
adicionará algumas informações, e também existirá um botão para a próxima tela, na segunda telá novamente o usuário irá 
selecionar algumas opções e novamente um botão para a última tela, na ultima tela o usuário irá confirmar o cancelamento.

<br>

Com esse exemplo pensamos... Fácil! Podemos criar três seções na página e uma variável de controle para exibir cada 
tela no seu momento oportuno certo? Certo! podemos sim, vamos ver como fica? Let’s code  

Irei criar um projeto utilizando o vue cli.

```bash
vue create steps
```   

<br>

Eu criei um projeto e um “componente” com três ‘seções’, cada uma representando uma etapa do fluxo descrito. Cada etapa 
tem um botão que passa para a próxima tela do fluxo, passando o nome da próxima tela para um método que atribui para uma 
variável de controle, utilizando ``v-if`` nas seções o vue decide qual tela mostrar. 

```vue
<template>
  <div class="page-container">
    <div
      v-if="step === 'firstStep'"
      class="form-container first-step">
      <form class="form">
        <input type="text" v-model="form.email" class="input email" id="email" placeholder="E-mail">
        <input type="text" v-model="form.name" class="input name" id="name" placeholder="Name">
      </form>
      <button @click="nextStep('secondStep')">Next</button>
    </div>

    <div
      v-if="step === 'secondStep'"
      class="form-container second-step">
      <form class="form">
        <input type="text" v-model="form.user" class="input user" id="user" placeholder="User">
        <input type="text" v-model="form.codeid" class="input codeid" id="codeid" placeholder="Code Id">
      </form>
      <button @click="nextStep('thirdStep')">Next</button>
    </div>

    <div
      v-if="step === 'thirdStep'"
      class="form-container third-step">
      <p>
        Você tem certeza que quer cancelar sua conta?
      </p>
      <button>Confirmar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Steps',

  data () {
    return {
      form: {
        email: '',
        name: '',
        user: '',
        codeid: ''
      },
      step: 'firstStep'
    }
  },

  methods: {
    nextStep(step) {
      this.step = step
    }
  }
}
</script>
```  
 
<br>
Funciona, mas podemos perceber que foge da ideia do vue de separar em componentes, temos as três telas em um aquivo só, 
além de, caso precise de uma lógica mais complexa iriam também ficar no mesmo arquivo.  
Com o Vue.js, podemos utilizar componentes dinâmicos, usando ``<component is="">`` podemos definir qual componente será 
renderizado pelo seu nome, vamos refatorar o código acima separando em componentes e utilizando essa funcionalidade.   

<br>

Primeiramente vamos criar um componente para cada tela do fluxo de cancelamento, ``FirstStep.vue``, ``SecondStep.vue`` e 
``ThirdStep.vue``.  

``FirstStep.vue``   
```vue
<template>
  <div
    class="form-container first-step">
    <form class="form">
      <input type="text" v-model="form.email" class="input email" id="email" placeholder="E-mail">
      <input type="text" v-model="form.name" class="input name" id="name" placeholder="Name">
    </form>
    <button @click="nextStep()">Next</button>
  </div>
</template>

<script>
  export default {
    name: "first-step",

    props: {
      form: {
        type: Object,
      }
    },

    methods: {
      nextStep () {
        this.$emit('nextStep', {step: 'second-step', form: this.form})
      }
    }
  }
</script>
```   

``SecondStep.vue``  
```vue
<template>
  <div
    class="form-container second-step">
    <form class="form">
      <input type="text" v-model="form.user" class="input user" id="user" placeholder="User">
      <input type="text" v-model="form.codeid" class="input codeid" id="codeid" placeholder="Code Id">
    </form>
    <button @click="nextStep()">Next</button>
  </div>
</template>

<script>
  export default {
    name: "second-step",

    props: {
      form: {
        type: Object,
      }
    },

    methods: {
      nextStep () {
        this.$emit('nextStep', {step: 'third-step', form: this.form})
      }
    }
  }
</script>
```

``ThirdStep.vue``   
```vue
<template>
  <div
    class="form-container third-step">
    <p>
      Você tem certeza que quer cancelar sua conta?
    </p>
    <button @click="finish">Confirmar</button>
  </div>
</template>

<script>
  export default {
    name: "third-step",

    props: {
      form: {
        type: Object,
      }
    },

    methods: {
      finish () {
        alert(JSON.stringify(this.form))
      }
    }
  }
</script>
```   

<br>

Em cada um coloquei o código respectivo de cada etapa, cada um também define uma ``props`` que sera os dados do formulário 
de cada etapa, e um método que será executado quando clicar no botão de próxima tela, o qual emite um evento ``nextStep`` 
com um objeto contendo o nome da próxima tela e os dados do formulário.   

<br>

Também criamos um componente que será a "base", onde renderizaremos as telas das etapas de cancelamento.   

``InitialPage.vue``   
```vue
<template>
  <div class="page-container">
    <component
      :is="step"
      :form="form"
      @nextStep="nextStep"/>
  </div>
</template>

<script>
  import FirstStep from "./FirstStep";
  import SecondStep from "./SecondStep";
  import ThirdStep from "./ThirdStep";

  export default {
    name: "initial-page",

    components: {
      'first-step': FirstStep,
      'second-step': SecondStep,
      'third-step': ThirdStep
    },

    data() {
      return {
        step: 'first-step',
        form: {
          email: '',
          name: '',
          user: '',
          codeid: ''
        }
      }
    },

    methods: {
      nextStep(data) {
        this.form = data.form
        this.step = data.step
      }
    }
  }
</script>
```   

Aqui importamos todos os componentes das telas, utilizamos ``<component :is="" />`` passando o nome de cada tela que é 
armazenado em uma variável, também colocamos um ``listener`` para o evento ``nextStep`` que é emitido por cada tela do 
fluxo de cancelamento, quando uma tela emite esse evento, é executado o método ``nextStep`` que recebe os dados enviados 
pela etapa atual, atribui os dados do formulário para a variável form que será passado como prop para a próxima tela e 
o nome da tela recebido que ser renderizado pelo ``<component is=""``. Desta forma cada tela sabe qual a próxima tela, 
evitamos utilizar condicionais e criamos componentes separados mantendo a ideia do framework.   

<br>
<br>

#### __Conclusão__  
Essa é uma funcionalidade bem bacana do VueJS e pode ser bastante útil e poderosa, só quis mostrar um caso de uso possível 
dessa feature :P   
Comentem se gostaram, compartilhem se conhecem alguém a quem possa ser útil este texto e até o próximo.

<br>




##### __Referências:__
- https://br.vuejs.org/v2/guide/components-dynamic-async.html
