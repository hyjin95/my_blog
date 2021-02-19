---
description: 2021.02.19
---

# 교육 - vue.js

### single page application

* 화면에  data를 배치해 놓고 가져오는 data에 따라 html화면이 자동으로 재구성된다.
* UIFramework : vue.js - 따로 프로젝트를 생성 - 또는 스크립트 선언을 통해 사용
* 새로고침없이 data에 따라 화면이 변한다. - &lt;style scope&gt;태그로 css를 컴포넌트마다 적용해 테마를 만들 수 있다.
* 스크립트에 html코드를 최소화 하기 위함

### jsp와의 차이점

* jsp는 화면에 변화를 주기위해서는 반드시 페이지이동\(새로고침\)이 필요하고 ajax를 사용하더라도 직접 html에 접근해 화면을 재구성해 주어야 한다.

### Vue.js : 스크립트

![](../../.gitbook/assets/1%20%28121%29.png)

* JS에서 선언한 vue.js를 보면 JSON형식인 것을 확인할 수 있다.
* el = element

### Vue.js : 프로젝트 생성

![node.js](../../.gitbook/assets/2%20%2896%29.png)

![CMD](../../.gitbook/assets/1%20%28123%29.png)

* node.js LTS파일 다운로드 - CMD에서 'npm -version'으로 설치 확인

![](../../.gitbook/assets/2%20%2898%29.png)

* vue.js install 및 설치 확인 - npm install -g vue-cli - vue --version

![](../../.gitbook/assets/3%20%2873%29.png)

* c 드라이브에 사용할 폴더 생성 및 폴더 접근

![](../../.gitbook/assets/4%20%2851%29.png)

* vue init webpack my-project 입력 - vue 프로젝트 생성 설정
* install이 끝나면 서버가 열리고 localhost를 보여준다. CMD에서 서버를 연 것이므로 CMD를 닫으면 서버도 닫혀버리니 주의

![](../../.gitbook/assets/5%20%2837%29.png)

* 확인

![](../../.gitbook/assets/.png%20%2855%29.png)

* Atom 에서 File &gt; Open File &gt; 만든 프로젝트를 열면 위와같은 기본 구성을 확인할 수 있다. 

### 기본 소스 분석

```markup
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>myproject</title>
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

* index.html
* div id값이 app인 것을 확인할 수 있다.

```javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({//json형태
  el: '#app',
  router,//페이지 정보, url정보를 담고 있음
  components: { App },//컴포넌트로 등록하면 template로 호출해 태그로 사용할 수 있다.
  template: '<App/>'//app태그 작성 = app를 적용시키겠다 = App.vue
})
```

* main.js
* 11번 : Vue에 elment로 html의 app id를 갖는 div를 가리키고 있다.
* 12 번 : router 페이지 정보, url정보를 갖는다.
* 13 번 : App라는 vue문서를 componet에 등록한다.
* 14번 : component에 등록된 vue중 App라는 vue문서를 tamplate로 지정한다.

```markup
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

* App.vue
* 9번 : export default - 다른 파일에 있는 양식을 참조해오기 위한 방식

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

Vue.use(Router)//vue에서 해당 router를 사용하겠다.

export default new Router({//url과 파일 지정.
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})
```

* index.js
* 여기서 router를 통해 vue를 등록하고 path를 지정할 수 있다.

### components : vue 만들기

```markup
<template>
  <div>
    {{ msg }}
  </div>
</template>

<script>
export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
    }
  }
}
</script>
```

* componets &gt; vue파일 생성 - Template에 있는 것들만 화면에 출력된다.
* 3번과 12번 은 n 개 작성 할 수 있다.

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import vueHello from '@/components/vueHello'

Vue.use(Router)//vue에서 해당 router를 사용하겠다.

export default new Router({//url과 파일 지정.
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/vueHello',
      name: 'vueHello',
      component: vueHello
    }
  ]
})
```

