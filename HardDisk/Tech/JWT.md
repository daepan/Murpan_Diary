# Authorization flow with JWT
인가 플로우에 대한 논쟁은 오랜 시간을 거쳐 진행되어 오고 있다.

그 이유는 잘 알고 있는 두 가지 해킹 위험이다.

1. XSS (교차 사이트 스크립팅)
2. XSRF (사이트 간 요청 위조)
두개의 해킹 위험을 방지한 인가를 하기 위해 여러가지 방법을 사용할 수 있다.

## refresh token
SameSite, HttpOnly 쿠키에 저장
SSR일 경우에는 proxy 설정 or 또 다른 Cookie 설정을 해주어야 한다.
BFF(Backend For Frontend)를 사용하면 되긴 하지만.. 비용이 추가로 발생한다.
FrontEnd가 Web이 아닐경우 추가 로직이 필요하다.

## localStorage에 저장
OWASP 보안 지침에 위배된다.
하지만 "로그인 상태 유지"를 위해 결국 필요하다(쿠키를 써도 되긴 한다).
아직도 논쟁 대상에 있다.
refresh token의 유효함을 검사하기 위해 추가 로직이 필요하다.
## sessionStorage에 저장
로그인 상태 유지, 탭 간 인가 공유가 불가하다.
access token > SameSite, HttpOnly 쿠키에 저장
sessionStorage에 저장
access token의 유효함을 검사하기 위해 추가 로직이 필요하다(물론 한 번 요청 하기만 하면 되지만 인터넷이 느리다면?).

## JS 변수 내에 저장
새로고침 할 때마다 access token을 새로 요청해야 한다.
나만의 결론을 내리자면 refresh token을 쓸 때는 localStorage, access token을 쓸 때는 sessionStorage를 사용하는 것이다. 강화된 쿠키를 쓰지 않는 이유는 쿠키 때문에 더 작성하는 로직이 localStorage보다 많다고 생각하기 때문이다(그냥 쿠키를 쓰지 않는 이유는 XSRF 때문이다).

출처

추가: 쿠키와 함께 CORS 요청을 하려면 Credentials 설정을 추가해야 한다.