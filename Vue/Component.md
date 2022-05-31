# 컴포넌트 기초 (Component)

### 컴포넌트 등록하기 
컴포넌트 이름은 kebab-case 또는 PascalCase로 정의한다. 

### props 
특정 유형의 값으로 사용될 수 있으며, 속성의 이름과 값에 각각 prop 이름과 타입이 포함된 객체로 prop를 나열할 수 있다.

```
// app.vue

<template>
  <MyBtn>Banana</MyBtn> // 기본
  <MyBtn
    :color="color">
    <span style="color: red;">Banana</span> // 검은 배경 빨간 글씨
  </MyBtn>
  <MyBtn
    large
    color="royalblue"> // 큰 버튼 파란 배경
    Apple
  </MyBtn>
  <MyBtn>Cherry</MyBtn> // 기본
  <button>Banana</button> // 기본 button
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  },
  data() {
    return {
      color: '#000'
    }
  }
}
</script>

//mybtn

<template>
  <div
    :class="{ large }"
    :style="{ backgroundColor: color}"
    class="btn">
    <slot></slot> // 내용
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'gray'
    },
    large: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
    &.large {
      font-size: 20px;
      padding: 10px 20px;
    }
  }
</style>
```

#### 속성 상속 
부모 컴포넌트의 html 요소에 class 속성으로 값을 입력한 경우, 자식 컴포넌트의 btn이라는 클래스에 상속이 된다. <br/>
inheritAttrs: false 를 통해 상속을 방지하고 $refs 를 통해 개별 설정할 수 있다.

<br/>

```
<template>
  <MyBtn
    class="heropy"
    style="color: red;"
    title="Hello world!">
    Banana
  </MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  }
}
</script>

// mybtn

<template>
  <div class="btn"> // 상속 X
    <slot></slot>
  </div>
  <!-- <h1
    :class="$attrs.class"
    :style="$attrs.style"></h1> // 밑이랑 동일 --> 
  <h1 v-bind="$attrs"></h1> // class="heropy", style="color: red" 상속 개별 설정
</template>

<script>
export default {
  inheritAttrs: false, // 상속 되지 않게 설정
  created() {
    console.log(this.$attrs) //proxy
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
  }
</style>
```

<br/>
<br/>

### emit 

emit은 다른 Component에게 현재 Component의 Event나 Data를 전달하기 위해 사용할 수 있다.

```
// App.vue

<template>
  <MyBtn
    @heropy="log"
    @change-msg="logMsg"> // kebabcase 변환
    Banana
  </MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  },
  methods: {
    log(event) {
      console.log('Click!!')
      console.log(event)
    },
    logMsg(msg) {
      console.log(msg)
    }
  }
}
</script>

//MyBtn.vue

<template>
  <div class="btn">
    <slot></slot>
  </div>
  <h1 @dblclick="$emit('heropy', $event)"> // app.vue 로 전송
    ABC
  </h1>
  <input
    type="text"
    v-model="msg" />
</template>

<script>
export default {
  emits: [
    'heropy',
    'changeMsg'
  ],
  data() {
    return {
      msg: ''
    }
  },
  watch: {
    msg() {
      this.$emit('changeMsg',this.msg) //app.vue 로 전송
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
  }
</style>
```

<br/>
<br/>

### slot
slot은 부모 컴포넌트에서 자식 컴포넌트의 엘리먼트를 지정할때 사용한다. <br/>
부모에 따라서 자식의 컴포넌트에 영향을 받을 테니, 컴포넌트 재사용성면에서 좋은 장점을 가진다.

```
<template>
  <MyBtn>
    <template #icon>
      <span>(B)</span>
    </template>
    <template #text>
      <span>Banana</span>
    </template>
  </MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  }
}
</script>

//MyBtn.vue
<template>
  <div class="btn">
    <slot name="icon"></slot> // (B)
    <slot name="text"></slot> // Banana
  </div>
</template>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
  }
</style>
```

<br/>
<br/>

### provide, inject 
기본적으로는 Props처럼 부모에서 자식 컴포넌트로의 데이터 전달을 위한 기능이다. <br/>
<b> 반응형을 지원하지 않는다. </b>

```
<template>
  <button @click="message = 'Good?' ">
    Click!
  </button>
  <h1>App: {{ message }}</h1> // App: Hello world!
  <Parent />
  <!-- <Parent :msg="message" /> -->
</template>

<script>
import Parent from '~/components/Parent'
import { computed } from 'vue'

export default {
  components: {
    Parent
  },
  data() {
    return {
      message: 'Hello world!'
    }
  },
  provide() {
    return {
      // msg: this.message
      msg: computed(() => {
        return this.message
      })
    }
  }
}
</script>

//parent.vue

<template>
  <Child />
  <!-- <Child :msg="msg" /> -->
</template>

<script>
import Child from '~/components/Child'

export default {
  components: {
    Child
  }
  // props: {
  //   msg: {
  //     type: String,
  //     default: ''
  //   }
  // }
}
</script>

//child.vue

<template>
  <div>
    Child: {{ msg }} // Child Hello World! // 화면으로 출력
    Child: {{ msg.value }} // Good!
  </div>
</template>

<script>
export default {
  inject: ['msg']
  // props: {
  //   msg: {
  //     type: String,
  //     default: ""
  //   }
  // }
}
</script>
```

<br/>
<br/>

### Refs 

Refs를 통해 자식 엘리먼트 데이터에 접근할 수 있다. <br/>
<b>접근, 가져오기까지만 가능하며 다른 역할은 수행할 수 없다. </b>

```
<template>
  <Hello ref="hello" /> //hello라는 이름으로 ref (참조)하겠다
</template>

<script>
import Hello from '~/components/Hello'

export default {
  components: {
    Hello
  },
  mounted() {
    console.log(this.$refs.hello.$refs.good) // <h1>Hello</h1>
  }
}
</script>

// Hello.vue

<template>
  <h1>Hello~</h1> // 화면 출력
  <h1 ref="good"> // 화면 출력
    Good?
  </h1>
</template>

```

<br/>
<br/>


TMI : 컴포넌트는 아직 많은 공부가 필요할 것 같다.. 😭
<hr/>

**Reference**

<ul>
  <li><a href="https://v3.ko.vuejs.org/guide/component-registration.html">VUE.JS</a></li>
</ul>
