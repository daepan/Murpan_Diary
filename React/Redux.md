<h1>What is the Redux</h1>
리덕스는 리액트에서 가장 많이 사용되는 상태 관리 라이브러리중 하나이다. 리덕스를 사용하면 컴포넌트의 상태 업데이트 관련 로직을 다른 파일로 분리시켜서 효율적으로 관리할 수 있다.

Redux 기본 용어
리덕스에 대해 기본적으로 알아야할 개념들을 먼저 숙지해보자.

액션(Action)

상태에 변화가 필요하다면 액션을 일으켜야한다. 액션은 객체로 표현되며 type필드를 반드시 가지고 있어야 한다.
```
{
   type: 'ADD_TODO',
   data: {
       id: 1,
       text: '리덕스 배우기'
   }
}
```
액션 생성함수(Action Creator)

액션 생성함수는 액션 객체를 만들어주는 함수이다. 화살표 함수로도 표현이 가능하다.
```
function addTodo(data) {
 return {
   type: 'ADD_TODO',
   data,
 }
}
```
리듀서(reducer)

리듀서를 한국어로 번역해보면 변화를 일으키는 것을 말한다. 리듀서는 현재 상태와 액션 객체를 받아, 필요하다면 새로운 상태를 리턴하는 함수이다. 액션 유형을 기반으로 이벤트를 처리하는 이벤트 리스너라고 생각하면 된다.
```
const initialState = {
 counter: 1,
}
function reducer(state = initialState, action) {
 switch (action.type) {
   case INCREMENT:
     return {
       counter: state.counter + 1,
     }
   default:
     return state
 }
}
```
스토어(Store)

스토어에는 상태가 들어있다. 하나의 프로젝트는 하나의 스토어만 가질 수 있다.

디스패치(Dispatch)

스토어의 내장 함수 중 하나인 디스패치는 액션 객체를 넘겨줘서 상태를 업데이트 하는 유일한 방법이다. 이벤트 트리거라고 생각할 수 있다.

구독(Subscribe)

스토어의 내장 함수 중 하나인 구독은 리스너 함수를 파라미터로 넣어 호출하면 상태가 업데이트될 때마다 호출된다. 일종의 이벤트 리스너라고 볼 수 있다.
```
const listener = () => {
 console.log('상태가 업데이트됨')
}
const unsubscribe = store.subsribe(listener)
unsubscribe() // 추후 구독을 비활성화할 때 함수를 호출
```
셀렉터(Selector)

일반적인 vanilla.js의 리덕스에서는 스토어의 내장함수인 getState를 사용하지만 react-redux에서는 상태 값을 가져올 때 사용한다.