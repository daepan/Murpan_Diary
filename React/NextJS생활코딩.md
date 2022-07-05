# Next.js (생활코딩 영상 분석)

>커뮤니티에서 웹을 구현하기 위한 기능을 영리하게 엮어둔 기능  
Next.js는 React 개발환경이 내장되어 있다.   
ex. Express + React-Router-Dom, ServerSide Rendering 등 기능들을 따로 추가하지 않아도됨

### 실행 방법

Next.js 실행하는 방법 
NEXT.js 공식 홈페이지의 Get Started를 참고하여 실행

```js
    //현재 디렉토리에 NEXT 개발환경 세팅명령어
    npx create-next-app@latest .
```

시작 명령어 모음
```js
    //개발환경실행
    npm run dev
    //배포 파일 생성
    npm run build
    //서비스 시작
    npm run start
```

npm run dev를 통해 

/pages/index.js 가 홈페이지에 대한 코드를 작성하는 부분

npm run build를 사용하게 되면 빌드파일 생성됨

npm run start 파일을

### Route

url에 따라 UI를 어떻게 응답할까를 생각할 때 이용하는 기능
```js

    // sub로 이동
    <a href="/sub">sub/index.js</a>
    // 파라미터 이용
    import {useRouter} from 'next/router'

    const rotuer = useRouter();
    const id = Number(router.query.id);
    return <>
        <p>parameter id : {id}</p>
    </>

    //API Route
```