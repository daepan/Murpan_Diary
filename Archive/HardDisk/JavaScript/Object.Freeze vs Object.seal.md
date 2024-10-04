refrence : [https://ui.toast.com/posts/ko_20220420](https://ui.toast.com/posts/ko_20220420)

우테코 프리코스를 하며 사람들의 코드를 둘러보며 리뷰하더 도중 이러한 코드를 발견할 수 있었다.

```jsx
cosnt 상수 = Object.freeze({
  a: 'A',
	...//
})
```

## 서론

그래서 상수처리하는데 왜 저게 필요한 것이지? 라는 생각과 왜 저런 코드를 작성할까를 궁금해서 탐색을 하게 되었다.

먼저 Object.freeze() 란 무엇인가에 대해 알아보았는데

> The **`Object.freeze()`**method *freezes* an object. Freezing an object [prevents extensions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
 and makes existing properties non-writable and non-configurable. A frozen object can no longer be changed: new properties cannot be added, existing properties cannot be removed, their enumerability, configurability, writability, or value cannot be changed, and the object's prototype cannot be re-assigned. `freeze()`
 returns the same object that was passed in.
reference: [MDN-Object.freeze]([https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze))
> 

해석하면

**`Object.freeze()` 메서드는 객체를 고정합니다. 
⇒** 즉, 개발자가 생성한 상수 객체를 고정하겠다고 선언하는 과정,

**개체를 고정하면 확장이 방지되고 기존 속성을 쓸 수 없고 구성할 수 없게 됩니다. 고정된 개체는 더 이상 변경할 수 없습니다. 새 속성을 추가할 수 없고 기존 속성을 제거할 수 없으며 열거 가능성, 구성 가능성, 쓰기 가능성 또는 값을 변경할 수 없으며 개체의 프로토타입을 다시 할당할 수 없습니다.
⇒ 자바스크립트에서 객체의 경우에는 `const`를 해도 변경이 가능합니다.
-  이유는** `const` 에 `object` 를 할당할 경우, memory heap 에 object 를 위한 메모리를 만들고 해당 주소값을 const 식별자에 할당합니다. 즉, 해당 주소값은 변하진 않지만 참조하고 있는 object 는 값을 변화시킬 수 있습니다.

한마디로 자바스크립트 객체에서의 상수이지만 값이 변경되는 경우를 염려하여 이와 같은 freeze를 활용하여 처리할 수 있다는 것을 알 수 있었다

아하 그렇구나 이해하고 넘어가려던 중!
TOAST UI 팀에서 작성된 `Object.freeze()` 와 `Object.seal()` 이라는 내용의 대결을 보고서는 이에 대해 나의 지식으로 정리하고자 이렇게 글을 쓰게 됩니다. (출처는 상단에 표시하였습니다.)

## Object.seal()?

seal이라는 의미는 편지를 봉한다는 의미를 지니는데, 전달 받은 객체의 모든 속성을 구성할 수 없게(`non-configurable`) 만든다

```jsx
const obj = { a: 100 };
Object.getOwnPropertyDescriptors(obj);
/* {
 *   a: {
 *        configurable: true,
 *        enumerable: true,
 *        value: 100,
 *        writable: true
 *      }
 * }
 *
 */

Object.seal(obj);
Object.getOwnPropertyDescriptors(obj);

/* {
 *   a: {
 *        configurable: false,
 *        enumerable: true,
 *        value: 100,
 *        writable: true
 *      }
 * }
 *
 */
```

위의 예시해서 보면 `configurable`이 false로 변경된 것을 확인할 수 있다.

그렇다면 seal을 해두면은 개발자는 무엇이 제한되는 것인가?

```jsx
// 1. 프로퍼티를 변경하는 경우
obj.a = 200;
console.log(obj.a);
// 200

// 2. 프로퍼티를 제거하는 경우
delete obj.a;
console.log(obj.a);
// 200

// 3. 새로운 프로퍼티를 추가하는 경우
obj.b = 500;
console.log(obj.b);
// undefined
```

위의 1번을 제외한 나머지 동작들은 실행되지 않고 1번만 동작하게 되었다.

그렇다면 왜 1번만 이루어진 것인가?

봉인된 객체의 `configurable`이 `false`가 되어도 기존 속성값은 `200`으로 변경되게 된다. 앞서 설명했듯 `configurable`
을 `false`로 설정하면 속성을 쓸 수 없도록 만들어지지만 `writable`이 명시적으로 `true`면 큰 상관 없이 변경이 가능하다.

## 그렇다면 이 둘의 차이는?

위의 내용들을 정리하면 `Object.freeze()` 와 `Object.seal()`의 기능상 목적은 동일하다. 그렇다면 왜 굳이 2개가?

이 둘의 차이점을 설명해보겠다.

`Object.freeze`와 `Object.seal` 중 `Object.freeze`가 보다 제한이 더 크게 걸린다.

```jsx
const obj = { a: 100 };
Object.getOwnPropertyDescriptors(obj);
/* {
 *   a: {
 *        configurable: true,
 *        enumerable: true,
 *        value: 100,
 *        writable: true
 *      }
 * }
 *
 */
Object.freeze(obj);
Object.getOwnPropertyDescriptors(obj);
/* {
 *   a: {
 *        configurable: false,
 *        enumerable: true,
 *        value: 100,
 *        writable: false
 *      }
 * }
 *
 */
```

위의 내용에서 보면 `configurable` 뿐만아니라 `writable`이 `false`로 변경된 것을 확인할 수 있다.

```jsx
// 1. 프로퍼티를 변경하는 경우
obj.a = 200;
console.log(obj.a);
// 100

// 2. 프로퍼티를 제거하는 경우
delete obj.a;
console.log(obj.a);
// 100

// 3. 새로운 프로퍼티를 추가하는 경우
obj.b = 500;
console.log(obj.b);
// undefined
```

물론 기능 예시의 기능의 경우를 보면 위에서 1번을 포함한 모든 기능이 작동하지 않는다. 이 결과를 통해 `Object.freeze()`를 `writable` 속성을 `false`로 변경시키기 때문에 `seal` 보다 더 엄격하다고 할 수 있다.

## `Object.freeze`와 `Object.seal` 정리

### 공통점

1. 전달된 객체들은 확장할 수 없게 되어 새로운 속성을 추가할 수 없다.
2. 전달된 객체 내부의 모든 요소들은 구성 불가능하게 되어 삭제할 수 없다.
3. 스트릭트 모드에서 두 메서드를 사용한 객체 모두 `obj.b = 500` 같이 프로퍼티를 추가하는 명령어의 경우에는 `undefined`성립하지 않는다. 

### 차이점

1. `Object.seal`은 프로퍼티를 수정할 수 있지만 `Object.freeze`는 그렇지 않다.

### 결함

이 두가지 메서드에서 목적과 맞지 않게 결함이 존재하는데, 바로 중첩된 객체의 경우에 제대로 이루어지지 않는다는 것이다.

```jsx
const obj = {
  foo: {
    bar: 10
  }
};

Object.seal(obj);
```

위 와 같은 중첩된 객체를 seal을 통해 잠금했을 때 서술자는 이렇게 변경된다.

```jsx
Object.getOwnPropertyDescriptors(obj);
/* {
 *   foo: {
 *        configurable: false,
 *        enumerable: true,
 *        value: {bar: 10},
 *        writable: false
 *      }
 * }
 */

Object.getOwnPropertyDescriptors(obj.foo);
/* {
 *   bar: {
 *        configurable: true,
 *        enumerable: true,
 *        value: 10,
 *        writable: true
 *      }
 * }
 */
```

이 차이를 이해하셨나요? 그렇습니다. 개발자의 의도에서는obj를 seal로 봉하려 했지만 객체 내부에 있는 객체는 아무런 변화가 없음을 알 수 있다.

위와 같은 문제를 해결하기 위해 deepFreeze라는 방식을 추천하는데 코드는 아래와 같다.

```jsx
function deepFreeze(object) {

  // 객체에 정의된 속성의 이름을 조회한다.
  var propNames = Object.getOwnPropertyNames(object);

  // 자신을 프리징하기 전 속성을 프리징한다.
  for (let name of propNames) {
    let value = object[name];

    if(value && typeof value === "object") {
      deepFreeze(value);
    }
  }

  return Object.freeze(object);
}
```

### 결론

`Object.freeze`와 `Object.seal`은 확실히 객체를 봉인하여 불변성을 지켜준다는 장점이 있어 상수와 같이 `const`에서 객체의 불안함을 해결할 수 있는 새로운 메서드가 될 것이다. ****

하지만 중첩된 객체를 동결하기 위해선 `deepFreeze`를 사용해야 한다는 점을 반드시 고려해야 한다.