## 서론
이전에 제작했던 Portal을 통해 이미지를 처리한 기억이 있었는데 해당 부분에서 최근 사장님 서비스를 출시하면서 이미지의 안정성이 우리쪽에서 올리는 것이 아닌 클라이언트가 생겨버렸다. 그렇다면 이러한 이미지가 혹시라도 깨져버린다면? 현재는 이를 막을 방법이 없을 것이다. 실제로 이미지 업로드 방식이 변경되면서 이미지가 여럿 깨지는 현상이 있었는데 미리 크러쉬 이미지가 나오는 것을 막고 에러 이미지를 통해 이를 확인할 수 있는 방법을 생각하게 되었습니다.

## 이미지 에러 추적문제

근본적으로 이미지가 깨졌을 때 에러 이미지를 찾고 싶은 것인데 그렇다면 이미지의 에러를 확인하고 싶다.
현재 방식에서는 `<img />` 를 활용해서 보여주고 있는데 에러를 추적하기 위해 태그 내부에 있는 `onerror`를 통해 추적할 수 있습니다.

해당 내용은 아래와 같습니다

> 이미지를 로드하거나 렌더링하는 동안 오류가 발생하고 오류 이벤트에 대한 onerror 이벤트 핸들러가 설정된 경우, 해당 이벤트 핸들러가 호출됩니다. 이는 여러 상황에서 발생할 수 있으며, 다음을 포함합니다:
> 
> - src 속성이 비어 있거나(`""`) `null`입니다.
> - src URL이 사용자가 현재 방문 중인 페이지의 URL과 동일합니다.
> - 이미지가 로드되지 않게 하는 방식으로 손상되었습니다.
> - 이미지의 메타데이터가 그 차원을 검색할 수 없을 정도로 손상되었고, `<img>` 요소의 속성에 차원이 지정되지 않았습니다.
> - 이미지가 사용자 에이전트에서 지원하지 않는 형식입니다.
> refrence: [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#image_loading_errors)


그렇다면 여기서 onError의 throw를 통해 ErrorBoundary를 통해 에러를 캐치하여 해당 문제를 해결하려했습니다. 
```tsx
<ErrorBoundary fallbackClassName="store-detail-image">
  <img 
	className={styles.image}
	src="" alt="상점 이미지"
	onError={(e) => { 
	  if(e.type === 'error') 
	    throw new Error('Image load error'); 
	  }
	}
  />
</ErrorBoundary>
```

그러나...
![[스크린샷 2024-04-10 오후 6.18.58.png]]


저의 예상에서는 ErrorBoudary 코드에서 throw Error를 캐치하여 <img> `<img>`를 대체하여 ErrorBoundary에서 제공되는 에러를 표시하는 컴포넌트를 반환하고 싶었습니다.

```tsx
import React, { ErrorInfo } from 'react';
import showToast from 'utils/ts/showToast';
import { AxiosError } from 'axios';

interface Props {
  fallbackClassName: string;
  children: React.ReactNode;
}

interface State {
  hasError: boolean;
}

function isAxiosError(error: AxiosError<any, any> | Error): error is AxiosError<any, any> {
  return ('response' in error);
}

export default class ErrorBoundary extends React.Component<Props, State> {
  constructor(props: Props) {
    super(props);
    // 이후에 사용시 해제
    // eslint-disable-next-line react/no-unused-state
    this.state = { hasError: false } as State;
  }

  // 이후에 사용시 해제
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  static getDerivedStateFromError(_: Error) {
    return { hasError: true };
  }

  // 이후에 사용시 해제
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  componentDidCatch(error: AxiosError<any, any> | Error, __: ErrorInfo) {
    showToast('error', isAxiosError(error) ? error.response?.data.error.message : error.message);
  }

  render() {
    const { children, fallbackClassName } = this.props;
    const { hasError } = this.state;
    if (hasError) {
      return (
        <div className={fallbackClassName}>
          Error
        </div>
      );
    }
    return children;
  }
}
```

![[Pasted image 20240410182603.png]]

하지만 실제로는 이렇습니다.
`<img>` 태그의 `onError` 이벤트를 `ErrorBoundary`가 캐치하지 못하는 이유는, `onError` 이벤트가 비동기적인 이벤트 리스너 내에서 발생하기 때문입니다. 

React의 `ErrorBoundary`는 컴포넌트 렌더링 과정 또는 컴포넌트 라이프사이클 메소드 실행 중에 동기적으로 발생하는 오류를 캐치합니다. 반면, `onError` 이벤트와 같은 이벤트 리스너에서 발생하는 오류는 이벤트 처리 메커니즘이 브라우저의 비동기 이벤트 시스템을 통해 처리되므로, React의 라이프사이클 메소드(`componentDidMount` 포함)와는 별개로 동작합니다.

### 동기 vs 비동기 오류

- **동기 오류**: React 컴포넌트의 `render` 메소드나 `componentDidMount`, `componentDidUpdate` 등의 라이프사이클 메소드 내에서 발생하는 오류는 동기적으로 처리됩니다. 이러한 오류는 `ErrorBoundary`에 의해 캐치되어 처리될 수 있습니다.
- **비동기 오류**: `onError` 이벤트 핸들러와 같은 이벤트 리스너에서 발생하는 오류, 또는 `setTimeout`, `Promise` 등의 비동기 작업에서 발생하는 오류는 `ErrorBoundary`가 캐치할 수 없습니다. 이는 React가 이러한 비동기 작업에서 발생한 오류를 자동으로 캐치할 수 없기 때문입니다.

### `onError` 이벤트의 동작 방식

`<img>` 태그의 `onError` 이벤트는 이미지 로딩이 실패했을 때 발생합니다. 예를 들어, 이미지 URL이 잘못되었거나, 네트워크 문제로 인해 이미지를 로드할 수 없는 경우에 발생할 수 있습니다. 이러한 `onError` 이벤트는 다음과 같이 사용됩니다:

### 결론
---
>`componentDidMount`와 같은 라이프사이클 메소드에서 `onError` 이벤트를 직접 관리하지 않으며, `onError` 이벤트는 이미지 로딩과 같은 비동기 이벤트의 결과에 대한 반응으로 동작합니다. 따라서 `ErrorBoundary`를 사용하여 이러한 비동기 이벤트에서 발생하는 오류를 직접 캐치하는 것은 불가능합니다.


## How to Solve?
그렇다면 이러한 이미지 에러를 추적하기 위해서는 어떻게 해야할까요?
img 태그에서 onError를 동기적으로 처리할 수 있도록 컴포넌트화를 시켜야하는 것으로 결론을 내렸습니다.

왜냐하면 img 동작 자체가 로딩이 리액트에서 감지하기 위해서는 리액트의 동기적 동작에 포함될 수 있게 컴포넌트로 넣게 되는 것을 생각하였습니다.

최종적으로 img에 대한 onError를 추적하는 코드는 아래와 같습니다.

```tsx
import React, { useState } from 'react';

interface ImageErrorBoundaryProps {
  src: string;
  alt: string;
  className: string;
  fallback: React.ReactNode;
}

export default function ImageErrorBoundary({
  src,
  alt,
  className,
  fallback,
}: ImageErrorBoundaryProps) {
  const [hasError, setHasError] = useState(false);

  return (
    <div>
      {hasError ? (
        <div className={className}>
          {fallback}
        </div>
      ) : (
        <img
          src={src}
          alt={alt}
          className={className}
          onError={() => setHasError(true)}
        />
      )}
    </div>
  );
}

```

이를 실제로 코드를 적용시키고 확인한 결과 값입니다.
```tsx
---
  <ImageErrorBoundary
    className={styles.image}
    src=""
    fallback={<ErrorDisplay />}
    alt="상점이미지"
  />
---
```
![[Pasted image 20240410230329.png]]