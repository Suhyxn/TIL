# 클래스(Class)와 스타일 바인딩 (Style Binding)

<br/>
<br/>

## HTML 스타일 바인딩 
<hr/>
</br>

### 객체 구문
v-bind:class (v-bind:class의 약자)에 객체를 전달하여 클래스를 동적으로 전환할 수 있다.
```
<div :class="{ active: isActive }"></div>
```
위 구문은 active 클래스의 존재가 데이터 속성 isActive의 진실성(Truthiness) 에 의해 결정됨을 의미한다.

<br/>

여러 클래스 전환도 가능하다.
```
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>

data() {
  return {
    isActive: true,
    hasError: false
  }
}

>>

<div class="static active"></div>

```

<br/>

다른 예제 2
```
<div :class="classObject"></div>

data() {
  return {
    classObject: {
      active: true,
      'text-danger': false
    }
  }
}
```
<br/>

다른 예제 3
```
<div :class="classObject"></div>

data() {
  return {
    isActive: true,
    error: null
  }
},
computed: {
  classObject() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

<br/>
<br/>

### 배열 구문 

배열을 :class에 전달하여 클래스 목록을 적용할 수 있다.

```
<div :class="[activeClass, errorClass]"></div>

data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}

>>

<div class="active text-danger"></div>
```
<br/>

조건부로 목록의 클래스도 삼항 표현식을 사용하여 수행할 수 있다.
```
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```
항상 errorClass를 적용하지만 isActive가 true 인 경우 activeClass만 적용합니다.

동일한 결과에 다른 코드
```
<div :class="[{ active: isActive }, errorClass]"></div>
```

<br/>
<br/>

## 인라인 스타일
<hr/>
<br/>

### 객체 구문

:style로 HTML과 유사하게 작성하며 CSS 속성 이름에는 카멜 케이스(camelCase) 또는 케밥 케이스(kebab-case, 케밥 케이스와 함께 <b>따옴표</b> 사용)를 사용할 수 있다.

``` 
<template>
  <h1
    :style="{
      color,fontSize // 직접 :style{" color:... }로 정의하기보단 data() {...}를 활용하자
    }"
    @click="changeStyle">
    Hello?!
  </h1>
</template>

<script>
export default {
  data () {
    return {
      color: 'orange',
      fontSize: '30px'
    }
  },
  methods: {
    changeStyle() {
      this.color = 'red'
      this.fontSize = '50px'
    }
  }
}
</script>
```

<br/>
<br/>

### 배열 구문 
:style의 배열 구문을 사용하면, 동일한 요소에 여러 스타일 객체를 적용 할 수 있다.
```
<template>
  <h1
    :style="[fontStyle, backgroundStyle]"
    @click="changeStyle">
    Hello?!
  </h1>
</template>

<script>
export default {
  data () {
    return {
      fontStyle: {
        color: 'orange',
        fontSize: '30px'
      },
      backgroundStyle: {
        backgroundColor: 'black'
      }
    }
  },
  methods: {
    changeStyle() {
      this.fontStyle.color = 'red'
      this.fontStyle.fontSize = '50px'
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
  <li><a href="https://v3.ko.vuejs.org/guide/class-and-style.html#html-%E1%84%8F%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A2%E1%84%89%E1%85%B3-%E1%84%87%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%83%E1%85%B5%E1%86%BC">VUE.JS</a></li>
</ul>