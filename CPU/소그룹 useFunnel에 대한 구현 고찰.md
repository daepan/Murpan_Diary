* 기존에 우리가 구현했던 useFunnel
```js
import { ReactElement, useState } from 'react';

export interface StepProps {
  name: string;
  children: ReactElement;
}

export interface FunnelProps {
  children: ReactElement<StepProps>[];
}

/**
* 단계별 입력 폼을 진행할 시 사용 권장
* @param { string } defaultStep 시작할 초기 단계
* @returns { Funnel, Step, setStep, currentStep } 단계별 입력 폼을 관리하는데 사용하는 유틸리티
*/
export const useFunnel = (defaultStep: string) => {
  const [step, setStep] = useState(defaultStep);

  const Step = (props: StepProps) => props.children;

  function Funnel({ children }: FunnelProps): ReactElement | null {
    const targetStep = children.find((childStep) => childStep.props.name === step);

    return targetStep ?? null;
  }

  return {
    Funnel, Step, setStep, currentStep: step,
  } as const;
};
```

### 우리들의 useFunnel의 구현 방식:

1. **기본 단계 관리**: `useState` 훅을 사용하여 현재 단계를 관리합니다.
2. **컴포넌트**:
    - **Step**: 내용을 감싸는 컴포넌트.
    - **Funnel**: 현재 단계와 일치하는 `Step` 컴포넌트를 렌더링합니다.
3. **상태 관리**: `setStep`을 통해 현재 단계를 변경할 수 있습니다.


## Toss의 useFunnel

