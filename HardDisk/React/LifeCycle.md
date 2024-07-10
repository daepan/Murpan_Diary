React Life Cycle
총 3가지의 주기로 이루어져 있다.

 Mounting, Updating, Unmounting

## Mounting
Mounting이란 DOM에 요소를 붙이는 것을 말한다.
React는 컴포넌트를 마운팅할 때 순서대로 4가지 내장 메소드를 호출하게 된다.

### 1. constructor
constructor() 메소드는 컴포넌트가 초기화될 때 다른 어떤 메소드보다 먼저 호출되며, state 와 다른 초기 값들을 세팅한다.
constructor() 메소드는 props 라고도 불리며, super(props) 를 가장 먼저 호출해야 한다. super(props) 는 부모의 constructor 메소드를 초기화하고, 부모로 부터 상속받은 메소드들을 컴포넌트로 하여금 사용할 수 있도록 해준다.

### 2. getDerivedStateFromProps
getDerivedStateFromProps() 메소드는 DOM에서 요소들이 랜더링 되기 직전에 호출된다.
최초의 props 에 기반한 state 객체를 저장한다.
state 를 인자로 받고, state 가 변하면 객체를 반환한다.

### 3. render
render() 메소드는 필수값이며, DOM에 HTML을 표현해준다.

*4. componentDidMount*
componentDidMount() 메소드는 컴포넌트가 랜더링된 직 후에 호출된다.
이미 DOM에 위치한 컴포넌트를 필요로 하는 구문을 사용하는 곳이다.

## Updating
그 다음 단계인 Updating은 컴포넌트가 업데이트 할 때를 의미한다.
컴포넌트는 상태나 props가 변경될 때 마다 업데이트 된다
5가지 내장 메소드가 있고, 컴포넌트가 업데이트 되면 순서대로 실행된다.

### 1. getDerivedStateFromProps
update 단계에서 getDerivedStateFromProps 메소드가 호출된다. 이 메소드는 컴포넌트가 업데이트 될 때 제일 먼저 호출되는 메소드이다.
초기 props에 기반한 state 가 저장되는 곳이다.

### 2. shouldComponentUpdate
shouldComponentUpdate() 메소드는 React가 랜더링을 계속해야하는지 마는지에 대한 Boolean 값을 반환한다.
기본 값은 true이다.


### 3. render
컴포넌트가 업데이트 되면 역시 render() 도 호출된다. 새로운 변경점들을 갖고 리랜더링 된다

### 4. getSnapshotBeforeUpdate
getSnapshotBeforeUpdate() 메소드는 업데이트 되기 전의 props와 state에 접근할 수 있다. 즉, update 이후에도 update 이전의 값들을 확인할 수 있다는 것을 의미한다.
만약 getSnapshotBeforeUpdate() 메소드가 존재하면, componentDidUpdate() 메소드도 포함해야 한다. 그렇지 않으면 에러가 발생한다.

### 5.  componentDidUpdate
componentDidUpdate() 메소드는 컴포넌트가 DOM에서 update된 후에 호출된다.


## Unmounting
그 다음은 컴포넌트가 DOM을 제거하거나 Unmounting 할 때를 의미한다.
React는 unmount 될 때 에서 단 하나의 메장 내소드를 갖고 있다.

### componentWillUnmount
컴포넌트가 DOM에서 제거될 때 호출되는 메소드이다.
