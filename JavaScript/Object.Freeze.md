## Object.Freeze

우테코 프리코스를 하며 사람들의 코드를 둘러보며 리뷰하더 도중 이러한 코드를 발견할 수 있었다.

```jsx
cosnt 상수 = Object.freeze({
  a: 'A',
	...//
})
```

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