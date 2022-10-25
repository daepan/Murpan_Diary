# Task Queue(Macrotask Queue)
우리가 기존에 알고 있던 Task Queue는 뒤에서 설명한 Microtask Queue와 구별하기 위해 Macrotask Queue라고도 부른다.

이 큐는 setTimeout(), setInterval(), setImmediate()와 같은 task를 넘겨받는다.
그렇다면 Microtask Queue는 어떤 비동기 작업들을 넘겨받는 것일까?



## Microtask Queue
Microtask Queue는 Promise나 async/await, process.nextTick, Object.observe, MutationObserver과 같은 비동기 호출을 넘겨받는다.
그리고 Microtask의 우선순위는 일반 task(또는 macrotask)보다 더 높다.
코드로도 이를 확인해보자. 아래 예시처럼, Promise로 비동기 호출을 하면 해당 작업은 Microtask Queue에 쌓이게 되는데, Microtask는 setTimeout과 같은 일반 Task보다 높은 우선순위를 가지고 있어서, Promise함수의 내용이 setTimeout함수의 내용보다 더 먼저 출력되는 것을 확인할 수 있다.
```js
// 1. 실행
console.log('script start')

// 2. task queue로 전달
setTimeout(function() {
  // 8. task 실행
  console.log('setTimeout')
}, 0)

// 3. microtask queue로 전달
Promise.resolve()
  .then(function() {
    // 5. microtask 실행
    console.log('promise1')
    // 6. microtask queue로 전달
  })
  .then(function() {
    // 7. microtask 실행
    console.log('promise2')
  })

// 4. 실행
console.log('script end')
해당페이지를 클릭하면 애니메이션으로 동작과정을 확인할 수 있다.
```
(브라우저 환경에 따라 아래와 같은 결과가 나오지 않을 수도 있다. 이는 브라우저마다 비동기 작업을 처리하는 세부적인 구조가 다르기 때문인데, 이 글은 V8 엔진을 사용하는 Chrome 브라우저를 바탕으로 한 설명 글이다.)
```
script start
script end
promise1
promise2
setTimeout
Promise
```
아래는 Promise가 Microtask Queue로 전달되는 과정을 나타낸 애니메이션이다.


## Async/Await
ES7에서는 Promise를 대체할 수 있는 async/await 문법이 등장했다. Async/Await이 Microtask Queue로 전달되는 과정을 확인해보자.  



Async/Await을 쓸 경우, Promise와는 다른 모습을 보이는 것을 확인할 수 있다.  

myFunc 함수 안의 두번째 줄이 실행되었을 때, one 함수는 콜 스택에서 pop되어 promise를 반환한다.
promise가 반환되었을 때 마주하는 것은 await 키워드인데, 이 경우 async 함수의 실행은 미뤄진다.
그리고 async 함수의 나머지 부분들을 실행하는 것은 microtask Queue로 넘겨진다.    
## Animation Frames
Animation Frames는 requestAnimationFrame과 같이 브라우저 렌더링과 관련된 task를 넘겨받는 Queue이다.  
우선순위는 Microtask보다는 낮고, (Macro)Task보다는 높다.  
코드로 확인해보자.  
```js
// 1. 실행
console.log("script start");

// 2. task queue로 전달
setTimeout(function () {
  // 10. task 실행
  console.log("setTimeout");
}, 0);

//3. microtask queue로 전달
Promise.resolve()
  .then(function () {
    // 6. microtask 실행
    console.log("promise1");
  }) // 7. microtask queue로 전달
  .then(function () {
    // 8. microtask 실행
    console.log("promise2");
  });

//4. AnimationFrame으로 전달
requestAnimationFrame(function () {
  //9. animation frame 실행
  console.log("animation");
});

//5. 실행
console.log("script end");
script start
script end
promise1
promise2
animation
setTimeout
```
## 정리
이벤트 루프가 비동기 작업을 처리하는 우선순위는 Microtask Queue -> Animation Frames -> Task Queue 순이다.
또한, 이벤트 루프는 Microtask Queue나 Animation Frames를 방문할 때는, 큐 안에 있는 모든 작업들을 수행하지만, Task Queue를 방문할 때는 한 번에 하나의 작업만 call stack으로 전달하고 다른 Queue를 순회한다.

