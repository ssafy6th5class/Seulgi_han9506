# Vue

##### Component

- 기본 HTML Element를 확장하여 재사용 가능한 코드를 캡슐화하는데 도움, 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소. Vue 컴포넌트 === Vue 인스턴스 === .vue

##### SFC(Single File Component)

- Vue의 컴포넌트 기반 개발의 핵심 특징
- 화면의 특정 영역에 대한 HTML, CSS, JavaScript 코드를 하나의 파일(.vue)에서 관리
- 각 기능 별로 파일을 나눠서 개발하는 것이 유지 보수 비용 적절
- Vue Component
  - 하나의 화면에서도 기능 별로 각기 다른 컴포넌트가 존재
  - 하나의 컴포넌트는 여러 개의 하위 컴포넌트 가질 수 있음
  - Vue 컴포넌트는 꼭 파일 단위가 아닌 단일 .html 파일 안에서도 여러 개의 컴포넌트 만들 수 있음

##### Vue CLI

- Vue.js를 개발을 위한 표준 도구
- Node.js : 자바스크립트를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 자바스크립트 런타임 환경
- NPM(Node Package Manager) : 자바스크립트 언어를 위한 패키지 관리자

##### Babel & Webpack

- Babel : 자바스크립트의 ECMAScript 2015+ 코드를 이전 버전으로 번역해주는 도구, 원시코드(최신버전) -> 목적코드(구버전)으로 옮기는 번역기(JavaScript compiler)
- Webpack : 모듈 간의 의존성 문제를 해결하기 위한 도구. Webpack은 bundler 중 하나. 여러 모듈을 하나로 묶어주고 묶인 파일은 하나로 합쳐짐
  - static module bundler : 하나의 모듈 === 하나의 스크립트

##### Vue 프로젝트 구조

- node_modules : node.js 환경의 여러 의존성 모듈
- public / index.html : Vue 앱의 뼈대가 되는 파일, 실제 제공되는 단일 html 파일
- src
  - assets : webpack에 의해 빌드된 정적 파일
  - components : 하위 컴포넌트들이 위치
  - App.vue : 최상위 컴포넌트
  - main.js : webpack이 빌드를 시작할 때 가장 먼저 불러오는 entry point. Vue 전역에서 활용할 모듈을 등록할 수 있는 파일
- babel.config.js : bable 관련 설정이 작성된 파일
- package.json : 프로젝트의 종속성 목록과 지원되는 브라우저에 대한 구성 옵션이 포함
- package-lock.json : node_modules에 설치되는 모듈과 관련된 모든 의존성을 설정 및 관리

##### Pass Props & Emit Events

- Vue app은 중첩된 컴포넌트 트리로 구성
- 컴포넌트간 부모-자식 관계가 구성되며 이들 사이에 의사 소통 필요
- 부모는 자식에게 데이터를 전달(Pass props)하며 자식은 부모에게 일어난 일을 부모에게 알림(Emit event)
- 부모는 props를 통해 자식에게 데이터를 전달하고, 자식은 events를 통해 부모에게 메시지를 보냄
- 컴포넌트 구조
  - 템플릿(HTML) : HTML의 body 부분, 각 컴포넌트를 작성
  - 스크립트(JavaScript) : JavaScript가 작성되는 곳, vue 인스턴스 구성
  - 스타일(CSS) : 컴포넌트의 스타일

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!--3.보여주기-->
    <about></about>
  </div>
</template>

<script>
// 1. 불러오기
import About from './components/About.vue'

