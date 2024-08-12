# 로그인 리팩토링: useState 사용 줄이기
## 서론
이번 레거시 그룹에서 시작하게된 코드 리팩토링과 관련된 기술에 대한 내용을 정리하는 것을 목표로 내용을 
state를 필요한 곳에서만 적재적소하게 사용하는 것을 최대 목표로 하였습니다.
그렇다면 state와 관련되어서 어떤 것이 문제인지 간단한 예제를 제시해보겠습니다.

## 문제제기
우리는 코인 리코드 작업 중에 어떤 문제가 있는지 탐색하던 중 로그인 화면에서 문제를 찾을 수 있었습니다.
해당 아래 이미지를 확인해주세요

[코인 로그인 페이지 입력 시 계속 리렌더링 표시가 남]

위의 코드에서 `state`를 통해 ID와 패스워드의 `value`를 추적하게 되고 입력 하나마다 이를 검사하고 리렌더링이 일어나는 것을 chrome의 `react-devTool-extension`을 통해 알 수 있습니다.
이런 현상이 일어나는지에 대한 내용을 한번 예시를 통해 소개해드리겠습니다.


```jsx
import React, { useState } from 'react';

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={(e) => setName(e.target.value)} />
      </label>
      <br />
      <label>
        Address{': '}
        <input value={address} onChange={(e) => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}

const Greeting = function Greeting({ name }) {
  console.log('Greeting was rendered at', new Date().toLocaleTimeString());
  return (
    <h3>
      Hello{name && ', '}
      {name}!
    </h3>
  );
};

```
실행 예제: https://react-i8mtvg.stackblitz.io

해당 입력 폼에서 입력할 때 마다 `console.log`가 발생하면서 리렌더링이 발생하는 상황을 알 수 있습니다.
해당 폼에 적용된 로직은 위에서 보게된 이미지와 동일하게 작성되었습니다.

`state`는 리액트 렌더링을 발생시키기 됩니다. 공식문서에 따르면
>`useState` 훅은 두 가지를 제공합니다:
>
> 렌더링 사이에 데이터를 유지하는 상태 변수.
> 변수를 업데이트하고 React가 컴포넌트를 다시 렌더링하도록 트리거하는 상태 설정 함수.
>
> reference: https://react.dev/learn/state-a-components-memory


useState 훅을 사용할 때 setState 함수를 호출하면 해당 컴포넌트는 상태가 변경되었다고 판단하여 리렌더링이 발생합니다. 이는 React의 기본 동작 방식 중 하나입니다. (물론 리액트에서 렌더링 일으키는 건 더 있습니다)

그렇다면 이것을 극복할 방법은 없는 것일까요?
저희는 이 해결 방안에 대해 총 2가지를 논의하였습니다.

