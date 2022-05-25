# 이벤트 핸들링 (Event Handling)

### v-on:click="methodName" = @click="methodName"
v-on디렉티브는 @기호로, DOM 이벤트를 듣고 특정한 반응에 동작할 때와 JavaScript를 실행할 때 사용한다.

```
<template>
  <div id="basic-event">
    <button @click="counter += 1">Add 1</button> // 버튼 클릭시  counter + 1 추가
    <p>The button above has been clicked {{ counter }} times.</p> 
  </div>
</template>

<script>
  Vue.createApp({
    data() {
      return {
        counter: 1
      }
    }
  }).mount('#basic-event')
</script>
```

<br/>
<br/>

### 메소드 이벤트 핸들러

속성 값으로 메소드의 이름을 받는다.

```
<template>
  <button @click="handler"> // 버튼 클릭시 handlder 실행
    Click me!
  </button>
</template>

<script>
export default {
  methods: {
    handler(event) { 
      console.log(event) 
    }
  }
}
</script>
```

<br/>
<br/>

### 인라인 메소드 핸들러
원본 DOM 이벤트에 액세스 해야할 때 $event를 통해 전달할 수도 있다.

```
<template>
  <button @click="handler('h1', $event)">
    Click 1
  </button>
  <button @click="handler('what', $event)">
    Click 2
  </button>
</template>

<script>
export default {
  methods: {
    handler(msg, event) {
      console.log(msg) // h1, what
      console.log(event) // PointerEvent {isTrusted: true, pointerId: 1 ... ,
    }
  }
}
</script>
```

<br/>
<br/>

### 복합 이벤트 핸들러 
여러 효과를 사용할 때는 생략했던 () 을 추가해 줘야 한다.

```
<template>
  <button @click="handlerA(), handlerB()"> // () 생략 X
    Click me!
  </button>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A') // 버튼 클릭시 A와 B 동시 발생
    },
    handlerB() {
      console.log('B') // 버튼 클릭시 A와 B 동시 발생
    }
  }
}
</script>
```

<br/>
<br/>

## 이벤트 수식어
DOM 이벤트 세부 사항을 처리하는 대신 데이터 로직에 대한 메소드만 사용할 수 있으면 더 좋다. <br/>
(Ex : event.preventDefault(), event.stopPropagation)
이 문제를 해결하기 위하여, Vue는 v-on이벤트에 이벤트 수식어를 제공한다. <br/>
수식어는 점으로 된 접미사이다.<br/>

<br/>

<ul>
<li>.stop</li>
<li>.prevent</li>
<li>.capture</li>
<li>.self</li>
<li>.once</li>
<li>.passive</li>
</ul>

<br/>

```
<template>
  <a
    href="http://naver.com"
    target="_blank"
    @click.prevent.once="handler"> // 두 번 누른 순간부터 페이지가 열리고 'ABC'는 출력되지 않는다.
    NAVER
  </a>
</template>

<script>
export default {
  methods: {
    handler() {
      console.log('ABC!')
    }
  }
}
</script>
```
<br/>
<li> 버블링 방지 </li>

```
// 버블링 방지

<template>
  <div
    class="parent"
    @click="handlerA">
    <div
      class="child"
      @click.stop="handlerB"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    }
  }
}
</script>

<style scoped lang="scss">
  .parent {
    width: 200px;
    height: 100px;
    background-color: royalblue;
    margin: 10px;
    padding: 10px;
    .child {
      width: 100px;
      height: 100px;
      background-color: orange;
    }
  }
</style>
```

<br/>
<li>캡처링 방지</li>

```
// 캡처링 방지

<template>
  <div
    class="parent"
    @click.capture.stop="handlerA">
    <div
      class="child"
      @click="handlerB"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    }
  }
}
</script>

<style scoped lang="scss">
  .parent {
    width: 200px;
    height: 100px;
    background-color: royalblue;
    margin: 10px;
    padding: 10px;
    .child {
      width: 100px;
      height: 100px;
      background-color: orange;
    }
  }
</style>
```

<br/>
<li>정확하게 클릭해야 발생</li>

```
// 정확하게 클릭해야 발생
<template>
  <div
    class="parent"
    @click.self="handlerA">
    <div
      class="child"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    }
  }
}
</script>

<style scoped lang="scss">
  .parent {
    width: 200px;
    height: 100px;
    background-color: royalblue;
    margin: 10px;
    padding: 10px;
    .child {
      width: 100px;
      height: 100px;
      background-color: orange;
    }
  }
</style>
```

<br/>
<li>로직과 화면 분리로 성능 향상</li>

```
//로직과 화면 분리 성능 향상
<template>
  <div
    class="parent"
    @wheel.passive="handler"> // 로직과 화면의 스크롤을 분리
    <div class="child"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handler(event) {
      for (let i = 0; i < 1000; i += 1) {
        console.log(event)
      }
      console.log(event)
    }
  }
}
</script>

<style scoped lang="scss">
  .parent {
    width: 200px;
    height: 100px;
    background-color: royalblue;
    margin: 10px;
    padding: 10px;
    overflow: auto;
    .child {
      width: 100px;
      height: 2000px;
      background-color: orange;
    }
  }
</style>
```

<br/>
<br/>

### 키 수식어
키보드 이벤트를 청취할 때, 종종 공동 키 코드를 확인해야 한다.

<br/>
<li>Enter</li>

```
// enter

<template>
  <input
    type="text"
    @keydown.enter="handler" />
</template>

<script>
export default {
  methods: {
    handler() {
      console.log('Enter!!')
    }
  }
}
</script>
```
<br/>
<li>Ctrl+Shift+A</li>

```
// ctrl+shift+a

<template>
  <input
    type="text"
    @keydown.ctrl.shift.a="handler" />
</template>

<script>
export default {
  methods: {
    handler() {
      console.log('ctrl+shift+a!!')
    }
  }
}
</script>
```

<br/>
<br/>

<hr/>

**Reference**

<ul>
  <li><a href="https://v3.ko.vuejs.org/guide/events.html#%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%8E%E1%85%A5%E1%86%BC%E1%84%8E%E1%85%B1">VUE.JS</a></li>
</ul>