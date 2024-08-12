# ErrorBoundaries

참고 : https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/error_boundaries/

React는 컴포넌트 기반의 라이브러리이며, 때때로 개발 중이나 운영 중에 예상치 못한 에러가 발생할 수 있습니다. 이런 에러들을 적절히 처리하지 않으면 애플리케이션 전체가 크래시될 수 있습니다. 에러 바운더리(Error Boundary)는 이러한 에러를 포착하고 백업 UI를 제공함으로써 앱의 나머지 부분이 정상적으로 작동할 수 있도록 도와주는 React 컴포넌트입니다.

예를 들어, UserProfile 컴포넌트가 사용자 데이터를 불러와서 화면에 표시합니다. 하지만 때때로 서버에서 데이터를 가져오는 과정에서 문제가 발생할 수 있고, 이로 인해 UserProfile 컴포넌트가 예상치 못한 에러를 발생시킬 수 있습니다.

```js
import React, { useState, useEffect } from 'react';

function UserProfile() {
  const [user, setUser] = useState(null);
  const [error, setError] = useState(false);

  useEffect(() => {
    fetch('/api/user')
      .then(response => response.json())
      .then(data => setUser(data))
      .catch(err => {
        setError(true);
        console.error('Failed to fetch user:', err);
      });
  }, []); // 빈 배열을 넘겨서 컴포넌트 마운트 시에만 실행되도록 함

  if (error) {
    throw new Error('Failed to load user profile');
  }

  if (user === null) {
    return <div>Loading user profile...</div>;
  }

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
    </div>
  );
}
```
 UserProfile 컴포넌트 내에서 에러가 발생하면 (예를 들어, 서버로부터 데이터를 가져오는 데 실패했을 때), 이 컴포넌트는 에러를 throw합니다. 
 
 ```js
 function App() {
  return (
    <ErrorBoundary>
      <UserProfile />
    </ErrorBoundary>
  );
}
```
 실제로 ErrorBoundary를 감싸주는 것으로 이에 대한 에러를 포착하게 됩니다.

 이에 대해서 ErrorBoundary는 실제로 사용할때에는 커스텀을 권장합니다.

```js
import React, { Component } from 'react';

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 에러 발생 시 상태를 업데이트하여 다음 렌더링에서 대체 UI를 보여줄 수 있게 함
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 에러 로깅이나 리포팅을 여기서 할 수 있음
    console.error('ErrorBoundary caught an error', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 에러 발생 시 보여줄 UI
      return <h1>Something went wrong.</h1>;
    }

    // 에러가 없으면 자식 컴포넌트를 정상 렌더링
    return this.props.children;
  }
}
 ```

 왜 클래스를 사용할까?

## Class 형식이 필요한 이유
* Class 형식이 필요한 이유가 무엇인가에 대해 간단히 설명하자면, 한마디로 Hooks에서 지원하는 방식으로는 오류 발생 시 제어할 방법이 없어서 입니다.😨
  * getSnapshotBeforeUpdate: 가장 마지막으로 렌더링 된 결과가 DOM 등에 반영되었을 때 호출
  * getDerivedStateFromError: 하위의 자손 컴포넌트에서 오류가 발생했을 때 render 단계에서 호출
  * componentDidCatch: 하위의 자손 컴포넌트에서 오류가 발생했을 때 commit 단계에서 호출

위의 세 가지 라이프 사이클이 아직은 Hooks에서 구현되지 않았기 때문입니다.