[Github 라이브러리 링크](https://github.com/toss/slash/blob/main/packages/react/use-funnel/src/useFunnel.tsx)

### Toss의 useFunnel의 구현 방식

### 주요 기능:

1. **고급 단계 관리**: URL 쿼리 파라미터를 통해 현재 단계를 관리하여, 브라우저 네비게이션과 단계 상태가 동기화됩니다.
2. **컴포넌트**:
    - **FunnelComponent**: 쿼리 파라미터를 기반으로 현재 단계를 결정하고 해당 단계를 렌더링합니다.
    - **Step**: 기본 구현과 동일하게 내용을 감싸는 컴포넌트.
3. **상태 관리 및 저장소**:
    - `useFunnelState`: 상태를 로컬 저장소(기본적으로 `sessionStorage`)에 저장하여 페이지 새로고침 후에도 상태를 유지합니다.
    - `createFunnelStorage`: 다른 저장소로 쉽게 변경할 수 있도록 유연하게 설계되었습니다.
4. **유틸리티 함수**:
    - `setStep`: 현재 단계를 설정하고 URL을 업데이트합니다.
    - `withState`: 초기 상태를 설정하고 상태와 단계를 함께 관리할 수 있게 합니다.


### 차이점:

1. **복잡성**:    
    - **우리들의 구현**은 간단히 말해서 단순하며, `state`를 메모리에만 저장합니다.
    - **Toss 구현**은 상태를 URL과 로컬 저장소에 저장하여 복잡한 네비게이션 및 상태 관리 기능을 제공합니다.

2. **유연성**:    
    - **우리들의 구현**은 단순한 폼이나 프로세스에 적합합니다.
    - **Toss의 구현**은 복잡한 애플리케이션에 적합하며, 여러 유틸리티 함수와 상태 저장 기능을 포함합니다.

3. **상태 유지**:
    - **우리들의 구현**은 페이지 새로고침 후 상태를 유지하지 않습니다.
    - **Toss의 구현**은 상태를 저장소에 저장하여 페이지 새로고침 후에도 상태를 유지합니다.

## 고찰

물론 Toss와의 구현 격차는 당연히 n년차 개발자와 구현 수준을 비교하는 것은 의미가 없다. 하지만 이것을 통해서 우리의 코드를 추가적으로 개선할 하나의 방향을 구할 수 있다.
일단 현재 코드의 문제점을 꼽자면 "state를 통한 상태관리가 불완전하다"를 알 수 있다. 하지만 이러한 문제를 해결하는 방법을 많이 고민해보았고 Toss와의 해결할 방법을 쿼리파라미터를 통해 이를 해결해보겠다.

```js
import { ReactElement, useState, useEffect } from 'react';
import { useRouter } from 'next/router';

export interface StepProps {
  name: string;
  children: ReactElement;
}

export interface FunnelProps {
  children: ReactElement<StepProps>[];
}

interface SetStepOptions {
  stepChangeType?: 'push' | 'replace';
  preserveQuery?: boolean;
  query?: Record<string, any>;
}

const DEFAULT_STEP_QUERY_KEY = 'funnel-step';

export const useFunnel = (defaultStep: string) => {
  const router = useRouter();
  const [step, setStep] = useState<string>(() => {
    // URL에서 현재 단계를 가져오거나 기본 단계를 사용
    return (router.query[DEFAULT_STEP_QUERY_KEY] as string) || defaultStep;
  });

  useEffect(() => {
    // URL 쿼리 파라미터가 변경되면 단계 상태를 업데이트
    const handleRouteChange = (url: string) => {
      const query = new URLSearchParams(url.split('?')[1]);
      const newStep = query.get(DEFAULT_STEP_QUERY_KEY) || defaultStep;
      setStep(newStep);
    };

    router.events.on('routeChangeComplete', handleRouteChange);
    return () => {
      router.events.off('routeChangeComplete', handleRouteChange);
    };
  }, [router, defaultStep]);

  const updateStep = (newStep: string, options?: SetStepOptions) => {
    const { preserveQuery = true, query = {} } = options || {};

    const newQuery = {
      ...(preserveQuery ? router.query : {}),
      ...query,
      [DEFAULT_STEP_QUERY_KEY]: newStep,
    };

    const url = {
      pathname: router.pathname,
      query: newQuery,
    };

    switch (options?.stepChangeType) {
      case 'replace':
        router.replace(url, undefined, { shallow: true });
        break;
      case 'push':
      default:
        router.push(url, undefined, { shallow: true });
        break;
    }

    setStep(newStep);
  };

  const Step = (props: StepProps) => props.children;

  function Funnel({ children }: FunnelProps): ReactElement | null {
    const targetStep = children.find((childStep) => childStep.props.name === step);
    return targetStep ?? null;
  }

  return {
    Funnel, Step, setStep: updateStep, currentStep: step,
  } as const;
};
```

### 주요 기능:

1. **URL 쿼리 파라미터와 동기화**: 현재 단계를 URL 쿼리 파라미터로 관리하여 브라우저 네비게이션과 동기화합니다.
2. **기존 상태 유지**: `useState`와 `useEffect` 훅을 사용하여 초기 상태를 설정하고, URL 변경에 따라 상태를 업데이트합니다.
3. **유연한 단계 전환**: `setStep` 함수는 `push` 또는 `replace` 옵션을 사용하여 URL을 업데이트하고, 필요한 경우 추가 쿼리 파라미터를 유지합니다.

이 구현은 기본 구현의 간결함을 유지하면서도 네비게이션 동기화와 유연성을 추가하여 더 많은 사용 사례에 적합하게 만들었습니다. 이렇게 하면 단순한 폼과 복잡한 애플리케이션 모두에 유용할 수 있습니다.

하지만 여기서 멈춘다면 파라미터를 언젠가 분석당해서 예기치 못한 방식으로의 Funnel의 해킹의 여지가 있습니다.

이러한 기능을 막기 위해 valid를 추가해서 적절한 useFunnel을 구현해보겠습니다.

```js
import { ReactElement, useState, useEffect, useCallback } from 'react';
import { useRouter } from 'next/router';

export interface StepProps {
  name: string;
  children: ReactElement;
}

export interface FunnelProps {
  children: ReactElement<StepProps>[];
}

interface SetStepOptions {
  stepChangeType?: 'push' | 'replace';
  preserveQuery?: boolean;
  query?: Record<string, any>;
}

const DEFAULT_STEP_QUERY_KEY = 'funnel-step';

export const useFunnel = (defaultStep: string, validSteps: string[]) => {
  const router = useRouter();
  const [step, setStep] = useState<string>(() => {
    const queryStep = router.query[DEFAULT_STEP_QUERY_KEY] as string;
    return validSteps.includes(queryStep) ? queryStep : defaultStep;
  });

  useEffect(() => {
    const handleRouteChange = (url: string) => {
      const query = new URLSearchParams(url.split('?')[1]);
      const newStep = query.get(DEFAULT_STEP_QUERY_KEY);
      if (newStep && validSteps.includes(newStep)) {
        setStep(newStep);
      } else {
        setStep(defaultStep);
      }
    };

    router.events.on('routeChangeComplete', handleRouteChange);
    return () => {
      router.events.off('routeChangeComplete', handleRouteChange);
    };
  }, [router, defaultStep, validSteps]);

  const updateStep = useCallback((newStep: string, options?: SetStepOptions) => {
    if (!validSteps.includes(newStep)) return;

    const { preserveQuery = true, query = {} } = options || {};
    const newQuery = {
      ...(preserveQuery ? router.query : {}),
      ...query,
      [DEFAULT_STEP_QUERY_KEY]: newStep,
    };

    const url = {
      pathname: router.pathname,
      query: newQuery,
    };

    switch (options?.stepChangeType) {
      case 'replace':
        router.replace(url, undefined, { shallow: true });
        break;
      case 'push':
      default:
        router.push(url, undefined, { shallow: true });
        break;
    }

    setStep(newStep);
  }, [router, validSteps]);

  const Step = (props: StepProps) => props.children;

  function Funnel({ children }: FunnelProps): ReactElement | null {
    const targetStep = children.find((childStep) => childStep.props.name === step);
    return targetStep ?? null;
  }

  return {
    Funnel, Step, setStep: updateStep, currentStep: step,
  } as const;
};
```
