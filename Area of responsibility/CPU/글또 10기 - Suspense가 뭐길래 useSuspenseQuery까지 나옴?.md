## 들어가며
최근에 한기대 코인 프로젝트를 진행하며 API 로직에 대한 변화가 있었습니다. tanStack-Query를 통해 useQuery로 API를 관리하고 있었는데, useSuspenseQuery로 변경하는 작업을 진행하고 있습니다.
처음에는 이 useSuspenseQuery가 뭐지에 대해 고민하다 Suspense를 제가 실제로 많이 다룬 경험이 없더군요. 그래서 한번 Suspense 깔끔하게 정리하면서 useQuery보다 useSuspenseQuery를 도입하게 된 이유를 서술하면서 이번 글을 마무리해보려 합니다.

## What is Suspense?

React의 **Suspense**는 데이터 로딩, 코드 분할 등과 같은 비동기 작업을 보다 직관적이고 깔끔하게 처리하기 위해 고안된 기능입니다. 기존의 로딩 상태 관리 방식은 종종 복잡한 조건문과 state 관리를 요구했으나, Suspense는 이러한 문제를 단순화합니다.

- **Suspense의 주요 역할**
    1. 컴포넌트가 비동기 작업을 완료할 때까지 UI 렌더링을 유예.
    2. 로딩 상태를 컴포넌트 수준에서 간결하게 처리 가능.
    3. 다양한 데이터 소스 및 동적 import를 한 곳에서 관리.

- **기존 방식과의 차이점**  
    기존에는 `isLoading`이나 `isFetching` 같은 상태 플래그를 사용해 데이터가 준비되었는지 확인하고 UI를 조건부 렌더링했습니다. 반면, Suspense는 이 모든 과정을 단순화해 fallback 컴포넌트를 통해 로딩 상태를 처리할 수 있습니다.

# useQuery vs. useSuspenseQuery

React-Query의 `useQuery`와 `useSuspenseQuery`는 비슷한 목적을 가진 훅이지만, 사용 방식과 처리 방식에서 큰 차이가 있습니다.

- **useQuery**  
    `useQuery`는 로딩 상태, 에러 상태, 데이터 상태를 개별적으로 다룹니다. 사용자가 `isLoading`, `isError`와 같은 플래그를 직접 관리해야 하기 때문에 로직이 복잡해질 수 있습니다.
    
- **useSuspenseQuery**  
    `useSuspenseQuery`는 React의 Suspense와 결합되어 로딩 및 에러 처리를 더 직관적으로 할 수 있게 합니다. 로딩 상태와 에러 처리가 Suspense와 ErrorBoundary로 위임되므로, 컴포넌트 내부의 로직이 간소화됩니다.
    
- **장단점 비교**
    - **useQuery**: 세부적인 상태 제어가 가능하지만 코드 복잡도가 높아질 수 있음.
    - **useSuspenseQuery**: 코드가 깔끔하고 관리가 쉬워지지만, Suspense에 대한 추가 학습이 필요.

Suspense를 도입하면서 더 간결한 코드와 일관된 상태 관리를 기대할 수 있었기에 `useSuspenseQuery`로 전환을 결정하게 되었습니다.

---

# Suspense 도입의 고려 사항

Suspense를 도입하려면 기존 코드베이스에 몇 가지 중요한 변경 작업이 필요합니다.

1. **ErrorBoundary 설정**  
    Suspense는 비동기 작업에서 에러를 처리하기 위해 반드시 ErrorBoundary와 함께 사용해야 합니다. 이를 통해 비동기 작업 실패 시 graceful fallback을 제공할 수 있습니다.
    
2. **Suspense fallback 구성**  
    로딩 중 상태를 보여주기 위한 fallback UI를 정의해야 합니다. 예를 들어, `<Suspense fallback={<LoadingSpinner />}>`를 사용하여 로딩 상태를 명시적으로 표현합니다.
    
3. **성능 및 사용자 경험**  
    Suspense를 사용하면 비동기 로직이 간소화되지만, fallback UI를 적절히 설계하지 않으면 사용자 경험에 부정적인 영향을 줄 수 있습니다. 이를 위해 성능 최적화 및 로딩 시간을 최소화하는 작업이 필요합니다.

4. React 18 버전 이상
	 기본적으로 Suspense 로직이 React 18버전 이상부터 도입되었기에 이를 사용하기 위해서는 React 18이 필수적입니다. 혹여나 레거시 프로젝트에 적용한다면 먼저 React를 올려둬야합니다.

