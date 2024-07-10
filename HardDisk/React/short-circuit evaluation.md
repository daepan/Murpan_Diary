# short-circuit evaluation

논리 연산자 (&& , ||) 를 사용하여 연산을 진행 할 때 `좌측 식의 값`에 따라 `우측 식의 실행 여부를 판단하는 동작`을 `단락 회로 평가`라고 한다.

만약 좌측식의 값이 false인 경우에는 && 연산을 하지 않고 조건문을 종료한다.

이러한 평가에 대한 의해 리액트 렌더링에 있어서 && 연산자의 사용을 지양하자고 한다.

왜냐하면 만약 리액트 렌더링에 있어서 `좌측 식의 값`에서 `undefined`인 경우에 렌더링 시간에 에러를 출력하게 된다.

```
"Uncaught Error: Error(...): Nothing was returned from render. This usually means a return statement is missing. Or, to render nothing, return null."
```

이러한 에러 출력을 피하기 위해서는 삼항 연산자의 사용을 권장한다고 한다.