# 조건부 랜더링 (Conditional Rendering)

### v-if
조건에 따라 블록을 렌더링할 때 사용한다. <br/>
블록은 디렉티브의 표현식이 true 값을 반환할 때만 렌더링 된다. <br/>

<br/>
v-else, v-else-if, template과 같이 사용할 수 있다. 

```
<template>
  <button @click="handler"> // 누를 때마다 count 1 증가
    Click me!
  </button>
  <h1 v-if="isShow"> // isShow=true시 출력
    Hello?!
  </h1>
  <h1 v-else-if="count > 3"> // count 3이상시 isShow=false시 출력
    Count > 3
  </h1>
  <h1 v-else> // count 3까지 isShow=false시 출력
    Good~
  </h1>
</template>

<script>
export default {
  data() {
    return {
      isShow: true,
      count: 0
    }
  },
  methods: {
    handler() {
      this.isShow = !this.isShow
      this.count += 1
    }
  }
}
</script>
```
template 사용
```
  <template>
  <button @click="handler">
    Click me!
  </button>
  <template v-if="isShow"> // 둘 이상의 엘리먼트를 전환할 때 이용하며, template은 눈에 보이지 않는 감싸는 역할을 한다.
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
  </template>
</template>

<script>
export default {
  data() {
    return {
      isShow: true,
      count: 0
    }
  },
  methods: {
    handler() {
      this.isShow = !this.isShow
      this.count += 1
    }
  }
}
</script>
```

<br/>
<br/>

### v-show
조건에 따라 블록을 렌더링할 때 사용하며, 단순히 엘리먼트의 CSS display 속성만을 전환한다.

```
<template>
  <button @click="handler">
    Click me!
  </button>
  <h1 v-show="isShow"> // display:none
    Hello?!
  </h1>
</template>

<script>
export default {
  data() {
    return {
      isShow: false,
      count: 0
    }
  },
  methods: {
    handler() {
      this.isShow = !this.isShow
      this.count += 1
    }
  }
}
</script>
```

<br/>
<br/>

## v-if 와 v-show 의 차이점

<ul>
<li>
v-if는 게으르(lazy) 다고 한다.
조건이 거짓(false)일 경우 아무 작업도 하지 않으며 처음으로 참(true)이 될 때까지 렌더링하지 않는다.
</li> <br/>

<li>
v-show는 CSS 기반 전환으로 초기 조건과 관계 없이 항상 렌더링된다.
조건기 거짓(false)일 경우 CSS Display:none 속성을 통해 전환한다.
</li> <br/>

<li>
일반적으로 v-if는 전환 비용이 높은 반면, v-show는 초기 렌더링 비용이 높다. 그러므로 무언가를 자주 전환해야 한다면 v-show를 사용하는 게 좋고, 런타임 시 조건이 변경되지 않는다면 v-if를 사용하는 게 더 낫다.
</li> <br/>

<li>
두 조건 중 우선 순위는 v-if가 더 높은 우선순위를 갖는다.
</li>
</ul>

<br/>
<br/>

<hr/>

**Reference**

<ul>
  <li><a href="https://v3.ko.vuejs.org/guide/conditional.html">VUE.JS</a></li>
</ul>