# 템플릿 문법

### 보간법

Mustache”(이중 중괄호 구문)기법을 사용한다.
```
<span>메시지: {{ msg }}</span>
```

msg 속성 값으로 대체되며 또한 msg 속성이 변경될 때 마다 갱신된다.
```
<template>
  <h1 
  v-once 
  @click="add"> 
    {{ msg }} // Hello world!가 출력되며 v-once를 제거하면 Hello world를 클릭 할 때 ! 추가된다.
  </h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  },
  methods: {
    add() {
      this.msg += '!'
    }
  }
}
</script>
```
v-once를 통해서 일회성 보간을 수행할 수 있다.

<br/>
<br/>

### 원시 HTML

이중 중괄호는 데이터를 HTML이 아닌 일반 텍스트로 해석합니다. 실제 HTML을 출력하려면 v-html 디렉티브를 사용해야 한다.

```
<template>
  <h1
    v-once
    @click="add">
    {{ msg }}
  </h1> // <div style="color: red;">Hello</div>
  <h1 v-html="msg"></h1> // Hello! 
</template>

<script>
export default {
  data() {
    return {
      msg: '<div style="color: red;">Hello!!</div>'
    }
  },
  methods: {
    add() {
      this.msg += '!'
    }
  }
}
</script>
```

<br/>
<br/>

### 속성 

Mustaches(이중 중괄호 구문)는 HTML 속성에 사용할 수 없다. <br/>
대신 v-bind 디렉티브를 사용한다.

```
<template>
  <h1 :class="msg"> // v-bind 약어 사용 : , v-on은 @
    {{ msg }}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'active'
    }
  },
  methods: {
    add() {
      this.msg += '!'
    }
  }
}
</script>

<style scoped>
  .active {
    color: royalblue;
    font-size: 100px;
  }
</style>
```

활용 예제
```
<template>
  <h1 
    :[attr]="'active'"
    @[event]="add">
    {{ msg }}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'active',
      attr: 'class',
      event: 'click'
    }
  },
  methods: {
    add() {
      this.msg += '!'
    }
  }
}
</script>

<style scoped>
  .active {
    color: royalblue;
    font-size: 100px;
  }
</style>
```

<br/>
<br/>

<hr/>

**Reference**

<ul>
  <li><a href="https://v3.ko.vuejs.org/guide/template-syntax.html">VUE.JS</a></li>
</ul>