## 들어가며
최근에 한기대 코인 프로젝트를 진행하며 API 로직에 대한 변화가 있었습니다. tanStack-Query를 통해 useQuery로 API를 관리하고 있었는데, useSuspenseQuery로 변경하는 작업을 진행하고 있습니다.
처음에는 이 useSuspenseQuery가 뭐지에 대해 고민하다 Suspense를 제가 실제로 많이 다룬 경험이 없더군요. 그래서 한번 Suspense 깔끔하게 정리하면서 useQuery보다 useSuspenseQuery를 도입하게 된 이유를 서술하면서 이번 글을 마무리해보려 합니다.

## What is Suspense?

>`<Suspense>` 는 자식 요소가 로드되기 전까지 화면에 대체 UI를 보여줍니다. 
>from. [React 공식 문서](https://ko.react.dev/reference/react/Suspense)

```javascript
<Suspense fallback={<Loading />}>  
	<SomeComponent />  
</Suspense>
```

React의 **Suspense**는 데이터 로딩, 코드 분할 등과 같은 비동기 작업을 보다 직관적이고 깔끔하게 처리하기 위해 고안된 기능입니다. 기존의 로딩 상태 관리 방식은 종종 복잡한 조건문과 state 관리를 요구했으나, Suspense는 이러한 문제를 단순화합니다.

쉽게 위 코드를 해석하면 `<Suspense>` 태그에 감싸진 자식 요소에 필요한 모든 코드와 데이터가 로드될 때까지 fallback에 선언된 컴포넌트를 보여주게 됩니다.
이 Suspense의 기능을 요약하면 다음과 같습니다.

- **Suspense의 주요 역할**
    1. 컴포넌트가 비동기 작업을 완료할 때까지 UI 렌더링을 유예.
    2. 로딩 상태를 컴포넌트 수준에서 간결하게 처리 가능.
    3. 다양한 데이터 소스 및 동적 import를 한 곳에서 관리.

- **기존 방식과의 차이점**  
    기존에는 `isLoading`이나 `isFetching` 같은 상태 플래그를 사용해 데이터가 준비되었는지 확인하고 UI를 조건부 렌더링했습니다. 반면, Suspense는 이 모든 과정을 단순화해 fallback 컴포넌트를 통해 로딩 상태를 처리할 수 있습니다.

## Suspense가 갖는 현대 웹에서의 의미

`Suspense` 가 도입된 것은 18년도 처음으로 리액트 16에서 시범적인 기능으로 들어오게 되었지만, 리액트 18버전으로 넘어오면서 정식 기능으로 출시되었습니다. 이 기능이 갖게 되는 의미를 좀 생각해보면 저는 비동기 처리에 대해서 선언적인 기능이 추가되었다는 좋은 기능이라고 생각합니다.

### 비동기 처리에 대한 선언적 처리

Suspense는 복잡한 비동기 데이터 처리를 더 선언적이고 간단한 방식으로 관리할 수 있도록 도와줍니다.
이전에 데이터를 처리하기 위해서는 `useEffect`나 `try..catch`와 같은 비동기 작업 중간에 제어할 수 있는 기능을 통해 로직을 간결하게 처리할 수 있다고 생각합니다.

```javascript
<React.Suspense fallback={<Loader />}>
  <MyComponent />
</React.Suspense>
```
    
### 에러 처리와의 통합
추가적으로 Suspense와 ErrroBoundary를 활용함으로, 비동기 처리 실패 시에 대한 기능에 



## Suspense 도입의 고려 사항

Suspense를 도입하려면 기존 코드베이스에 몇 가지 중요한 변경 작업이 필요합니다.

1. **ErrorBoundary 설정**  
    Suspense는 비동기 작업에서 에러를 처리하기 위해 반드시 ErrorBoundary와 함께 사용해야 합니다. 이를 통해 비동기 작업 실패 시 graceful fallback을 제공할 수 있습니다.
    
2. **Suspense fallback 구성**  
    로딩 중 상태를 보여주기 위한 fallback UI를 정의해야 합니다. 예를 들어, `<Suspense fallback={<LoadingSpinner />}>`를 사용하여 로딩 상태를 명시적으로 표현합니다.
    
3. **성능 및 사용자 경험**  
    Suspense를 사용하면 비동기 로직이 간소화되지만, fallback UI를 적절히 설계하지 않으면 사용자 경험에 부정적인 영향을 줄 수 있습니다. 이를 위해 성능 최적화 및 로딩 시간을 최소화하는 작업이 필요합니다.

4. React 18 버전 이상
	 기본적으로 Suspense 로직이 React 18버전 이상부터 도입되었기에 이를 사용하기 위해서는 React 18이 필수적입니다. 혹여나 레거시 프로젝트에 적용한다면 먼저 React를 올려둬야합니다.

