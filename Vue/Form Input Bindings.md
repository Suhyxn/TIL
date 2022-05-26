# 폼 입력 바인딩 (Form Input Bindings)

### v-model
``` v-model ``` 디렉티브를 사용하여 input, textarea, select 요소들에 양방향 데이터 바인딩을 생성할 수 있다. 
v-model 디렉티브는 input type 요소를 변경하는 올바른 방법을 자동으로 선택한다. </br>

v-model은 내부적으로 서로 다른 속성을 사용하고 서로 다른 입력 요소에 대해 서로 다른 이벤트를 전송한다.

<ul>
<li> text 와 textarea 태그는 value속성과 input이벤트를 사용한다. </li>
<li> 체크박스들과 라디오버튼들은 checked 속성과 change 이벤트를 사용한다. </li>
<li> Select 태그는 value 를 prop으로, change를 이벤트로 사용한다. </li>
</ul>

</br>

<li>checkbox</li>

```
<template>
  <h1>{{ msg }}</h1> // 변경된 msg 실시간 출력
  <input
    type="text"
    v-model="msg" /> // 실시간 입력한 데이터로 msg 교체
  <h1>{{ checked }}</h1>  // false 클릭시 true 출력
  <input
    type="checkbox"
    v-model="checked" />  // false 클릭시 true 
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!', // msg 초기값
      checked: false // checked 초기값
    }
  }
}
</script>
```

</br>

> IME (opens new window)(중국어, 일본어, 한국어 등)가 필요한 언어의 경우 IME 중 v-model이 업데이트 되지 않습니다.  

```
// 한글 사용시

<template>
  <h1>{{ msg }}</h1>
  <input
    type="text"
    :value="msg"
    @input="msg = $event.target.value" />
  <h1>{{ checked }}</h1>
  <input
    type="checkbox"
    v-model="checked" />
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!',
      checked: false
    }
  }
}
</script>
```

</br>
</br>

### 값 바인딩하기 
활성 인스턴스의 동적 속성에 값을 바인딩하려고 할 때, v-bind를 사용할 수 있다. 또한, v-bind를 사용하면 입력 값을 문자열이 아닌 값에 바인딩 할 수 있다.

</br>

<li>checkbox</li>

```
<!-- `toggle` 는 true 또는 false 입니다 -->

<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" />

// 체크된 경우:
 vm.toggle === 'yes'
// 체크되지 않은 경우:
 vm.toggle === 'no'
```

</br>
<li>radio</li>

```
<!-- `picked` 는 선택시 문자열 "a" 입니다 -->

<input type="radio" v-model="pick" v-bind:value="a" />

// 체크된 경우:
vm.pick === vm.a
```

</br>
<li>select</li>

```
<!-- `selected` 는 선택시 "123" 입니다 -->

<select v-model="selected">
  <!-- 인라인 객체 리터럴 -->
  <option :value="{ number: 123 }">123</option>
</select>

// 선택된 경우:
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

</br>
</br>

### 수식어 

</br>
<li>.lazy</li>

```
//엔터나 다른 부분을 클릭해야 데이터가 동기화 된다
<template>
  <h1>{{ msg }}</h1>
  <input
    type="text"
    v-model.lazy="msg" /> // Hello world! 적어도 바로 갱신되지 않고 엔터나 다른 부분을 클릭하여야 작성한 데이터로 갱신
  <!-- <input
    type="text"
    :value="msg"
    @change="msg = $event.target.value" /> --> // 동일한 효과
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```

</br>
<li>.number</li>

```
// 인풋 요소 타입은 문자열로 반환된다. 숫자 데이터로 지속되어야 된다면 .number를 사용하자

<template>
  <h1>{{ msg }}</h1>
  <input
    type="text"
    v-model.number="msg" /> // .number이 없다면 string
</template>

<script>
export default {
  data() {
    return {
      msg: '123'
    }
  },
  watch: {
    msg() {
      console.log(typeof this.msg) //number
    }
  }
}
</script>
```

</br>
<li>.trim</li>

```
// 앞 뒤 공백을 제거 

<template>
  <h1>{{ msg }}</h1>
  <input
    type="text"
    v-model.trim="msg" />
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  },
  watch: {
    msg() {
      console.log(this.msg)
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
  <li><a href="https://v3.ko.vuejs.org/guide/forms.html#%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8">VUE.JS</a></li>
</ul>