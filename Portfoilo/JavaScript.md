### ES2015+의 주요 기능

#### 1. `const`와 `let`

- `const`와 `let`은 블록 스코프를 가집니다.
    - **`const`**: 한 번 대입하면 다른 값으로 재할당할 수 없습니다.
    - **`let`**: 값을 계속 수정할 수 있습니다.
#### 2. 템플릿 문자열

- 백틱(`)을 사용하여 문자열을 만들고 변수도 포함할 수 있습니다.

#### 3. 객체 리터럴

- 객체의 메서드를 간결하게 작성할 수 있습니다.
- 동적으로 속성명을 생성할 수 있습니다.

#### 4. 화살표 함수

- 간결한 함수 표현 방식으로, `this` 바인딩이 다릅니다.

#### 5. 비구조화 할당

- 객체나 배열에서 속성이나 요소를 추출하여 변수에 할당할 수 있습니다.

#### 6. 프로미스 (Promise)

- 비동기 작업을 처리하기 위한 객체입니다.

#### 7. async/await

- 프로미스를 더 간결하게 사용할 수 있게 해주는 문법입니다.


### 자바스크립트의 데이터 타입

자바스크립트의 데이터 타입은 크게 세 가지로 나눌 수 있습니다:

1. **Primitive Type**
2. **Structural Type**
3. **Structural Root Primitive**

#### 1. Primitive Type

- **undefined**: `typeof instance === 'undefined'`
- **Boolean**: `typeof instance === 'boolean'`
- **Number**: `typeof instance === 'number'`
- **String**: `typeof instance === 'string'`
- **BigInt**: `typeof instance === 'bigint'`
- **Symbol**: `typeof instance === 'symbol'`

**특징**:

- 기본 데이터 타입으로, 값 자체가 변하지 않음 (immutable).
- 각 타입에 대해 다음과 같은 특징이 있습니다:
    - **Number**: 모든 숫자는 double-precision 64-bit binary 형식을 따릅니다.
        
    - **BigInt**: Number 타입의 범위를 넘어가는 숫자를 저장하고 연산할 수 있습니다.
        
    - **Symbol**: 고유하고 변하지 않는 값으로, 주로 객체의 유일한 프로퍼티 키를 만들기 위해 사용됩니다.

        

#### 2. Structural Types

- **Object**: `typeof instance === 'object'`
- **Function**: `typeof instance === 'function'`

**특징**:

- 복합 데이터 타입으로, 여러 값을 포함할 수 있습니다.
- 객체는 속성과 메서드를 가질 수 있습니다.

#### 3. Structural Root Primitive

- **null**: `typeof instance === 'object'`

**특징**:

- 객체의 비어있음을 나타내는 특수 값입니다.
- `typeof null`이 `'object'`인 것은 자바스크립트의 초기 설계 오류로 인한 것입니다.

### 데이터 타입의 필요성

데이터 타입이 필요한 이유는 다음과 같습니다:

1. **메모리 공간 크기 결정**: 값을 저장할 때 확보해야 하는 메모리 공간의 크기를 결정합니다.
2. **값 참조 시 메모리 공간 크기 결정**: 값을 참조할 때 한 번에 읽어 들여야 할 메모리 공간의 크기를 결정합니다.
3. **2진수 해석 방법 결정**: 메모리에서 읽어 들인 2진수를 어떻게 해석할지 결정합니다.

### 예시 설명

- **변수 선언**: 자바스크립트 엔진은 `score` 변수를 특정 주소 `addr1`에 할당하고, 초기 값으로 `undefined`를 저장합니다.
- **값 할당**: `score`에 `100`을 할당하면, 엔진은 `100`이 `number` 타입임을 인식하고, 새로운 주소 `addr2`에 8바이트의 메모리 공간을 확보하여 값을 저장합니다. `score`는 이제 `addr2`를 가리킵니다.
- **값 참조**: 값을 참조할 때 엔진은 `number` 타입임을 알고, 8바이트를 읽어들여 값을 반환합니다.

이러한 과정은 자바스크립트 엔진이 데이터 타입을 기반으로 메모리 관리와 데이터 처리 방식을 결정하는 원리를 보여줍니다.