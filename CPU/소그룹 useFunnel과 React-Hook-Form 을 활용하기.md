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
    
    - **우리들의 구현**은 단순하며, 상태를 메모리에만 저장합니다.
    - **고급 구현**은 상태를 URL과 로컬 저장소에 저장하여 복잡한 네비게이션 및 상태 관리 기능을 제공합니다.
2. **유연성**:
    
    - **기본 구현**은 단순한 폼이나 프로세스에 적합합니다.
    - **고급 구현**은 복잡한 애플리케이션에 적합하며, 여러 유틸리티 함수와 상태 저장 기능을 포함합니다.
3. **상태 유지**:
    
    - **기본 구현**은 페이지 새로고침 후 상태를 유지하지 않습니다.
    - **고급 구현**은 상태를 저장소에 저장하여 페이지 새로고침 후에도 상태를 유지합니다.