* index.js
* index.js 에 컴포넌트\(vue파일\) 등록
* router에 사용할 path 등록

### router-link : 링크 만들기

```markup
<template>
  <div id="app">
    <img src="./assets/logo.png">
    
    <router-link to="/"> HelloWorld </router-link>
    <router-link to="vueHello"> vueHello </router-link>
    
    <router-view/>
  </div>
</template>
```

* App.vue
* &lt;router-link&gt;는 html의 &lt;a&gt;태그와 같다.

![](../../.gitbook/assets/6%20%2825%29.png)

* 확인

### 등록과 출력 : v-bind

```markup
<template>
  <div>
    {{ msg }}
    {{ test }}
    <span v-bind:title="msg">
      타이틀이 보입니다.
    </span>
  </div>
</template>

<script>
export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
      ,test: 'test123'
    }
  }
}
</script>
```

* v-bind: 로 속성을 작성하면 등록한 data를 bind해 가져와 사용할 수 있다.

![](../../.gitbook/assets/5%20%2836%29.png)

* 출력
* 마우스를 올려 title값 확인

### if, 제어문 : v-if, v-if-else, v-else

```markup
<template>
  <div>
    {{ msg }}
    {{ test }}
    <span v-if="seen" v-bind:title="msg">
      타이틀이 보입니다.
    </span>
    <span v-else v-bind:title="msg">
      else가 보입니다.
    </span>
  </div>
</template>
```

* v-if
* seen이라는 data가 정의되어있지 않기때문에 if문의 결과는 false이다.
* 해당 &lt;span&gt;태그는 보이지 않게된다.

![](../../.gitbook/assets/2%20%2897%29.png)

* 확인

```markup
<template>
  <div>
    {{ msg }}
    {{ test }}
    <span v-if="seen == 0" v-bind:title="msg">
      타이틀이 보입니다.
    </span>
    <span v-else-if="seen == 2" v-bind:title="msg">
      else if 가 보입니다.
    </span>
    <span v-else v-bind:title="msg">
      else 가 보입니다.
    </span>
  </div>
</template>

<script>
export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
      ,test: 'test123'
      ,seen: 2
    }
  }
}
</script>
```

* v-else
* v-else-if

![](../../.gitbook/assets/3%20%2872%29.png)

*  확인

### 반복문 v-for

```markup
<script>
export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
      ,test: 'test123'
      ,seen: 2
      ,todos : [
        {text:"testText1"}
        ,{text:"testText2"}
        ,{text:"testText3"}
      ]
    }
  }
}
</script>
```

* 배열 data가 필요하다.

```markup
<template>
  <div>
    <ol>
      <li v-for="todo in todos"> {{ todo.text }} </li>
    </ol>
  </div>
</template>
```

* template에 추가
* todo는 todos의 알리아스명

![](../../.gitbook/assets/4%20%2852%29.png)

* 출력

### Button과 함수\(method\) 선언

```markup
<template>
  <div>

    <button v-on:click="dataChange"> 데이터 변경</button>

  </div>
</template>
```

* template에 &lt;button&gt;선언
* v-on으로 클릭에 대한 메서드 지정
* 괄호가 없지만 파라미터로 값을 넘겨야 하는 함수라면 괄호를 작성한다.

```markup
<script>
export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
      ,test: 'test123'
      ,seen: 2
      ,todos : [
        {text:"testText1"}
        ,{text:"testText2"}
        ,{text:"testText3"}
      ]
    }
  }
  ,methods:{
    dataChange : function(){
      this.todos = [
        {text:"testText11"}
        ,{text:"testText22"}
        ,{text:"testText33"}
        ,{text:"testText44"}
        ,{text:"testText55"} 
      ]
    }
  }
}
</script>
```

* methods 속성에서 함수 선언이 가능하다.
* data에 접근할 수 있다.

![](../../.gitbook/assets/1%20%28122%29.png)

* 버튼을 누른 화면
* 확인

### 컴포넌트 등록과 태그 사용

