### React Fragment

**React Fragment**는 여러 요소를 하나의 부모 요소 없이 그룹화할 수 있는 방법을 제공합니다. 이를 통해 불필요한 DOM 요소를 추가하지 않고도 여러 자식을 반환할 수 있습니다.

#### 특징

- **불필요한 노드 제거**: `<div>`와 같은 불필요한 래퍼 요소를 피할 수 있습니다.
- **간단한 구문**: `<React.Fragment>` 혹은 더 짧게 `<> </>`로 사용할 수 있습니다.



### React Hook

**React Hook**은 함수형 컴포넌트에서 상태와 생명주기 기능을 사용할 수 있게 해주는 기능입니다. React 16.8부터 도입되었습니다.

- 함수형 컴포넌트에서 State와 Lifecycle 기능을 연동해주는 함수
- '클래스형'에서는 동작하지 않으며, '함수형'에서만 사용 가능


기본적인 Hook으로 상태관리를 해야할 때 사용하면 된다.

상태를 변경할 때는, `set`으로 준 이름의 함수를 호출한다.

```
const [posts, setPosts] = useState([]); // 비구조화 할당 문법
```

`useState([]);`와 같이 `( )` 안에 초기화를 설정해줄 수 있다. 현재 예제는 빈 배열을 만들어 둔 상황인 것이다.


#### useEffect

컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook

> '클래스' 컴포넌트의 componentDidMount()와 componentDidUpdate()의 역할을 동시에 한다고 봐도 된다.

```
useEffect(() => {
    console.log("렌더링 완료");
    console.log(posts);
});
```

posts가 변경돼 리렌더링이 되면, useEffect가 실행된다.