## What the reduce?
> `reduce()`는 배열의 각 요소에 대해 주어진 리듀서 (reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.
> 출처: ([Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce))

즉, `reduce()`란 배열에 사용하는 메서드로,  배열의 각 요소에 리듀서라는 함수를 
실행시키는 경우가 있을 때 필요한 것입니다.

> 그렇다면 `forEach`를 사용해도 괜찮지 않을까요?

이러한 내용에 많은 사람들이 생각이 이렇게 된다.
배열에 대한 간섭이라면 쉽고 간편하게 쓸 수 있다고 생각한다.

한번 이에 대한 코드를 작성해보자

```js
var forEachSum = 0;
var arr = [0,1,2,3];

arr.forEach((acc) => {
	sum += acc;
});

var reduceSum = arr.reduce((prev, curr) => prev + curr);

console.log(forEachSum) // 6
console.log(reduceSum) // 6
```

뭐야 다를게 없네?
단순한 합 계산에 있어서 별 다른 점을 찾아 볼 수 없었습니다.

그렇다면 reduce는 언제 사용해야할까요?
