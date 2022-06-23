# React Rendering

최근 리액트에 대한 블랙박스에 관한 주제로 이야기한 적이 있는데 그때 아는 지인에게 소개 받은 새로운 렌더링에 관련된 페이지를 읽게 되었다.  
영어라서 꽤나 읽는데 어려웠지만 듣고 보니 렌더링이란 무엇인가 알게되었고   
리액트는 어떻게 동작하는가에 지식이 늘은 것 같아 이를 정리하려고 한다.

## 렌더링이란?
먼저 리액트에서 말하는 렌더링은 무엇인가에 대한 설명을 읽어보면
```
렌더링은 여러분의 모든 컴포넌트에게 현재 state와 props값의 조합을 기반으로 각각의 UI를 어떻게 화면에 띄우고 싶어하는지 설명하도록 물어보는 React의 프로세스입니다.
```
위의 내용을 정의라고 생각했을 때 막연하게 무엇인가 떠오르지 않을 것이다.  
과정을 통해 한번 둘러보자

1. React에서 JSX로 코드를 작성
```JSX
    return <SomeComponent a={42} b="TestSting">Text</SomeComponent>
```  

2. 리액트는 이를 `React.createElement()`로 전
```js
    return React.createElement(
        SomeComponet,
        {a:42,b:"TestString"},
        "Text"
    )
```
3. 그리고 이는 JS로 element객체로 컴파일
```js
    type: SomeComponent,
    props: {a:42, b:"TestString"},
    chlidren:["Text"]
```

위 과정을 통해 createElement는 의도한 UI 구조를 설명하는 JS 일반 객체인 React elements를 반환한다.