export default {
  name: 'App',
  components: {
    // 2. 등록하기
    About,
  },
```

- Props : 데이터는 Props 옵션을 사용하여 부모 컴포넌트에서 자식 컴포넌트로 전달됨, 단방향 데이터 흐름(자식 요소가 의도치 않게 부모 요소의 상태를 변경하여 앱의 데이터 흐름을 이해하기 어렵게 만드는 것 방지, 부모의 속성이 변경되면 자식 송석에게 전달되지만 반대로는 안됨)

  - camelCase : 선언시
  - kebab-case : HTML

  ```vue
  // App.vue
  <template>
    <div id="app">
      <img alt="Vue logo" src="./assets/logo.png">
      <!--자식 컴포넌트에 보낼 prop 데이터 선언-->
      <about 
  		my-message="this is prop data"
          <!--부모에서 데이터 업데이트될때마다 자식 데이터로 전달-->
          :parent-data="parentData"  <!--부모 데이터의 props를 동적으로 바인딩-->
       	@child-input-change="parentGetChange"   <!--청취, parentGetChange 메서드 호출-->
       >   
      </about>
    </div>
  </template>
  <script>
  import About from './components/About.vue'
  
  export default {
    name: 'App',
    components: {
      About,
    },
    data: function () { // data는 반드시 함수, 인스턴스가 모두 같은 data 객체 공유하는 것을 방지
      return {
        parentData: 'This is parent data by v-bind',    
      }
    },
    methods: {
      parentGetChange: function (inputData) {    // inputData : this.childInputData
        console.log(`About으로부터 ${inputData}를 받음!!!`)
      }
    }
  }
  </script>
  
  
  // About.vue
  <template>
    <div>
      <h1>About</h1>
      <h2>{{ myMessage }}</h2>
      <h2>{{ parentData }}</h2>
      <input 
      type="text"
      @keyup.enter="childInputChange"
      v-model="childInputData"
      >
    </div>
  </template>
  
  <script>
  export default {
    name: 'About',
    props: {
      myMessage: String,   // '사용하겠다' 선언
      parentData: String,
    },
  }
    methods: {
      childInputChange: function () {
        // child-input-change 이벤트 트리거
        this.$emit('child-input-change', this.childInputData)
      }
    }
  }
  </script>
  ```

- Emit : 현재 인스턴스에서 이벤트를 트리거, 추가 인자는 리스터의 콜백 함수로 전달. 부모 컴포넌트는 자식 컴포넌트가 사용되는 템플릿에서 v-on을 사용하여 자식 컴포넌트가 보낸 이벤트를 청취

##### Vue Router

- 라우트에 컴포넌트를 매핑한 후, 어떤 주소에서 렌더링할지 알려줌. 위치에 대한 최적의 경로를 지정하며, 이 경로를 따라 데이터를 다음 장치로 전향시키는 장치

  - 기존 프로젝트를 진행하고 있던 도중에 추가하면 App.vue를 덮어쓰게 되므로 백업해야 함

- Vue Router - index.js : 라우트에 관련된 정보 및 설정이 작성되는 곳

- Vue Router - router-link : 사용자 네비게이션을 가능하게 하는 컴포넌트, 목표 경로는 'to'. 브라우저가 페이지를 다시 로드하지 않음

- Vue Router - router-view : 라우트에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트

- History mode : HTML History API를 사용해서 라우터를 구현한 것, 페이지를 다시 로드하지 않고 URL 탐색.

- Named Routes : 이름을 가지는 라우트

  ```vue
  <router-link :to="{ name: 'Home' }">Home</router-link> |
  <router-link :to="{ name: 'About' }">About</router-link> |
  <router-link :to="{ name: 'TheLotto', params: { lottoNum: 6 }}">TheLotto</router-link>
  ```

- 프로그래밍 방식 네비게이션 : 다른 URL로 이동하기 위해 this.$router.push 호출

  ```vue
  <template>
    <div class="about">
      <h1>This is an about page</h1>
      <button @click="moveToHome">Home으로 이동</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'About',
    methods: {
      moveToHome: function() {
        this.$router.push({ name: 'Home' })
      }
    }
  }
  </script>
  ```

- Dynamic Route Matching : 동적 인자 전달. 주어진 패턴을 가진 라우트를 동일한 컴포넌트에 매핑해야 하는 경우

  ```javascript
  // index.js
  const routes = [
    {
      path: '/lotto/:lottoNum',   // :lottoNum이 동적 인자
      name: 'TheLotto',
      component: TheLotto,
    }
  ]
  ```

  ```vue
  // TheLotto.vue
  <template>
    <h2>{{ $route.params.lottoNum }}개의 번호를 추첨합니다.</h2>
  </template>
  
  <script>
    methods: {
      getLuckyNum: function () {
        const numbers = _.range(1, 46)
        this.selectedLuckyNum = _.sampleSize(numbers, this.$route.params.lottoNum)
      }
    }
  }
  </script>
  ```

##### components와 views

- App.vue : 최상위 컴포넌트
- views/ : router에 매핑되는 컴포넌트를 모아두는 폴더
- components/ : router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더

##### 라우팅 처리

- SSR : 라우팅에 대한 결정권을 서버가 가짐
- CSR : 클라이언트는 더 이상 서버로 요청을 보내지 않고, 응답받은 HTML 문서안에서 주소가 변경되면 특정 주소에 맞는 컴포넌트 렌더링, 라우팅에 대한 결정권을 클라이언트가 가짐