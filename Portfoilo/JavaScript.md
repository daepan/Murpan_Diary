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


### Object Prototype

#### 정의

- **Prototype**은 JavaScript 객체가 다른 객체로부터 상속하는 메커니즘입니다.
- JavaScript는 **prototype-based language**로, 상속을 지원하며 객체는 프로토타입 객체를 갖습니다.
- 프로토타입 객체는 메서드와 속성을 상속하는 템플릿 객체와 같습니다.
- 객체의 프로토타입 객체 또한 프로토타입 객체를 가지며, 이를 **prototype chain**이라고 부릅니다.

### 프로토타입 객체 이해하기


#### 객체 인스턴스 생성


#### 메서드 호출 예제


#### 동작 과정

1. **메서드 검색**: `person1` 객체가 `valueOf()` 메서드를 갖고 있는지 확인합니다.
    - `person1` 객체의 생성자인 `Person` 함수에 `valueOf()` 메서드가 정의되어 있는지 확인합니다.
2. **프로토타입 체인 탐색**: 만약 `person1` 객체에 `valueOf()` 메서드가 없다면, `person1`의 프로토타입 객체를 확인합니다.
    - 프로토타입 객체에 `valueOf()` 메서드가 없다면, 프로토타입 객체의 프로토타입 객체를 계속해서 확인합니다.
    - 이 과정은 프로토타입 객체가 `null`이 될 때까지 계속됩니다.

### Prototype Chain 예제


### Prototype 속성

- **`__proto__`**: 객체 인스턴스와 프로토타입 객체 사이의 연결을 나타내는 속성입니다.
- **`prototype`**: 생성자 함수의 프로토타입 속성으로, 생성된 모든 객체 인스턴스는 이 프로토타입을 상속받습니다.

### 정리

- **Prototype**은 JavaScript 객체가 다른 객체로부터 메서드와 속성을 상속받는 메커니즘입니다.
- **Prototype Chain**은 객체의 프로토타입을 따라가며 메서드와 속성을 찾는 과정입니다.
- 객체 인스턴스는 생성자 함수의 **`prototype`** 속성을 통해 프로토타입 객체에 연결됩니다.
- **메서드 호출 시** 객체에서 메서드를 찾지 못하면 프로토타입 체인을 따라가며 메서드를 탐색합니다.

이와 같이 JavaScript의 프로토타입 메커니즘을 이해하면 객체 상속과 메서드 탐색 과정을 명확히 알 수 있습니다.


### 클로저 (Closure)

#### 정의

- 클로저는 함수와 그 함수가 선언된 어휘적 환경(Lexical Environment)의 조합입니다.
- 클로저는 내부 함수가 외부 함수의 스코프에 접근할 수 있게 해줍니다.
- JavaScript에서 클로저는 함수가 생성될 때마다 만들어집니다.

#### Lexical Scoping

- **어휘적 스코핑**은 중첩된 함수에서 변수 이름이 확인되는 방식을 정의합니다.
- 내부 함수는 부모 함수가 반환된 후에도 부모 함수의 스코프에 접근할 수 있습니다.

##### 예제

```javascript
function init() {
  var name = 'Mozilla'; // name은 init의 로컬 변수
  function displayName() { // displayName()은 내부 함수, 클로저
    alert(name); // 부모 함수의 변수를 사용
  }
  displayName();
}
init();

```

#### 클로저의 활용

- 클로저를 이용하면 데이터와 함수를 연결하여 객체 지향 프로그래밍(OOP)과 유사한 패턴을 구현할 수 있습니다.

##### 예제

```javascript
function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
```
### 클로저를 이용한 비공개 메서드 구현

- JavaScript는 비공개 메서드를 구현하기 위한 네이티브 방법을 제공하지 않지만, 클로저를 통해 이를 구현할 수 있습니다.

##### 모듈 디자인 패턴 예제



### 클로저 스코프 체인

모든 클로저는 세 가지 스코프를 가집니다:

1. 로컬 스코프 (자신의 스코프)
2. 외부 함수 스코프
3. 전역 스코프

##### 예제



### 루프에서 클로저 생성 시의 흔한 실수

- 클로저가 루프 내에서 생성될 때 예상치 못한 동작을 할 수 있습니다.

##### 잘못된 예제

javascript

코드 복사

`function setupHelp() {   var helpText = [     { id: 'email', help: 'Your e-mail address' },     { id: 'name', help: 'Your full name' },     { id: 'age', help: 'Your age (you must be over 16)' }   ];    for (var i = 0; i < helpText.length; i++) {     var item = helpText[i];     document.getElementById(item.id).onfocus = function() {       showHelp(item.help);     }   } }`

- 위 코드는 모든 엘리먼트에서 마지막 아이템의 도움말을 표시합니다.

##### 해결 방법

1. **함수를 사용하여 새로운 스코프 생성**
    
2. **`let` 키워드 사용**

    

### 성능 고려사항

- 클로저가 필요하지 않을 때 클로저를 사용하면 메모리와 속도에 영향을 미칠 수 있습니다.
- 예를 들어, 새로운 객체/클래스를 만들 때 메서드는 객체의 생성자 대신 프로토타입에 정의하는 것이 좋습니다.

##### 예제

```javascript
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}
MyObject.prototype.getName = function() {
  return this.name;
};
MyObject.prototype.getMessage = function() {
  return this.message;
};

```

이와 같이 JavaScript의 클로저를 이해하면 함수와 변수의 스코프를 효과적으로 관리할 수 있으며, 더 나은 코드 구조를 설계할 수 있습니다.