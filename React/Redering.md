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

2. 리액트는 이를 `React.createElement()`로 전환
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

이러한 React element들을 컴포넌트 트리로부터 모았다면 React는 새로운 객체트리인 가상 DOM과 비교합니다  
그리고 실제 DOM이 해당 render 결과물처럼 변하기 위해서 바뀌어야 하는 변경 리스트를 만듭니다.   
이렇게 비교하고 계산하는 과정을 *재조정*(Reconcilation)이라고 합니다.   


> *가상DOM에 대해 다시 한번 생각하기*  
>   
> *NOTE: React 팀은 최근 몇 년간 "가상 DOM"이라는 표현을 경시했습니다. Dan Abramov(Redux 개발자)의 트윗*
> 
>저는 "가상 DOM"이라는 용어를 버리길 바랍니다. 2013년에는 말이 되는 용어였습니다. 이걸 쓰지 않으면 사람들은 React가 모든 render마다 DOM노드를 만들 것이라고 믿었기 때문이죠. 그런데 요즘 사람들은 이렇게 생각하지 않습니다.    
"가상 DOM"은 DOM문제의 해결책으로 들릴 수 있겠지만, React는 그렇지 않습니다.   
React는 "value UI (의역: 값으로 UI를 만드는 방식)"입니다. 핵심 원리는, UI는 그저 문자열이나 배열로 이루어진 값이라는 점입니다.  
 값이기에, 변수에 저장하고, 전달하고, JS의 제어 흐름을 따라갈 수 있습니다. 이 표현이 정말 중요한 요점입니다.  
  약간의 DOM 변경을 피하기 위해 비교하는 과정이 아닙니다. React는 언제나 DOM을 표현하는 것이 아닙니다. 예를 들면, <Message recipientId={10} /> 은 DOM이 아닙니다. 개념적으로, Message.bind(null, {recipientId: 10}) 이라는 function을 추후에 호출할 뿐입니다.

그렇다면 여기서 질문 하나  
만약 DOM 엘리먼트가 n개 이상일 경우를 가상 DOM 트리 구조와 실제 DOM을 비교하는 과정을 거친다면 시간복잡도 O(n^3)이라는 굉장한(?) 문제가 있는데  
이를 해결하기 위해 리액트 개발팀은 2가지 가정을 세웁니다.  

1. 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 만들어낸다.
2. 개발자가 key prop을 통해, 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할지 표시해 줄 수 있다.

* 리액트의 비교알고리즘
두 개의 트리(가상 DOM, 실제 DOM)를 비교할 때, React는 두 엘리먼트의 루트부터 비교합니다.  
이후의 동작은 루트에 따라 달라집니다. 

1. 엘리먼트의 타입이 다른 경우

>두 개의 트리의 루트 엘리먼트의 타입이 다르면, React는 이전 트리(현재 DOM 트리)를 버리고 완전히 새로운 트리를 구축합니다. `<a>`에서 `<img>`로, `<Article>`에서 `<Comment>`로, 혹은 `<Button>`에서 `<div>`로 바뀌는 것 모두 트리 전체를 재구축하는 경우입니다.

트리를 버릴 때 이전 DOM 노드들은 모두 제거(``컴포넌트 인스턴스의 componentWillUnmount()가 실행``)됩니다.  
  
 새로운 트리가 만들어질 때, 새로운 DOM 노드들이 DOM에 삽입됩니다.  
 그에 따라 컴포넌트 인스턴스는 UNSAFE_componentWillMount()(이는 레거시라 사용을 지양)가 실행되고 componentDidMount()가 이어서 실행합니다.   
이전 트리와 연관된 모든 state는 제거 새로운 DOM 노드의 라이프 사이클이 작동하여 새로운 DOM을 생성합니다.

2. DOM 엘리먼트의 타입이 같은 경우
>같은 타입의 두 React DOM 엘리먼트를 비교할 때, React는 두 엘리먼트의 속성을 확인하여, 동일한 내역은 유지하고 변경된 속성들만 갱신합니다. 

```js
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

이 두 엘리먼트를 비교하면, React는 현재 DOM 노드 상에 className만 수정합니다.

style이 갱신될 때, React는 또한 변경된 속성만을 갱신합니다.
```js
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />

```
위 두 엘리먼트 사이에서 변경될 때, React는 fontWeight는 수정하지 않고 color 속성만 수정합니다.

DOM 노드의 처리가 끝나면, React는 이어서 해당 노드의 자식들을 재귀적으로 처리합니다.//재귀적 처리는 무엇일까??


3. 같은 타입의 컴포넌트 엘리먼트

>컴포넌트가 갱신되면 인스턴스는 동일하게 유지되어 렌더링 간 state가 유지됩니다. React는 새로운 엘리먼트의 내용을 반영하기 위해 현재 컴포넌트 인스턴스의 props를 갱신합니다. 이때 해당 인스턴스의 UNSAFE_componentWillReceiveProps(), UNSAFE_componentWillUpdate(), componentDidUpdate를 호출합니다.

다음으로 render() 메서드가 호출되고 비교 알고리즘이 이전 결과와 새로운 결과를 재귀적으로 처리합니다.
