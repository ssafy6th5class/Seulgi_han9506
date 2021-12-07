# Vue

##### vuex

- 상태 관리 패턴 + 라이브러리
- 상태(state)를 전역 저장소(store)로 관리할 수 있도록 지원하는 라이브러리
- 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소
- 동일한 state를 공유하는 다른 컴포넌트들도 동기화

##### vuex(2)

- State

  1. 중앙에서 관리하는 모든 상태 정보(data)

  2. 각 애플리케이션마다 하나의 저장소만 갖게 됨
  3. 각 컴포넌트는 Vuex Store에서 state 정보를 가져와서 사용

- Mutations

  1. state를 변경하는 유일한 방법
  2. handle는 동기적임(비동기적 로직은 state가 변화하는 시점이 의도한 것과 달라질 수 있고, 콜백이 실제로 호출될 시기를 알 수 없음)
  3. 첫번째 인자로 state를 받고, Actions에서 commit( ) 메서드에 의해 호출

- Actions

  1. 비동기 작업 포함

  2. context 객체 인자 받음, 컴포넌트에서 dispatch( ) 메서드에 의해 호출

  3. Actions를 통해 state를 조작할 수 있지만 하지 않음

     -> 명확한 역할 분담을 통해 규모가 커졌을 때, state를 올바르게 관리하기 위해서

- Getters

  1. state를 활용하여 계산 수행, 실제 상태를 변경하지는 않음
  2. getters의 결과는 state 종속성에 따라 캐시되고, 종속성이 변경된 경우에만 재계산

##### vuex(3)

- index.js : Vuex core concepts(actions, mutations, state 등)이 작성되는 곳

- 컴포넌트에서 Vuex Store의 state에 접근

  ```vue
  $store.state
  ```

- Pass Props

  ```vue
  // TodoList.vue
  <todo-list-item :todo="todo"></todo-list-item>   // 하위 컴포넌트로 내려보낼 데이터 작성
  
  // todoListItem.vue
  {{todo.title}}      // 데이터 사용
  
  props: {    // 부모 컴포넌트로부터 받은 데이터 등록
  	todo: {
  		type: Object,
  	}
  }
  ```

- Actions

  ```vue
  this.$store.dispatch('createTodo', todoItem)   // 컴포넌트에서 Actions 호출
  ```

- Mutations

  ```vue
  // index.js
  mutations: {
  	CREATE_TODO: function (state, todoItem) {
  		state.todos.push(todoItem)
  	}
  }
  actions: {
  	createTodo: function (context, todoItem) {
  		context.commit('CREATE_TODO', todoItem)// actions에서 commit 통해 mutations 호출
  	}
  }
  actions: {
  	createTodo: function ({commit}, todoItem) {   // 구조분해할당으로 다시 작성
  		commit('CREATE_TODO', todoItem)
  	}
  }
  ```

- getters

  ```vue
  // index.js
  getters: {
  	completedTodosCount: function (state) {
  		return state.todos.filter(todo => {
  			return todo.isCompleted === true
  		}).length
  	}
  }
  
  //App.vue
  <h2>Completed Todo: {{ completedTodosCount }}</h2>
  
  computed: {
  	completedTodosCount: function () {
  		return this.$store.getters.completedTodosCount
  	}
  }
  ```

- Javascript Spread Syntax : 전개 구문. 배열이나 문자열과 같이 반복 가능한 문자를 요소로 확장하여 0개 이상의 key-value의 쌍으로 된 객체로 확장

  ```vue
  if (todo===todoItem) {
  	return {
  		...todo,     // 객체 복사
  		isCompleted: !todo.isCompleted
  	}
  }
  ```

##### Compontent Binding Helper

- 배열 조작을 편하게 할 수 있도록 하는 것

- mapState : Computed와 State를 매핑. state를 객체 전개 연산자로 계산하여 추가

  ```vue
  import {mapState} from 'vuex'
  
  computed: {
  	...mapState([
  		'todos',
  	])
  }
  ```

- mapGetters : Computed와 State를 매핑. getters를 객체 전개 연산자로 계산하여 추가

- mapActions : actions를 객체 전개 연산자로 계산하여 추가

- mapMutations

##### LocalStorage

- Vuex state : Vuex state를 자동으로 브라우저의 LocalStorage에 저장해주는 라이브러리

- 페이지가 새로고침 되어도 Vuex state를 유지시킴

  ```vue
  npm i vuex-persistedstate
  ```

  ```vue
  import createPersistedState from 'vuex-persistedstate'
  
  export default new Vuex.Store({
  	plugins: [
  		createPersistedState(),
  	],
  })
  ```

##### actions 없이 mutations으로만 state를 변경해도 되는가?

- 변경할 수 있다. 하지만 저장소의 각 컨셉은 역할이 존재하도록 설계되어 있다.