# Computed

### Computed 

coputed (계산된데이터)
data 옵션에 정의해둔 특정한 데이터를 추가적으로 어떤 연산을 통해 정의한 다음에 정의한 값을 반환하는 계산된 데이터 <br/>
<br/>
data들을 정의한 후 원본에 손상이 가지 않도록 computed를 통해 계산할 수 있으며 캐싱 기능을 통해 간단한 로직은 더 부담없이 활용할 수 있다.

```
<template>
  <h1>{{ reversedMessage }}</h1> / !detupmoC olleH
  <h1>{{ reversedMessage }}</h1> / !detupmoC olleH
  <h1>{{ reversedMessage }}</h1> / !detupmoC olleH
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello Computed!'
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    }
  },
  methods: { // coumputed를 이용하면 불필요
    reverseMessage() {
      return this.msg.split('').reverse().join('')
    }
  }
}
</script>
```

### Setter

Computed 속성의 Setter
<br/>
Computed 속성은 기본적으로 getter 이지만, 필요할 때엔 setter 도 제공할 수 있다.

```
<template>
  <button @click="add">
    ADD
  </button>
  <h1>{{ reversedMessage }}</h1> // ?!Hello Computed!
  <h1>{{ reversedMessage }}</h1> // ?!Hello Computed!
  <h1>{{ reversedMessage }}</h1> // ?!Hello Computed!
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello Computed!' 
    }
  },
  computed: {
    // Getter
    // reversedMessage() {
    //   return this.msg.split('').reverse().join('')
    // }
    // Getter, Setter
    reversedMessage: {
      get() {
        return this.msg.split('').reverse().join('')
      },
      set(newValue) {
        this.msg = newValue
      }
    }
  },
  methods: { 
    add() {
      this.reversedMessage += '!?'
    }
  }
}
</script>
```

### Watch
감시할 데이터를 지정하고 그 데이터가 바뀌면 선언한 함수를 실행하라는 방식으로 명령형 프로그래밍이다. <br/>
=> 차이점 : computed속성은 계산해야 하는 목표 데이터를 정의하는 방식으로 선언형 프로그래밍이다. <br/>
<br/>
데이터를 업데이트 할 때 비동기처리나 무거운 처리(많은 처리)를 실행하고 싶은 경우 편리하다


```
<template>
  <h1 @click="changeMessage">
    {{ msg }} // click 시 Good!
  </h1>
  <h1>{{ reversedMessage }}</h1> // click 시 !dooG
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello?'
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    }
  },
  watch: {
    msg(value) {
      console.log('msg: ', this.msg) // msg: Good!
    },
    reversedMessage(value) {
      console.log('reversedMessage: ', value) // reversedMessage: !dooG
    }
  },
  methods: {
    changeMessage() {
      this.msg = 'Good!'
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
  <li><a href="https://v3.ko.vuejs.org/guide/computed.html#computed-%E1%84%89%E1%85%A9%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC">VUE.JS</a></li>
</ul>