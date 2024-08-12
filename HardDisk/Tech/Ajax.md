<h1>Ajax</h1>
AJAX(Asynchronous JavaScript And XML)서버와 비동기적으로 데이터를 주고 받는 자바스크립트 기술
라고 정의한다.

하지만 이렇게 말한다면 이해하기 어렵다! 이를 설명하고 공부해보자
공부하기 위해서는 약간의 사전지식을 필요로 하는데
서버와의 통신을 이해해야하는데
보통 서버와 통신을 할 때 기본적으로 서버에 데이터를 요청할 때,
원하는 URL과 그 URL을 통해 GET요청을 해야 서버와 정상적으로 통신이 가능하다.

여기서 GET으로 요청하는 방법은 총 3가지가 있다
1. 주소창에 URL을 입력하여 GET 요청하기
2. form태그 URL을 입력하여 GET 요청하기
3. Ajax로 GET 요청하기

개발하는 입장에서 GET을 요청한다면 2번과 3번 중에 하나를 선택해서 GET을 요청해야할 것이다.
2번과 3번을 비교해보자
2번의 경우에는 form태그를 통해 GET을 한다고 했다. 하지만 이렇게 정보를 요청할 경우
홈페이지가 처음부터 새로고침이 되는 단점을 지니게 된다.
하지만 3번 AJAX의 경우에는 새로고침 없이 서버에게 GET요청할 수 있어, 웹에서 데이터를 요청할 때 
화면이 끊기거나 하지않고 부드럽게 웹을 제작할 수 있게 해준다.

자바스크립트로 Ajax 요청 날리기
방법 1. Old JS
```
    <script>
        var ajax = now XMLHttpRequest();
            ajax.onreadystatechange=function(){
                if(this.readyState == 4 && this.status == 200){
                    console.log(ajax.responseText)
                }
            };
            ajax.open("GET","http://~~~~~~~~", true);
            ajax.send();
    </script>
```
옛날 JS방식으로 불러올 수 있다. 하지만 보기만해도 복잡하고 명령어가 난해하다.
지금은 현재 많이 사용하지 않는 방식이다.

방법 2 New JS
```
  <script>
     fetch('http://~~~~~~~~~')
     .then((response)=>{//콜백함수
        if(!response.ok){
            throw new Error('400 or 500 Error')
        }
         return response.json()
     })
     .then((result)=>{
         consonle.log(result)
     })
     .catch(()=>{
         console.log('error')
     })
    </script>
```
fetch함수를 들어온 파라미터가 URL주소를 통해 받은 데이터이다. 1번 방법보다는 많이 간단한 코드로 
작성할 수 있다는 장점이 있다.

방법 2 - 1
```
    async function dataplz() {
        var response = await fetch('https://~~~~~~~~~~~');
        if(!response.ok){
            throw new Error(''400 or 500 Error);
        }
        var result = await response.json();
        console.log(result)
    }
    dataplz().catch(()=>{
        console.log('error');
    })
```
async await을 통한 비동기 처리. 위와 크게 다르지 않다.

방법 3. 외부 라이브러리 활용
React나 Vue의 경우 axios를 통해 정보를 요청한다.
```
    axios.get('https://~~~~~~~~~~~')
     .then((result)=>{
         consonle.log(result.data)//서버에서 받아온 데이터
     })
     .catch(()=>{
         console.log('error')
     })

```

Ajax Error Example

1. CORS 관련 에러
대충 이런 경우는 보안에 의해 막히는 현상이다.
개인적으로 집에서 개발하는 경우 CORS 정책을 꺼둠으로써 에러를 해결할 수 있다.

헤더에 Access-Control-Allow-Origin:'*' 추가하기
Node JS
```
    var cors =require('cors')
    app.use(cors());
```