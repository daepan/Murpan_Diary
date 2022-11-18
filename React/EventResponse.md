# Event Response

리액트에서 이벤트를 설정할 때 JSX 태그에 props를 통해 값을 전달하고 해당 이벤트를 발생 시킬 수 있다. 
```js
export default function Button() {
  function handleClick() {
    alert('클릭');
  }

  return (
    <button onClick={handleClick}>
      클릭하기
    </button>
  );
}
```

이렇게  `handleClick`이라는 이벤트 핸들러라는 함수를 선언하여 button 태그에 props 값으로 전달하는 코드입니다.

규칙에 따라서 이벤트 핸들러의 이름을 지정하는 것은 handler라는 이름을 붙이는 것이 일반적인 규정이라고 합니다.

## 이벤트 핸들러는 호출이 아닌 전달
우리는 위 코드를 작성하다가 하나의 의문이 들기도 한다,

> handleClick 이 아니라 handleClick()으로 줘야하는 거 아닌가?

이건 정말 미묘한 차이인데 첫번째 handleClick 함수는 onClick 이벤트 핸들러로 전달됩니다. 
React는 이것을 기억하고 사용자가 버튼을 클릭할 때만 함수를 호출하도록 지시합니다.

그렇다면 handleClick()으로 넣게 되면 무슨일이 일어날까?
만약 이 태그가 렌더링이 된다면 클릭하지 않아도 즉시 handleClick() 함수를 실행합니다. 
그 이유는 자바스크립트가 JSX 내부에 있어 바로 실행되기 때문입니다.

만약 함수와 같은 괄호를 선언하게 될 경우에는

> onClick = {() => alert(‘클릭’)} 

위의 화살표 방식을 활용해서 사용할 수 있다.
