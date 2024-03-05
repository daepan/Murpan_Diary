# 로그인 리팩토링: useState 사용 줄이기
## 서론
이번 레거시 그룹에서 시작하게된 코드 리팩토링과 관련된 기술에 대한 내용을 정리하는 것을 목표로 내용을 
state를 필요한 곳에서만 적재적소하게 사용하는 것을 최대 목표로 하였습니다.

그렇다면 state와 관련되어서 어떤 것이 문제인지 간단한 예제를 제시해보겠습니다.



## 문제제기
우리는 코인 리코드 작업 중에 어떤 문제가 있는지 탐색하던 중 로그인 화면에서 문제를 찾을 수 있었습니다.
해당 아래 이미지를 확인해주세요

[코인 로그인 페이지 입력 시 계속 리렌더링 표시가 남]

이러한 이미지 현상을 왜 이런 현상이 일어나는지에 대한 내용을 설명해보겠습니다.

[로그인 예시 폼 추가]

위의 코드에서 state를 통해 ID와 패스워드의 value를 추적하게 되고 입력 하나마다 이를 검사하고 리렌더링이 일어나는 것을
react-extension을 통해 알 수 있습니다.

state는 어떻게 리액트 랜더링을 발생시키는 것일까요?
useState 훅을 사용할 때 setState 함수를 호출하면 해당 컴포넌트는 상태가 변경되었다고 판단하여 리렌더링이 발생합니다. 이는 React의 기본 동작 방식 중 하나입니다.
그렇다면 이것을 극복할 방법은 없는 것일까요?

저희는 이 해결 방안에 대해 총 2가지를 논의하였습니다.

1. [React.memo](https://react.dev/reference/react/memo)를 통한 리렌더링 방지
2. [useRef](https://react.dev/reference/react/useRef#useref)와 [useImpretiveHandle](https://react.dev/reference/react/useImperativeHandle)을 활용한 컨트롤

1번의 방법으로 시도해 보았을 때에는
이렇게 우리가 의도하지 않은 단순한 입력이지만 페이지에서 강제적으로 렌더링 하는 것을 확인할 수 있다. 
과연 이런 부분에서 state를 사용할 필요가 있을까라는 생각을 하게 되었고, 실제 코드를 분석하고 useState의 어떤 부분에서 이런 문제가 발생하는지 분석해보자

## useState란?
useState는 React에서 제공하는 Hook 중 하나입니다. 함수형 컴포넌트에서 상태를 관리할 때 사용됩니다.

useState를 사용하면 현재 상태 값과 해당 값을 업데이트할 수 있는 함수를 반환합니다. 이를 통해 상태를 생성하고 변경할 수 있습니다. 상태 값은 컴포넌트의 렌더링 사이클 동안 유지되며, 상태가 변경될 때마다 컴포넌트가 리렌더링됩니다.

예를 들어, 다음과 같이 useState를 사용하여 상태를 관리할 수 있습니다.

리액트 딥다이브 관련 링크: https://www.youtube.com/watch?v=7YhdqIR2Yzo

## diffing 알고리즘의 가정


## setState의 딥 다이브: setState는 어떤 식으로 구현되어 있는가?
Reference : [state](https://bogdan-lyashenko.github.io/Under-the-hood-ReactJS/stack/book/Part-8.html)
---

## useRef를 활용한 current를 활용


## Digging Tech useState는 어떻게 상태를 확인하고 업데이트할까?

# 회원가입 부분 // 다음 내용


