# 인스턴스와 라이프사이클

### 인스턴트

뷰로 화면을 개발하기 위해 필수적으로 생성해야하는 기본 단위

```
new Vue({
	...
});
```
<li> 생성자 : 객체를 새로 생성할 때 자주 사용하는 옵션과 기능들을 미리 저장해 놓고, 새로 개체를 생성할 때 기존에 포함된 기능과 더불어 기존 기능을 쉽게 확장하여 사용하는 기법 </li> <br/>

<li> 옵션 속성
template : 화면에 표시할 HTML, CSS 등의 마크업 요소를 정의하는 속성
method : 화면 로직 제어와 관계된 메서드를 정의하는 속성, 마우스 클릭 이벤트 처리와 같이 화면의 전반적인 이벤트와 화면 동작과 관련된 로직을 추가 할 수 있다. </li> <br/>

<li> created : 뷰 인스턴스가 생성되자마자 실행할 로직을 정의할 수 있는 속성 </li>

<br/>
<br/>

### 인스턴트 생성하기 

모든 Vue 어플리케이션은 createApp 함수를 사용하여 새로운 어플리케이션 인스턴스를 생성하여 시작한다.

``` 
const app = Vue.createApp({ // options // })
createApp(App).mount('#app') 
```
대부분의 메소드들은 동일한 인스턴스를 반환하여 연결(chaining)을 허용한다.

<br/>
<br/>

### 컴포넌트

 조합하여 화면을 구성할 수 있는 블록 

```
const app = Vue.createApp(RootComponent = //App.vue// )
// 어플리케이션을 mount 할 때 렌더링의 시작점으로 사용된다.

const vm = app.mount('#app')
```

컴포넌트는 각각 고유한 유효 범위를 갖고 있기 때문에 직접 다른 컴포넌트의 값을 참조할 수 없다. </br>

따라서 뷰 프레임워크 자체에서 정의한 컴포넌트 데이터 전달 방법을 따라야한다.
상위(부모) - 하위(자식) 컴포넌트 간의 데이터 전달 방법을 쓴다.

<br/>
<br/>

### 컴포넌트 인스턴스 속성들

App.vue 처럼 하나의 뷰 컴포넌트 밖에서도 컴포넌트 안에 내용들을 확인할 수 있다. 

```
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.count) // => 4
```

<br/>
<br/>

## 라이프사이클 훅
각 컴포넌트는 생성될 때 일렬의 초기화 단계를 거친다. <br/>
<ul>
<li>데이터 관찰</li>
<li>템플릿 컴파일(변환)</li>
<li>인스턴스를 DOM에서 마운트 (HTML에 연결)</li>
<li>데이터 변경 시 DOM을 업데이트 (화면을 갱신)</li>
</ul> <br>

반응성이 있을 때 라이프사이클 훅 함수를 실행한다.

<img src="https://v3.ko.vuejs.org/images/lifecycle.svg" alt="Lifecycle hooks">

<li> beforeCreate
: 인스턴스가 생성되고 나서 가장 처음으로 실행되는 라이프 사이클 단계, data속성과 metods속성이 아직 정의되어 있지 않고, 돔과 같은 화면 요소에 접근할 수 없음 </li><br/>

<li> created
: data속성과 metods속성에 정의된 값에 접근하여 로직을 실행 할 수 있지만, 인스턴스가 화면요소에 부착되기 전이기 때문에 template속성에 정의된 돔 요소로 접근할 수 없음 </li><br/>

<li> beforeMount
: template속성에 지정한 마크업 속성을 render()함수로 변환 후 el속성에 지정한 화면 요소에 인스턴스를 부착하기 전에 호출되는 단계 </li><br/>

<li> mounted
: el속성에 지정한 화면 요소에 인스턴스가 부착되면 호출되는 단계 </li><br/>

<li> beforeUpdate
: el속성에서 지정한 화면 요소에 인스턴스가 부착되고 나면 인스턴스에 정의한 속성들이 화면에 지환 </li><br/>

<li> updated
: 데이터가 변경되고 나서 가상 돔으로 다시 화면을 그리고 나면 실행되는 단계 (데이터 변경이 일어나 화면이 다시 그려졌을 때 호출됨) </li><br/>

<li> beforeDestroy
: 뷰 인스턴스가 파괴되기 직전에 호출되는 단계, 아직 인스턴스에 접근할 수 있음 </li><br/>

<li> destroy
: 뷰 인스턴스가 파괴되고 나서 호출되는 단계, 뷰 인스턴스에 정의된 모든 속성이 제거되고 하위에 선언한 인스턴스들 또한 모두 파괴된다 </li><br/>

```
<template>
  <h1>{{ count }}</h1>
</template>

<script>
export default {
  data() {
    return {
      count: 2
    }
  },
  beforeCreate() {
    console.log('Before Create!', this.count) // Before Create! undefined
  },
  created() {
    console.log('Created!, this.count') // Created! 2
    console.log(document.querySelector('h1')) // null
  },
  beforeMount() {
    console.log('Before Mount!')
    console.log(document.querySelector('h1')) // null
  },
  mounted() {
    console.log('Mounted!')
    console.log(document.querySelector('h1')) // <h1>2</h1>
  } // beforeCreate부터 밑 코드들 순서가 바뀌더라도 결과는 beforeCreate => created => beforeMount => mounted 순으로 출력된다.
}
</script>
```
