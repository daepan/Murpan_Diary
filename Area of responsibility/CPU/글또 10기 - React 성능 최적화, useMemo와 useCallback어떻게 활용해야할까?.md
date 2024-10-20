## 들어가며
리액트를 사용할 때, 성능 최적화는 중요한 과제 중 하나입니다. 작은 규모의 애플리케이션에서는 크게 체감되지 않겠지만, 컴포넌트 구조가 복잡해지고 데이터가 자주 변동하는 상황에서는 불필요한 렌더링이 사용자 경험에 영향을 미칠 수 있습니다. 이런 이유로 성능 최적화가 필요한 시점을 잘 판단하고, 적절한 기법을 사용하는 것이 중요합니다.
### 최적화가 필요한 경우
리액트 애플리케이션에서 최적화가 필요한 대표적인 상황은 다음과 같습니다:

- **상위 컴포넌트의 상태 변화**로 인해 하위 컴포넌트가 불필요하게 다시 렌더링될 때.
- **비용이 많이 드는 계산**(예: 복잡한 연산, API 호출)이나 데이터 처리가 자주 발생할 때.
- 대량의 데이터를 **렌더링하거나 필터링**하는 경우 성능 저하가 발생할 때.

이런 상황에서 자주 사용하는 것이 바로 `useCallback`과 `useMemo`입니다.

21년도 React Conference에서 소개된 테마 색상을 변경할 수 있는 기능이 추가된 간단한 Todo List에서의 경우를 보여드리겠습니다.

![](https://i.imgur.com/gJUQahL.png)

위의 코드에서 themeColor가 드래그를 통해 변경하면 themeColor를 상속 받고 있는 TodoList가 불필요하게 계속해서 재렌더링되는 성능 이슈가 발생합니다.
이에 대해서 `memoization`이나 `debounce`등의 테크닉을 활용하는게 좋아 보인다고 생각하고 우리는 열심히 구현할 것입니다.

구현 예시 : [React Conf: React without memo](https://www.youtube.com/watch?v=lGEMwh32soc)
### 최적화를 위한 useCallback과 useMemo
React로 개발하면서 위에 제시된 최적화가 필요한 상황을 맞이하였을 때, 생각할 수 있는 것은 여러가지가 있을 수 있습니다. Throttle, debounce 같은 처리방식도 있고, React에서는 `useCallback`, `useMemo`와 같은 메모이제이션 훅을 통해 성능을 향상시킬 수 있습니다.

간단하게 두 개의 훅이 어떤 것인지 정리해보겠습니다.

>1.  useCallback
> `useCallback`은 **함수를 메모이제이션**하여, 의존성 배열이 변경되지 않는 한 동일한 함수 객체를 반환합니다. 주로 자식 컴포넌트에 함수를 props로 넘길 때, 불필요한 리렌더링을 방지하기 위해 사용합니다

```js
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

> 2. useMemo
> `useMemo`는 **값을 메모이제이션**하여, 의존성 배열이 변경되지 않는 한 계산을 다시 하지 않고 저장된 값을 반환합니다. 주로 **비용이 많이 드는 계산**을 반복하지 않도록 할 때 사용합니다.

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

둘 다 성능 최적화 용도로 메모이제이션을 활용해서 사용되지만, **useCallback은 함수 재생성 방지**에, **useMemo는 값 재계산 방지**에 초점이 맞춰져 있습니다.

## 메모이제이션의 숨은 비용: 클로저와 메모리

메모이제이션은 성능 최적화를 위해 자주 사용되는 기술이지만, 그 이면에는 메모리 사용과 관련된 숨은 비용이 존재합니다. 특히 클로저와 메모리 관리 측면에서 주의할 필요가 있습니다.

### 클로저와 메모이제이션의 관계

React의 `useMemo`와 `useCallback` 훅은 컴포넌트가 **재렌더링될 때마다 불필요한 재계산을 방지**하기 위해 메모이제이션을 사용합니다. 이를 위해 클로저를 활용해 이전 값을 기억하고, 필요 시 저장된 값을 다시 사용할 수 있게 합니다.

아래 `useMemo`의 React 내부 구현 예시에서 클로저가 어떻게 작동하는지 살펴보겠습니다.


```
### 클로저와 메모리의 관계

위 코드에서 `nextCreate` 함수가 클로저를 통해 의존성 배열과 현재 상태를 **기억**합니다. 그 결과, **값과 의존성 배열은 메모리에 저장**되고, 이후 렌더링에서도 동일한 값을 사용하거나, 의존성 배열이 변할 때만 재계산을 수행합니다.

클로저는 함수가 선언된 환경을 기억하기 때문에, React의 훅 시스템에서 메모이제이션할 값이나 함수를 기억하고 그 값들이 다시 필요할 때 사용될 수 있습니다.

js

코드 복사

`function updateMemo<T>(   nextCreate: () => T, // 메모이제이션할 값을 생성하는 함수   deps: Array<mixed> | void | null, // 의존성 배열 ): T {   const hook = updateWorkInProgressHook();    // 현재 훅의 상태를 가져옵니다. 클로저를 통해 이전 값과 의존성 배열을 참조할 수 있습니다.    const nextDeps = deps === undefined ? null : deps;    // 새로운 의존성 배열이 전달되었는지 확인합니다.    const prevState = hook.memoizedState;    // 이전에 메모이제이션된 값과 의존성 배열을 가져옵니다.    if (nextDeps !== null) {     const prevDeps: Array<mixed> | null = prevState[1];      if (areHookInputsEqual(nextDeps, prevDeps)) {       return prevState[0];        // 클로저를 통해 이전 값과 의존성 배열을 기억하고, 값이 변경되지 않았으면 메모이제이션된 값을 반환합니다.     }   }    const nextValue = nextCreate();    // 의존성 배열이 변경되었으므로 새로운 값을 계산합니다.    hook.memoizedState = [nextValue, nextDeps];    // 새로 계산된 값과 새로운 의존성 배열을 메모이제이션합니다.   return nextValue; }`

### 메모리 사용 측면에서의 문제

이처럼 클로저는 메모이제이션된 값과 그 값이 사용된 환경을 메모리에 계속해서 저장합니다. 의존성 배열이 변경되지 않으면 이 값들이 계속 메모리에 남아 있게 되므로, **메모리 사용량이 증가**할 수 있습니다.

이러한 동작은 특히 다음과 같은 상황에서 문제가 될 수 있습니다:

- **불필요한 메모이제이션**: 자주 변하지 않는 값에 대해 메모이제이션을 남용할 경우, 메모리만 차지하고 성능 개선 효과는 미미할 수 있습니다.
- **큰 데이터나 복잡한 객체를 메모이제이션할 때**: 메모리에 저장된 값이 클 경우, 메모리 누수가 발생할 가능성이 높아집니다.

따라서 메모이제이션을 사용할 때는 **클로저가 불필요한 데이터를 메모리에 계속 유지하는 상황**을 방지하기 위해, 의존성 배열과 계산 비용을 신중하게 고려해야 합니다.