* vue에서는 컴포넌트를 직접 정의해 사용한다.
* script에서 컴포넌트를 정의하면 Template\(html\)에서 태그 형식으로 호출해 사용할 수 있다. - 화면단과 기능단의 적극적인 분리가 가능하다. - html코드의 재사용성이 높아진다.

```markup
<template>
  <div>

    <li v-for="todo in todos"> {{ todo.text }} </li>

  </div>
</template>

<script>
export default {
  name: 'testComp'
  ,props:['todos']

}
</script>

```

* testComp.vue 문서 생성
* v-bind가 없으면 item문자열이 전달되고 있으면 data가 전달된다.

```markup
<template>
  <div>

    <testComp v-bind:todos="todos"></testComp>

  </div>
</template>

<script>

import testComp from '@/components/testComp'

export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
      ,test: 'test123'
      ,seen: 2
      ,todos : [
        {text:"testText1"}
        ,{text:"testText2"}
        ,{text:"testText3"}
      ]
    }
  }
  ,methods:{
    dataChange : function(){
      this.todos = [
        {text:"testText11"}
        ,{text:"testText22"}
        ,{text:"testText33"}
        ,{text:"testText44"}
        ,{text:"testText55"}
      ]
    }
  }
  ,components:{testComp}
}
</script>
```

* vueHello.vue 
* script에 testComp.vue를 import한다. export default와 붙어있으면 import가 정상적으로 되지 않는다. 한 줄 띄우기
* export default에 components속성을 작성하고 testComp를 등록한다.
* 4번 : 화면에서 태그로 호출해 출력한다.

![](../../.gitbook/assets/2%20%2895%29.png)

* 확인
* 데이터 변경 버튼을 누르면 데이터 두군데 모두 이벤트가 적용된다.

### Grid와 dataSet

```markup
<template>
  <div>

    <table border=1 align="center">
      <tr>
        <th v-for="col in dataset.column">
          {{ col }}
        </th>
      </tr>
      <tr v-for="row in dataset.rows">
        <td v-for="data in row">{{ data }}</td>
      </tr>
    </table>

  </div>
</template>

<script>
export default {
  name: 'grid'
  ,props:['dataset']

}
</script>
```

* grid.vue 생성

```markup
<template>
  <div>

    <grid v-bind:dataset="dataset"></grid>

  </div>
</template>

<script>

import testComp from '@/components/testComp'
import grid from '@/components/grid'

export default {
  name: 'vueHello',
  data () {
    return {
      msg: 'vueHello'
      ,test: 'test123'
      ,seen: 2
      ,todos : [
        {text:"testText1"}
        ,{text:"testText2"}
        ,{text:"testText3"}
      ]
      ,dataset:{
        column : ['type','no','title']
        ,rows:[
           {type : '111', no : '1', title : 'textTitle111'}
          ,{type : '222', no : '2', title : 'textTitle222'}
          ,{type : '333', no : '3', title : 'textTitle333'}
        ]
      }
    }
  }
  ,components:{testComp, grid}
}
</script>
```

* vueHello.vue

![](../../.gitbook/assets/3%20%2874%29.png)

* 확인

```markup
<template>
  <div>

    <table border=1 align="center">
      <tr>
        <th v-for="col in dataset.column">
          {{ col }}
        </th>
      </tr>
      <tr v-for="row in dataset.rows">
        <template v-for="col in dataset.column">
          <template v-for="(value, key) in row"">
            <td v-if="col == key">
              {{value}}
            </td>
          </template>
        </template>
      </tr>
    </table>

  </div>
</template>

<script>
export default {
  name: 'grid'
  ,props:['dataset']
}
</script>
```

* 컬럼명 수정이 가능하게 끔 컬럼명과 row를 매칭시킨다.
* &lt;template&gt;태그를 사용해 화면에는 출력되지 않지만 적용되도록 할 수 있다. - 이중 for문 을 사용할때 사용된다.

