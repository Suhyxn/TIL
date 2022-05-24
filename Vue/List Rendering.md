# 리스트 랜더링 (List Rendering)

### v-for
디렉티브를 사용하여 배열을 기반으로 리스트를 렌더링 할 수 있다. <br/>
v-for 디렉티브는 item in items 형태로 특별한 문법이 필요하다. <br/>
여기서 items는 원본 데이터 배열이고 item은 반복되는 배열 엘리먼트의 별칭이다. <br/>

```
<template>
  <button @click="handler">
    Click me! // 클릭시 Orange-dEpTJAFq3N 추가
  </button>
  <ul>
    <li
      v-for="fruit in newFruits" 
      :key="fruit.id"> // 상태 유지
      {{ fruit.name }}-{{ fruit.id }} // Apple-4llsnjkP3, Banana-...
    </li>
  </ul>
</template>

<script>
import shortid from 'shortid'  

export default {
  data() {
    return {
      fruits: [ 'Apple', 'Banana', 'Cherry']
    }
  },
  computed: {
    newFruits() {
      return this.fruits.map(fruit => ({ // 객체 속성 반복
          id: shortid.generate(),
          name: fruit
      }))
    }
  },
  methods: {
    handler() {
      this.fruits.push('Orange')
    }
  }
}
</script>
```

<br/>
<br/>

### 상태유지 
Vue가 v-for에서 렌더링된 엘리먼트 목록을 갱신할 때 기본적으로 "in-place patch" 전략을 사용한다. <br/>
 데이터 항목의 순서가 변경된 경우 항목의 순서와 일치하도록 DOM(HTML) 요소를 이동하는 대신 Vue는 각 요소를 적절한 위치에 패치하고 해당 인덱스에서 렌더링할 내용을 반영하는지 확인한다.

이 기본 모드는 효율적이지만 목록의 출력 결과가 하위 컴포넌트 상태 또는 임시 DOM 상태(예: 폼 input)에 의존하지 않는 경우에 적합하다.

각 노드의 id를 추적하여, 재사용하거나 순서를 변경하는등의 작업을 위해 각 아이템에 유일한 key 속성을 주어, vue에게 힌트를 줄 수 있다.

```
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>
```
<br/>

<blockquote> 객체나 배열처럼, 기본 타입(Primitive value)이 아닌 값을 v-for의 키로 사용해서는 안된다. 대신 문자열이나 숫자를 사용. </blockquote>

<br/>
<br/>

### 배열 변경 감지 

#### 변이 메소드
Vue는 감시중인 배열의 변이 메소드를 래핑하여 뷰 갱신을 트리거합니다. 래핑된 메소드는 다음과 같습니다. <br/>

<ul>
<li> push() </li>
<li> pop() </li>
<li> shift() </li>
<li> unshift() </li>
<li> splice() </li>
<li> sort() </li>
<li> reverse() </li>
</ul>

콘솔을 열고 변형 메소드를 호출하여 이전의 예제의 items 배열로 사용할 수도 있다.

<br/>
<br/>

### 배열 교체 

이름에서 알 수 있듯이 변이 메소드는 호출된 원래 배열을 변경한다. <br/>
이에 비해 filter(), concat() and slice()와 같은 원래 배열을 변경하지는 않지만 항상 새 배열을 반환하는 <b>비-변이 메소드</b>도 있다. <br/> 
비-변이 메소드로 작업할 때 이전 배열을 새 배열로 바꿀 수 있다.

```
example1.items = example1.items.filter(item => item.message.match(/Foo/))
```

<br/>
<br/>

### ```<template>```에서의 v-for

템플릿의 v-if와 마찬가지로, v-for와 함께 ```<template>```태그를 사용하여 여러 요소의 블록을 렌더링 할 수도 있다.

```
<ul>
  <template v-for="item in items"> // 태그는 보이지 않음
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

<br/>
<br/>

###  v-if가 있는 v-for
동일한 노드에 있는 경우, v-if는 v-for보다 우선 순위가 높다. <br/>
즉, v-if 조건은 v-for 범위의 변수에 접근할 수 없다.

```
<!-- This will throw an error because property "todo" is not defined on instance. -->

<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

<b> <Template>로 v-for을 랩핑해서 사용한다. </b>
```
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo }}
  </li>
</template>
```

<br/>
<br/>

<hr/>

**Reference**

<ul>
  <li><a href="https://v3.ko.vuejs.org/guide/list.html">VUE.JS</a></li>
</ul>