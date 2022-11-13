# useMemo와 useCallback를 언제 사용해야 하는가?

React에서 `useMemo`와 `useCallback` Hooks는 성능 최적화(불필요한 렌더링 방지)를 위해 사용한다고 적혀있습니다. 하지만 이 두개의 Hooks는 조심해서 사용해야합니다. 불필요한 렌더링 방지로 성능 최적화를 한다고 알고 있을 수도 있겠지만 불필요한 렌더링 방지를 위해서 발생하는 비용때문에 성능 최적화가 아니라 성능이 더 느려질 수도 있기 때문입니다! 😕

`useCallback`은 memozation된 callback을 반환해주고, `useMemo`는 memozation된 값을 반환해주어 함수 컴포넌트에서 재렌더링할 때 함수 내에서 callback이나 값을 재선언을 막아주어(컴포넌트가 재렌더링할 때 함수가 순차적으로 실행되므로) 성능 최적화를 해준다고 알고 있습니다. 하지만 이는 재선언을 막을 때의 비교 비용을 고려하지 않은 상태일 때입니다. 다음의 간단한 예는 성능 최적화가 아닌 성능이 더 느려짐을 알 수 있습니다.

```jsx
// useCallback을 쓰지 않는 경우
const handleChange = e => setName(e.target.value);
// useCallback을 쓰는 경우입니다. 
const handleChange = useCallback(e => setName(e.target.value), []);
// 하지만 이것은 아래의 두 번의 선언과 같습니다.
const handleChange = e => setName(e.target.value);
const handleChangeCallback = useCallback(handleChange, []);
```

`useCallback`과 `useMemo`는 다음과 같은 이유때문에 내장되었습니다.

**참조 동일성**

React의 Hooks의 일부(`useCallback`, `useMemo`, `useEffect`, `useLayoutEffect`)는 dependency list를 갖습니다. 일반적으로 이 Hooks들은 dependency list 내의 값이 변경되었을 때 정해진 행동을 실행합니다. dependency list 중에 배열/객체/함수가 포함되어 있을 경우 이야기가 달라집니다.

dependency list에 배열/객체/함수의 값이 바뀌지 않았는데 리렌더링때문에 참조가 달라져 Hooks를 불필요하게 실행하게 됩니다. 이때 `useCallback`과 `useMemo`를 사용하면 리렌더링해도 참조가 같게 유지할 수 있기 때문에 Hooks를 실행하지 않게 만듭니다.

React에서 컴포넌트는 props/state가 바뀌었을 때 자식 컴포넌트까지 리렌더링을 합니다. 이 때에도 위와 같이 배열/객체/함수가 똑같은데도 단지 재렌더링할 때 같은 곳을 참조하지 않아서 생기는 불필요하게 자식 컴포넌트까지 렌더링하는 것을 막는데에도 사용됩니다(`React.memo`와 같이).

**복잡하고 자원(메모리)이 많이 드는 계산**

위에서 말했듯이 React에서 컴포넌트는 props/state가 바뀌었을 때 자식 컴포넌트까지 리렌더링하게 됩니다. 이때 복잡한 계산에 사용되는 변수가 바뀌었을 때만 계산하도록 `useMemo`를 사용할 수 있습니다.

글쓴이는  무엇보다도 "측정없이 추상화/성능 최적화를 하는 것은 비용이 따르기 때문에 성능 측정을 최우선시 되어야 한다"고 합니다. 정확한 것은 다음 section에서 알아보죠🙂

## [AHA 프로그래밍?](https://kentcdodds.com/blog/aha-programming)

위 section에서 말했듯이 글쓴이인 Kent C. Dodds는 "측정없이 추상화/성능 최적화를 하는 것은 비용이 따르기 때문에 성능 측정을 최우선시 되어야 한다"라고 말하면서 DRY, WET, AHA 프로그래밍 원리를 소개합니다.

DRY는 "Don't Repeat Yourself"의 약자로서 코드 복사, 붙여넣기를 하지 말라는 뜻입니다. 코드 복사 붙여넣기를 할 때 오류가 있는 코드를 복사했을 경우 나중에 붙여넣기한 코드도 수정을 해야합니다! 중복되는 코드의 경우 추상화하여 함수로 만드는 걸 권장한다고 하네요.

WET은 "Write Everything Twice"의 약자로 DRY와 비슷합니다. 비슷한 코드를 두 번 쓰는 것은 괜찮지만, 세 번 쓰는 것은 안 된다(세 번째가 된다면 그때 시간을 들여서 추상화를 해라)는 뜻입니다. 물론 실제로는 과도한 추상화를 하지 말라는 말이죠.

AHA는 "Avoid Hasty Abstraction"의 약자로 섣부른 추상화를 하지 말라는 뜻입니다. 즉, 중복된 코드가 잘못된 추상화가 적용된 코드보다 생산성이 높으며, 요구사항에 따라 어떻게 변화할 지 모르기 때문에 중복 코드에 대한 사례를 다 알기 전에는 중복 코드를 사용하는 것에 두려워 하지 않는 것이 좋다는 원리입니다.

글쓴이는 DRY, WET 프로그래밍보다 AHA 프로그래밍을 선호한다고 하네요 🙂