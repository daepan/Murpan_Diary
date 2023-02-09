# What is Axios
여러분들은 개발하시면서 한번쯤 들어보기도 했고 사용도 하셨을 라이브러리 axios에 대해 자세히 알고 계시나요?
이번 기회에 한번 정리해보시면 좋을 것 같습니다.

> Axios란?
> node.js 및 브라우저에서 사용하는 Promised 기반의 클라이언트입니다
> 서버 측에서는 기본 node.js HTTP 모듈을 사용하고 클라이언트(브라우저)에서는 XMLHttpRequests를 사용합니다

## Axios의 기능
1. 브라우저에서 XMLHttpRequest 만들기
2. node.js에서 http 요청하기
3. Promise API 지원
4. `request `및 `response` 가로채기
5. `request `및 `response` 데이터 변환
6. 취소 요청
7. JSON 데이터에 대한 자동 변환
8. XSRF로부터 보호하기 위한 클라이언트 측 지원

사용방법은 크게 두가지로, 직접 라이브러리를 설치하거나 CDN 링크를 활용하는 방식이 있습니다.

1. 라이브러리 설치
```
 yarn add axios //if using npm => npm install axios
```

2. CDN 링크
```
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```	

> 참고적으로 unpkg와 jsdelivr에 대한 이야기를 해보자면 이 둘을 비교하는 것은 옳지 않다. 왜냐하면 unpkg의 업그레이드 버전 jsdelivr이며, cdnjs보다 훨씬 더 범용적으로 사용할 수 있도록 제작되었기 때문에 jsdelivr을 사용하는 것을 추천한다.

