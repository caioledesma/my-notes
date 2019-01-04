# Vue.js

- [Vue.js](#vuejs)
  - [O que é Vue?](#o-que-é-vue)
  - [Iniciando](#iniciando)
  - [Estrutura de pastas](#estrutura-de-pastas)
    - [Onde está o conteúdo?](#onde-está-o-conteúdo)
  - [Global View Object](#global-view-object)
  - [Interpolação e Data Binding](#interpolação-e-data-binding)
  - [Adequando o template](#adequando-o-template)
    - [Adicionando dados](#adicionando-dados)
  - [Laços de repetição](#laços-de-repetição)
  - [Obtendo dados através de requests](#obtendo-dados-através-de-requests)
    - [Vue Resource](#vue-resource)
    - [Axios](#axios)
    - [Plugins](#plugins)
  - [Estilizando um componente](#estilizando-um-componente)
  - [Criando um novo componente](#criando-um-novo-componente)
    - [Comunicação entre componentes](#comunicação-entre-componentes)
    - [Incluindo o componente](#incluindo-o-componente)
    - [Slots](#slots)
  - [Estilos e escopo](#estilos-e-escopo)

## O que é Vue?

Framework para desenvolvimento de aplicacões SPA que busca o melhor de outros frameworks como o Angular e o React.

- Mais rápido
- Menor
- Mais simples

## Iniciando

Primeiramente precisamos instalar o CLI do vue para começar um projeto:

```sh
$ npm install -g vue-cli
```

Depois podemos utilizar o comando `vue init <template>` para criar nosso projeto do 0. Por exemplo:

```sh
$ vue init webpack-simple
```

> Note que existem vários templates de Vue presentes [no website oficial](https://github.com/vuejs-templates).

Podemos então simplesmente executar `npm run dev` para ter nossa primeira página VueJS no ar.

## Estrutura de pastas

Ao abrirmos a pasta do nosso projeto (neste documento como `vuejs-test`), vamos ver que temos um arquivo `index.html` na raiz que não contém **absolutamente nada**. Isto porque todo o conteúdo é parseado e compilado dentro de um arquivo chamado `build.js` que é então carregado para o navegador. Ele que vai conter todo o conteúdo da página e todo o conteúdo da pasta `src`.

Se abrirmos o nosso arquivo `package.json` vamos ver que temos uma série de dependências:

```json
{
  "name": "vuejs-test",
  "description": "Vue JS test project",
  "version": "1.0.0",
  "author": "Lucas Santos <lhs.santoss@gmail.com>",
  "license": "MIT",
  "private": true,
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "dependencies": {
    "vue": "^2.5.11"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ],
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-env": "^1.6.0",
    "babel-preset-stage-3": "^6.24.1",
    "cross-env": "^5.0.5",
    "css-loader": "^0.28.7",
    "file-loader": "^1.1.4",
    "node-sass": "^4.5.3",
    "sass-loader": "^6.0.6",
    "vue-loader": "^13.0.5",
    "vue-template-compiler": "^2.4.4",
    "webpack": "^3.6.0",
    "webpack-dev-server": "^2.9.1"
  }
}
```

Temos o babel instalado juntamente com plugins, além disso temos o webpack que vai criar o bundle que vai empacotar nosso bundle para o arquivo `build.js`.

### Onde está o conteúdo?

O conteúdo da nossa página está presente dentro da pasta `src` no arquivo `app.vue`:

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
```

Este arquivo é o que é chamado de *single file template*. Ou seja, é um módulo de um único arquivo com um template interno. Dentro deste arquivo temos um componente chamado `app` declarado. Todo arquivo Vue é baseado em três seções importantes:

- Template: É aonde vamos ter a estrutura do nosso componente, seria onde o HTML irá ficar
- Script: É onde vamos definir o comportamento do nosso componente
- Style: É onde vamos definir o estilo do nosso componente
  - Em style, podemos ter vários pré processadores utilizando o atributo `lang`, no nosso caso está setado para `scss`, mas podemos ter `less`, `stylus` ou então vazio, que seria o CSS normal sem nenhum pré processamento

## Global View Object

Vemos que em nosso arquivo `main.js` temos um conteúdo bastante simples: Ele só contém o código de inicialização que vai indicar aonde nosso componente será renderizado:

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: '#app',
  render: h => h(App)
})
```

Veja que estamos renderizando o componente `App` dentro do elemento com ID `app`, veja que, para podermos fazer isso tudo funcionar, precisamos do que é chamado de *Global View Object*, que é o módulo `vue` importado na primeira linha. Depois uma nova instância do Vue é criada através de uma *Vue Instance*, passando um objeto de configuração para este objeto.

Este objeto de configuração é bastante simples, temos um `el` que será o seletor CSS que vai indicar aonde vamos renderizar nosso componente e também temos um método `render` que leva um argumento `h` que é o renderizador recebendo o componente que vai ser rederizado.

## Interpolação e Data Binding

A tag `<template>` é onde vamos criar a nossa visualização. Se observarmos bem o nosso arquivo `App.vue` vamos ver que temos um placeholder `{{ msg }}`. É desta forma que fazemos a interpolação de dados no Vue, colocando nossa variável dentro de `{{ }}`.

O conteúdo desta variável pode vir de outro componente externo, ou então pode vir do próprio componente, como é o nosso caso. Quando a variável está dentro do próprio componente, temos uma função especial chamada `data` que fica na seção `scripts` que permite que exportemos um objeto com todos os dados que desejamos que nosso componente tenha acesso:

```vue
<script>
export default {
  name: 'app',
  data () {
    return {
      variavel: 'valor'
    }
  }
}
</script>
```

Essa função `data` precisa sempre retornar um objeto JS e **todas as interpolações são *data bindings***, ou seja, quando estamos interpolando um valor, na verdade, estamos criando um *data binding* unidirecional que flui sempre da **origem do dado** para o **destino do dado** e não ao contrário como tínhamos no Angular 1.

## Adequando o template

Para podermos criar a nossa aplicação não podemos nos contentar somente com o que o Vue nos deu por padrão não é mesmo? Temos que adequar este template ao que queremos criar. Vamos começar removendo o conteúdo do arquivo `App.vue` e deixando apenas as seções principais:

```vue
<template>

</template>

<script>
export default {

}
</script>

<style lang="scss">

</style>
```

Na nossa aplicação, vamos exibir uma lista de fotos. Para isso vamos começar colocando uma tag `<img>` com uma foto qualquer:

```vue
<template>
<img src="https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg" alt="Cachorro">
</template>

<script>
export default {

}
</script>

<style lang="scss">

</style>
```

Vamos agora adicionar um título à nossa aplicação:

```vue
<template>
  <h1>PicLib</h1>
  <img src="https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg" alt="Cachorro" />>
</template>

<script>
export default {

}
</script>

<style lang="scss">

</style>
```

Agora, se recarregarmos a página de testes, vamos ver que temos um erro e nada é exibido:

> Component template should contain exactly one root element

Isso significa que nossa tag `<template>` não pode ter múltiplas tags filhas, tudo deve estar aninhado dentro do mesmo elemento inicial. Podemos resolver este problema envolvendo os elementos em uma tag container qualquer:

```vue
<template>
  <div id="app">
    <h1>PicLib</h1>
    <img src="https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg" alt="Cachorro" />
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss">

</style>
```

### Adicionando dados

Vamos adicionar dados ao nosso template através da função `data`:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <img src="https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg" alt="Cachorro" />
  </div>
</template>

<script>
export default {
  data() {
    return { appTitle: 'PicLib' }
  }
}
</script>

<style lang="scss">

</style>
```

Além disso, como vamos ter uma lista de imagens no futuro, temos que ter a capacidade de passar tudo o que nossa imagem necessita através de parâmetros também, vamos criar outra variável:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <img src="{{ foto.url }}" alt="{{ foto.alt }}" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      foto: {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      }
    }
  }
}
</script>

<style lang="scss">

</style>
```

Vamos ter um erro dizendo: **"Interpolation inside attributes has been removed. Use v-bind or the colon shorthand instead. For example, instead of <div id="{{ val }}">, use <div :id="val">"**. Isto significa que, dentro de atributos, não podemos utilizar interpolação, vamos ter que utilizar um novo modelo de interpolação utilizando o modelo `v-bind:<attr>`:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <img v-bind:src="foto.url" v-bind:alt="foto.alt" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      foto: {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      }
    }
  }
}
</script>

<style lang="scss">

</style>
```

Além disso, podemos usar uma forma mais simples somente com o `:`:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <img :src="foto.url" :alt="foto.alt" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      foto: {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      }
    }
  }
}
</script>

<style lang="scss">

</style>
```

Interpolações, na verdade, são *shorthands* para um atributo chamado `v-text` que podemos utilizar, então, ao invés de `{{ appTitle }}` podemos remover o conteúdo do nosso `h1` alterando para o seguinte modelo:

```vue
<template>
  <div id="app">
    <h1 v-text='appTitle'></h1>
    <img :src="foto.url" :alt="foto.alt" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      foto: {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      }
    }
  }
}
</script>

<style lang="scss">

</style>
```

## Laços de repetição

E se quisermos adicionar mais de uma foto? Podemos simplesmente adicionar mais uma imagem no template, certo?

```vue
<template>
  <div id="app">
    <h1 v-text='appTitle'></h1>
    <img :src="foto.url" :alt="foto.alt" />
    <img :src="foto2.url" :alt="foto2.alt" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      foto: {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      },
      foto2: {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro2'
      }
    }
  }
}
</script>

<style lang="scss">

</style>
```

Porém sabemos que isso se torna inviável ao longo do tempo, trabalhar com objetos individuais não é escalável, então vamos começar a trabalhar com arrays. E, para isso, vamos criar um elemento de lista no nosso HTML:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li>
        <img :src="foto.url" :alt="foto.alt" />
      </li>
      <li>
        <img :src="foto2.url" :alt="foto2.alt" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      fotos: [{
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      },
      {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro2'
      }]
    }
  }
}
</script>

<style lang="scss">

</style>
```

Agora vamos iterar pelo array para exibir nossas fotos com a diretiva `v-for`:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for='foto of fotos'>
        <img :src="foto.url" :alt="foto.alt" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      fotos: [{
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      },
      {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro2'
      }]
    }
  }
}
</script>

<style lang="scss">

</style>
```

> **Nota**
> A diretiva `v-for` pode ser utilizada tanto com `in` quanto com `of` como em `v-for='foto in fotos'` ou `v-for='foto of fotos'`

Outro detalhe importante é que é *recomendável* termos uma chave para cada item da lista que é iterado, a melhor forma de fazermos isto com arrays é utilizando seu próprio índice, podemos obter um índice do `v-for` colocando uma tupla do tipo `(foto, index)` da seguinte forma:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for='(foto, index) of fotos'>
        <img :src="foto.url" :alt="foto.alt" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      fotos: [{
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      },
      {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro2'
      }]
    }
  }
}
</script>

<style lang="scss">

</style>
```

E por fim utilizamos um `v-bind:key` (ou apenas `:key`) para criar a chave:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for='(foto, index) of fotos' :key="index">
        <img :src="foto.url" :alt="foto.alt" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: 'PicLib',
      fotos: [{
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro'
      },
      {
        url: 'https://statig2.akamaized.net/bancodeimagens/2c/43/2l/2c432l7iz802ihqs98uzjic6x.jpg',
        alt: 'Cachorro2'
      }]
    }
  }
}
</script>

<style lang="scss">

</style>
```

## Obtendo dados através de requests

Todos sabemos que, na maioria dos casos, todas as nossas imagens vão vir a partir de uma API externa. Esta API externa vai nos dar uma lista de imagens que vamos poder ler e incluir na nossa página:

Para podermos utilizar o poder das APIs, vamos instalar um novo módulo chamado *vue-resource* através do comando `npm i vue-resource`.

>**Nota**
>O `vue-resource` não é mais a recomendação da documentação oficial, agora podemos utilizar o Axios para fazer requests

### Vue Resource

Depois de fazer a instalação do `vue-resource`, vamos registrá-lo no nosso arquivo `main.js`:

```js
import Vue from 'vue'
import App from './App.vue'

import VueResource from 'vue-resource'

Vue.use(VueResource)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

Agora em nosso componente `App.vue` vamos esvaziar a nossa lista:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for="(foto, index) of fotos" :key="index">
        <img :src="foto.url" :alt="foto.alt">
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: "PicLib",
      fotos: []
    }
  }
};
</script>

<style lang="scss">
</style>
```

**Hooks**

Todos os componentes do Vue tem o que é chamado de *lifecycle hooks*, esta é uma funcionalidade que nos permite executar um código em um determinado momento do componente.

> Para mais informações sobre os lifecycles, veja [este link com explicações](https://alligator.io/vuejs/component-lifecycle/) e o [link da documentação oficial](https://vuejs.org/v2/api/#Options-Lifecycle-Hooks)

A função que queremos é chamada `created`, que é executada sempre que o componente é criado. Podemos incluir juntamente com a nossa definição de dados:

```vue
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for="(foto, index) of fotos" :key="index">
        <img :src="foto.url" :alt="foto.alt">
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: "PicLib",
      fotos: []
    }
  },
  created() {}
};
</script>

<style lang="scss">
</style>
```

Agora podemos utilizar o `vue-resource` para podermos buscar nossas fotos. Imagine que temos uma API imaginária que nos trará algumas fotos:

```vue
<template>
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for="(foto, index) of fotos" :key="index">
        <img :src="foto.url" :alt="foto.alt">
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: "PicLib",
      fotos: []
    }
  },
  created() {
    this.$http
      .get("http://localhost:3000/fotos")
      .then(res => res.json())
      .then(fotos => (this.fotos = fotos));
      .catch(console.error)
  }
};
</script>

<style lang="scss">
</style>
```

Veja que podemos acessar a propriedade `fotos` que temos na função `data` através do objeto `this`. O método `created` é **síncrono** então não temos como usar `async/await` para ele.

### Axios

A biblioteca padrão que a documentação oficial indica é o Axios. O Axios é uma biblioteca genérica de requests, para podermos utiliza-la, vamos ter que instalar dois módulos, o Axios em si e o wrapper que transforma a biblioteca em um plugin do Vue, chamado `vue-axios`:

```sh
$npm i vue-axios axios
```

Agora vamos no nosso arquivo `main.js`:

```js
import Vue from 'vue'
import App from './App.vue'

import VueResource from 'vue-resource'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueResource)
Vue.use(VueAxios, axios)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

Isso exportará para nós o método `this.$http` e também `this.axios` para ser utilizado na aplicação. A partir daí podemos realizar exatamente a mesma ideia que desenvolvemos no bloco anterior, com as devidas mudanças por conta da biblioteca:

```vue
<template>
<template>
  <div id="app">
    <h1>{{ appTitle }}</h1>
    <ul>
      <li v-for="(foto, index) of fotos" :key="index">
        <img :src="foto.url" :alt="foto.alt">
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      appTitle: "PicLib",
      fotos: []
    }
  },
  created() {
    this.$http
      .get("http://localhost:3000/fotos")
      .then(response => (this.fotos = response.data));
      .catch(console.error)
  }
};
</script>

<style lang="scss">
</style>
```

### Plugins

Um plugin do Vue nada mais é do que uma função que exporta um método install que recebe uma instancia do Vue e uma instancia de opções. Podemos tirar vantagem disso para criar nossos próprios plugins e não depender de libs como a `vue-axios`. Vamos criar um arquivo na pasta `src/plugins` chamado `axios.js`:

```js
import axios from 'axios'

let AxiosPlugin = {}

AxiosPlugin.install = (Vue, options) => {
  Vue.prototype.$http = axios
  Vue.prototype.$axios = axios
}

export default AxiosPlugin
```

Agora, no nosso arquivo `main.js` vamos importar o módulo e utilizar

```js
import Vue from 'vue'
import Axios from './plugins/axios'
import App from './App.vue'

Vue.use(Axios)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

E então podemos continuar chamando como estávamos chamando anteriormente.

## Estilizando um componente

O Vue, juntamente com outras bibliotecas front-end, é conhecida pela criação de componentes que permitem não só levar o estilo mas também o comportamento de telas inteiras ou pedaços dela para outros lugares sem precisarmos fazer nada.

Na nossa página vamos precisar estilizar algumas coisas, então vamos começar criando classes e wrappers para o que vamos ter que fazer

```html
<template>
  <div id="app" class="site-body">
    <h1 class="site-title centered">{{ appTitle }}</h1>
    <ul class="image-list">
      <li v-for="(foto, index) of fotos" :key="index" class="image-item">
        <img :src="foto.url" :alt="foto.alt" width="100" height="100" class="image">
      </li>
    </ul>
  </div>
</template>
```

Na nossa seção `style` vamos adicionar os nosso estilos:

```vue
<style lang="scss">
.site-body {
  font-family: Arial, Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}

.site-title {
  .centered {
    text-align: center;
  }
}

.image-list {
  list-style: none;

  .image-item {
    display: inline-block;
  }
}
</style>
```

Vamos colocar cada imagem dentro de um painel e cada painel terá um título, isso pede algumas mudanças de layout. Vamos criar o nosso painel:

```html
<template>
  <div id="app" class="site-body">
    <h1 class="site-title centered">{{ appTitle }}</h1>
    <ul class="image-list">
      <li v-for="(foto, index) of fotos" :key="index" class="image-item">
        <div class="painel">
          <h2 class="painel-titulo">{{ foto.alt }}</h2>
          <div class="painel-conteudo">
            <img :src="foto.url" :alt="foto.alt" width="100" height="100" class="image">
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>
```

E agora vamos aplicar o estilo como fizemos ao outro:

```vue
<style lang="scss">
.site-body {
  font-family: Arial, Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}

.site-title {
  .centered {
    text-align: center;
  }
}

.image-list {
  list-style: none;

  .image-item {
    display: inline-block;
  }
}

.painel {
  padding: 0 auto;
  border: solid 2px grey;
  display: inline-block;
  margin: 5px;
  box-shadow: 5px 5px 10px grey;
  width: 260px;
  height: 100%;
  vertical-align: top;
  text-align: center;

  .painel-titulo {
    text-align: center;
    border: solid 2px;
    background: lightblue;
    margin: 0 0 15px 0;
    padding: 10px;
    text-transform: uppercase;
  }
}
</style>
```

## Criando um novo componente

E se quiséssemos utilizar um desses paineis em outro lugar? Vamos transformá-lo em um componente compartilhado. O primeiro passo é criar uma pasta chamada `components` dentro da nossa `src`. Dentro desta pasta vamos criar uma nova pasta `shared` e dentro dela uma outra pasta com o nome do nosso componente, por fim vamos ter um arquivo `Painel.vue` dentro desta pasta.

> Algumas convenções são utilizadas para facilitar o desenvolvimento e a padronização de projetos. A organização de pastas neste modelo é uma convenção bem conhecida, o uso de CamelCase para a nomenclatura dos componentes é outra.

Nosso componente será este:

```vue
<template>
  <div class="painel">
    <h2 class="painel-titulo"></h2>
    <div class="painel-conteudo"></div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss">
.painel {
  padding: 0 auto;
  border: solid 2px grey;
  display: inline-block;
  margin: 5px;
  box-shadow: 5px 5px 10px grey;
  width: 260px;
  height: 100%;
  vertical-align: top;
  text-align: center;

  .painel-titulo {
    text-align: center;
    border: solid 2px;
    background: lightblue;
    margin: 0 0 15px 0;
    padding: 10px;
    text-transform: uppercase;
  }
}
</style>
```

Agora vamos remover tudo que é relativo ao painel do nosso componente principal e deixar somente a chamada de **como** queremos que o painel seja no final:

```vue
<template>
  <div id="app" class="site-body">
    <h1 class="site-title centered">{{ appTitle }}</h1>
    <ul class="image-list">
      <li v-for="(foto, index) of fotos" :key="index" class="image-item">
        <image-panel :title="foto.alt" class="painel-conteudo">
          <img :src="foto.url" :alt="foto.alt" class="image">
        </image-panel>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return { appTitle: "PicLib", fotos: [] }
  },
  created() {
    this.$http
      .get("http://www.splashbase.co/api/v1/images/latest")
      .then(({ data }) => {
        console.log(data)
        const URLs = data.images.map(image => ({
          url: image.url,
          alt: image.id
        }))
        this.fotos = URLs
      })
      .catch(console.error)
  }
}
</script>

<style lang="scss">
.site-body {
  font-family: Arial, Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}

.site-title {
  .centered {
    text-align: center;
  }
}

.image-list {
  list-style: none;

  .image-item {
    display: inline-block;
  }
}

.image {
  width: 100%;
}
</style>
```

### Comunicação entre componentes

Como podemos fazer nosso componente de painel funcionar no nosso `App.vue` e receber um valor chamado `title`. Para isso, dentro do painel, vamos criar uma propriedade `props` dentro do nosso escopo `script`:

```html
<script>
export default {
  props: ["title"]
}
</script>
```

O valor de `props` é um array de nomes de propriedades que podem ser recebidas pelo componente, a partir de agora podemos utilizar como se fosse uma variável padrão, vamos colocá-lo no nosso título:

```html
<template>
  <div class="painel">
    <h2 class="painel-titulo">{{ title }}</h2>
    <div class="painel-conteudo"></div>
  </div>
</template>

<script>
export default {
  props: ["title"]
}
</script>
```

### Incluindo o componente

Agora, vamos para o nosso componente principal `App.vue` e vamos importar nosso componente de painel dentro da tag `<script>`:

```html
<script>
import Painel from "./components/shared/painel/Painel.vue"
export default {
  data() {
    return { appTitle: "PicLib", fotos: [] }
  },
  created() {
    this.$http
      .get("http://www.splashbase.co/api/v1/images/latest")
      .then(({ data }) => {
        console.log(data)
        const URLs = data.images.map(image => ({
          url: image.url,
          alt: image.id
        }))
        this.fotos = URLs
      })
      .catch(console.error)
  }
}
</script>
```

Para podermos utilizar o componente importado, temos que registrar esse novo componente como utilizável, fazemos isso através da chave `components`:

```html
<script>
import Painel from "./components/shared/painel/Painel.vue"
export default {
  components: {
    "image-panel": Painel
  },
  data() {
    return { appTitle: "PicLib", fotos: [] }
  },
  created() {
    this.$http
      .get("http://www.splashbase.co/api/v1/images/latest")
      .then(({ data }) => {
        console.log(data)
        const URLs = data.images.map(image => ({
          url: image.url,
          alt: image.id
        }))
        this.fotos = URLs
      })
      .catch(console.error)
  }
}
</script>
```

> Note que estamos dando um **alias** para o `Painel`, para podermos utilizar como `image-panel`

Ao voltarmos a nossa aplicação, vamos ver que os paineis estão sem conteúdo, mas estão exibindo o título que estamos enviando para ele.

### Slots

Por baixo dos panos, o Vue irá buscar a tag relativa ao `image-panel` e substituirá tudo pelo `template` do componente. Temos que indicar que o componente filho poderá receber outra marcação ou outra tag como conteúdo, no nosso caso, a imagem que vamos colocar dentro dele. Para isso vamos utilizar uma marcação chamada `<slot>` dentro do próprio painel.

Basta substituir a marcação `div` que colocamos o conteúdo por `slot`:

```html
<template>
  <div class="painel">
    <h2 class="painel-titulo">{{ title }}</h2>
    <slot class="painel-conteudo"></slot>
  </div>
</template>
```

Agora o Vue sabe que dentro do nosso componente, nós temos um outro componente ou tag, portanto ela será renderizada.

## Estilos e escopo