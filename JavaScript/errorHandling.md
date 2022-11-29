에러가 있는 스크립트를 작성했을 때 이를 중단하고 에러가 발생했다고 알려줄 수 있도록 만들 수 있다. 자바스크립트에서는 `try...catch`구문이나 `try...catch...finally`구문을 사용한다.

## try...catch 구문

```jsx
let json = "{ bad json }";

try {

  let user = JSON.parse(json); // <-- 여기서 에러가 발생하므로
  alert( user.name ); // 이 코드는 동작하지 않는다.

} catch (e) { // ES2019에서는 e를 생략해도 된다.
  // 에러가 발생하면 제어 흐름이 catch 문으로 넘어온다.
  alert( "데이터에 에러가 있어 재요청을 시도합니다." );
  alert( e.name );
  alert( e.message );
	// e 객체에는 name, message, stack이 있다.
}
```

1. 먼저, `try {...}` 안의 코드가 실행된다.
2. 에러가 없다면, `try` 안의 마지막 줄까지 실행되고, `catch` 블록은 건너뛴다.
3. 에러가 있다면, `try` 안 코드의 실행이 중단되고, `catch(err)` 블록으로 제어 흐름이 넘어간다. 변수 `err`(아무 이름이나 사용 가능)는 무슨 일이 일어났는지에 대한 설명이 담긴 에러 객체를 포함한다.

<aside>
💡 setTimeout처럼 "스케줄 된(scheduled)" 코드에서 발생한 예외는 `try..catch`에서 잡아낼 수 없다.

</aside>