1. [React.memo](https://react.dev/reference/react/memo)를 통한 리렌더링 방지
2. [useRef](https://react.dev/reference/react/useRef#useref)와 [useImpretiveHandle](https://react.dev/reference/react/useImperativeHandle)을 활용한 컨트롤

그렇다면 위의 예제 코드를 2가지 방식에 맞게 개선해보았습니다.


1번 방법인 memo를 통해 리렌더링을 방지할 수 있었습니다.
```jsx
import React, { memo, useState } from 'react';

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        이름 {': '}
        <input value={name} onChange={(e) => setName(e.target.value)} />
      </label>
      <br />
      <label>
        Address{': '}
        <input value={address} onChange={(e) => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}

const Greeting = memo(function Greeting({ name }) {
  console.log('Greeting was rendered at', new Date().toLocaleTimeString());
  return (
    <h3>
      Hello{name && ', '}
      {name}!
    </h3>
  );
});
```

실행예제: https://react-m6yd1u.stackblitz.io

### React.memo란?
`React.memo`는 React의 렌더링 최적화를 위해 설계된 고차 컴포넌트`(Higher-Order Component, HOC)`입니다

`React.memo`는 컴포넌트에 전달되는 `props`가 변경되었는지 확인하기 위해 얕은 비교를 수행합니다. 얕은 비교는 객체의 최상위 레벨의 값들만을 비교하는 방법입니다. 즉, `props` 객체나 그 내부의 객체가 가리키는 값이나 참조가 이전 렌더링과 비교해 변했는지 확인합니다.

예제 코드에서는 `React.memo`를 사용함으로써, `name` 값에 변화가 없다면 `<Greeting />` 컴포넌트의 재렌더링을 건너뛰게 됩니다.

2번 방법인 useRef와 useImpretive를 활용한 리렌더링 방지입니다.

```jsx
import React, {
  useState,
  useRef,
  useImperativeHandle,
  forwardRef,
} from 'react';

export default function MyApp() {
  const [name, setName] = useState('');
  const passwordRef = useRef();

  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={(e) => setName(e.target.value)} />
      </label>
      <br />
      <PasswordInput ref={passwordRef} />
      <Greeting name={name} />
    </>
  );
}

const PasswordInput = forwardRef((props, ref) => {
  // input 요소에 대한 ref 생성
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    // 외부에서 접근할 수 있도록 getPassword 함수 제공
    getPassword: () => inputRef.current.value,
  }));

  return (
    <label>
      Password{': '}
      <input
        type="password"
        ref={inputRef} // input 요소에 ref 연결
        defaultValue="" // controlled 대신 uncontrolled 컴포넌트로 사용
      />
    </label>
  );
});

const Greeting = ({ name }) => {
  console.log('Greeting was rendered at', new Date().toLocaleTimeString());
  return (
    <h3>
      Hello{name && ', '}
      {name}!
    </h3>
  );
};
```

실행예제: https://react-ffejjd.stackblitz.io

### useImpretiveHandle과 forwardRef

useImpretiveHandle과 forewardRef를 활용하게 되는데, useImpretiveHandle은 리액트 훅 중 하나로, 부모 컴포넌트에게 노출할 ref 핸들을 사용자가 직접 정의할 수 있게 (ref로 노출 시키는 노드의 일부 메서드만 노출할 수 있게) 해주는 훅입니다.
forwardRef는 사용하면 구성 요소가 참조를 사용하여 DOM 노드를 상위 구성 요소에 노출할 수 있습니다.

### useRef를 활용한 최적화

`useRef` 훅은 React 컴포넌트에서 참조`(ref)`를 생성하고 접근할 수 있게 해줍니다. 이 훅이 반환하는 객체(`ref` 객체)는 `.current` 프로퍼티를 통해 참조된 DOM 요소나 React 엘리먼트에 직접 접근할 수 있게 해줍니다. 여기서는 `passwordRef`를 생성하여 `PasswordInput` 컴포넌트에 전달하고 있습니다. 이를 통해 부모 컴포넌트`(MyApp)`가 자식 컴포넌트`(PasswordInput)` 내부의 함수에 접근할 수 있습니다.

`useState`의 경우에는 상태가 변경되면 컴포넌트가 리렌더링되지만, `.current` 프로퍼티를 활용함을 통해  직접 접근하여 변경을 일으키기 때문에 리렌더링을 발생 시키지 않습니다.



## 그렇다면 어떤 것이 좋을까?

1번과 2번 방법 둘 다 최적화라는 방식에서 결과가 개선된 것을 확인했습니다. 두 기능의 결과에 대한 차별점을 확인해보겠습니다.

*  1번의 경우 
	* React.memo의 경우에는 `React.memo`를 사용함으로써, `name` 값에 변화가 없다면 `<Greeting />` 컴포넌트의 재렌더링을 건너뛰게 됩니다. 이의 경우에는 Address가 변경되어도 name의 상태가 유지됩니다.
* 2번의 경우
	* 반환하는 객체(`ref` 객체)는 `.current` 프로퍼티를 통해 참조된 DOM 요소나 React 엘리먼트에 직접 접근할 수 있게 해줍니다. 여기서는 `passwordRef`를 생성하여 `PasswordInput` 컴포넌트에 전달하면서 최적화 합니다.

그 중, React 공식문서에서[ React.memo의 사용법](https://react.dev/reference/react/memo#usage)과 관련해서 DeepDive 내용에서 이러한 내용이 있었습니다

>If your app is like this site, and most interactions are coarse (like replacing a page or an entire section), memoization is usually unnecessary. On the other hand, if your app is more like a drawing editor, and most interactions are granular (like moving shapes), then you might find memoization very helpful.
>
>"만약 당신의 앱이 이 사이트처럼 대부분의 상호작용이 페이지를 교체하거나 전체 섹션을 대체하는 것과 같은 대략적인(interaction) 형태라면, 메모이제이션은 보통 불필요합니다. 반면에, 당신의 앱이 드로잉 에디터와 같고 대부분의 상호작용이 도형을 이동하는 것과 같이 세밀한(granular) 형태라면, 메모이제이션이 매우 유용할 수 있습니다."

## 결론

메모이제이션을 적용할지 여부는 애플리케이션의 상호작용 방식에 따라 달라집니다. 
여기서 우리가 적용하려는 경우에서는 로그인 페이지에서의`input` value에 대한 state 변화를 하는 경우이기 때문에 컴포넌트 자체가 단순했기에 Ref를 선택하여 최적화를 진행하였습니다.
하지만 이런 결론을 산출하는 과정에서 여러가지 의구심을 남겼습니다.
> React.memo는 언제 써야 베스트 케이스일까?
> 어디에 메모이제이션되는지?
> 어떤 성능 문제가 발생하는지?

다음 글에서는 memo와 관련된 내용에 대해 다이브 해보는 경험을 작성해보겠습니다.



---
# 딥다이브: useState

## diffing 알고리즘의 가정

리액트 딥다이브 관련 링크: https://www.youtube.com/watch?v=7YhdqIR2Yzo
## setState의 딥 다이브: setState는 어떤 식으로 구현되어 있는가?

Reference : [state](https://bogdan-lyashenko.github.io/Under-the-hood-ReactJS/stack/book/Part-8.html)
---

## useRef를 활용한 current를 활용


## Digging Tech useState는 어떻게 상태를 확인하고 업데이트할까?


