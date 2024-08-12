# blocking / non-blocking / sync / async

Node.js 자료를 공부하다가 논블로킹 I/O라는 개념에 대해 설명하는 내용이 있는데  
블로그에서 비동기와 유사하다고 하는 경우도 있고 비동기라고도 설명하는데  
확실한 차이에 대해 한번 기록해보자  
  
## Blocking/NonBlocking

단어 그대로 해석한다면 Blocking.
행위자가 취한 행위 자체가, 또는 그 행위로 인해 다른 무엇이 막혀버린, 제한된, 대기하는 상태.

Blocking/NonBlocking은 호출되는 함수가 바로 리턴하느냐 마느냐가 관심사를 둔다. 

호출된 함수가 바로 리턴해서 호출한 함수에게 `제어권을 넘겨주고`, 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있으면 NonBlocking이다.

그렇지 않고 호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 함수에게 `제어권을 넘겨주지 않고 대기`하게 만든다면 Blocking이다.

한마디로 제어권을 넘기느냐 안넘기느냐에 대한 차이로 blocking과 non-blocking이라고 나눈다는 것이다.  
**이것이 마치 비동기와 동기의 개념과 같다라고 생각한다면 매우 잘못된 생각!!**

## Synchronous / Asynchronous
동시에 발생하는 것들.
동시라는 것은 즉, 동일한 순간에서, 함께 무언가가 이루어지는 두 개 이상의 개체 혹은 이벤트를 의미한다고 볼 수 있다.

호출된 함수의 수행 결과 및 종료를 호출한 함수가(호출된 함수뿐 아니라 호출한 함수도 함께 신경 쓰면 Synchronous  
호출된 함수의 수행 결과 및 종료를 호출된 함수 혼자 직접 신경 쓰고 처리한다면 Asynchronous  

## Non-blocking & Synchronous
![](https://musma.github.io/assets/song/2019-04-17/figure3.gif)
  
  그래서 이런 로직을 생각해볼 수 있는 겁니다. 호출된 함수가 호출한 함수에게 제어권을 바로 건네주어(return) 호출한 함수가 다른 업무를 볼 수 있었음(Non-blocked)에도 불구하고 여전히 호출된 함수의 업무 결과에만 줄곧 함께(synchronously) 신경쓰고있는 형태가 될 겁니다.

## Node example
그렇다면 제어권의 차이라는 건 이해했다면 Node.js에서 blocking과 non-blocking을 확인해보자.

* nodejs에서의 blocking 예제([readFileSync()](https://www.geeksforgeeks.org/node-js-fs-readfilesync-method/)메서드 활용)
```js
const fs = require('fs');
   
const filepath = 'text.txt';
  
//  sync-blocking 방법을 통한 파일 읽기 
const data = fs.readFileSync(filepath, {encoding: 'utf8'});
  
// 내용 파일 출력
console.log(data);
  
// 1~10까지 sum 합계산
let sum = 0;
for(let i=1; i<=10; i++){
    sum = sum + i;
}
  
// sum 출력
console.log('Sum: ', sum);
```
출력결과
```
This is from text file.
Sum:  55
```  

* 확실한 비교를 위해 non-blocking([readFile()](https://www.geeksforgeeks.org/node-js-fs-readfile-method/))메서드 활용을 통해 구현해보곘다.
```js
const fs = require('fs');
   
const filepath = 'text.txt';
  
// async=nonblocking 방식의 파일 읽기 
fs.readFile(filepath, {encoding: 'utf8'}, (err, data) => {
    // 파일 내용 출력
    console.log(data);
});
  
// 1~10까지 sum 합계산
let sum = 0;
for(let i=1; i<=10; i++){
    sum = sum + i;
}
// sum 출력
console.log('Sum: ', sum);
```
츨력결과
```
Sum:  55
This is from text file.
```
위 둘의 차이가 한 눈에 들어온다.   
>`non-blocking`에서 `sum`은 실제로 파일 내용보다 먼저 print된다. 이는 프로그램이 `readFile()` 함수가 반환되고 다음 작업으로 이동할 때까지 기다리지 않기 때문이다.(Non=Blocking) 그리고 `readFile()` 함수가 반환하면 내용을 print한다.  


