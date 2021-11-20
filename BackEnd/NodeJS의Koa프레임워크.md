들어가기에 앞서...
프론트엔드를 공부하면서 서버의 중요성을 절실히 느끼게 된다. 서버에서 필요한 데이터가 있어야
더 완성도 있는 결과물을 만들 수 있다. 그렇기에 리액트 다루는 기술에서 보게된 내용을 공부하고 정리해보겠다.

BackEnd
어떤 종류의 데이터를 몇 개씩 보여줄지, 그리고 어떻게 보여줄지 등에 관한 로직을 만드는 것을
서버 프로그래밍 또는 백엔드 프로그래밍이라고 한다.

Node.js
자바스크립트 엔지을 기반으로 웹 브라우저 뿐만 아니라 서버에서도 자바스크립트를 사용할 수 있는 
런타임

Koa
기존 Express에 부족한 부분을 보완하여 Express 개발팀이 새롭게 개발한 프레임워크
Express는 미들웨어, 라우팅, 템플릿 같은 다양한 기능이 자체적으로 내장되어있지만
Koa는 미들웨어 기능만 갖추고 있으며,다른 라이브러리를 적용하여 사용한다
=> 필요한 기능들만 붙여서 서버를 만들수 있기에 Express보다 가볍게 사용이 가능하다!
추가로 Koa는 async/await 문법을 정식지원하기에 비동기 작업을 더 편리하게 관리가 가능하다.
/TIL 11.09

koa로 서버 띄워서 백엔드 라우팅하기

1. node 확인
```
    node --version
    v16.7.0
```

2. blog파일 생성 및 blog backend파일 생성

```
C:\Users\user\Desktop\WS>mkdir blog

C:\Users\user\Desktop\WS>cd blog

C:\Users\user\Desktop\WS\blog>mkdir blog-backend

C:\Users\user\Desktop\WS\blog>cd blog-backend

C:\Users\user\Desktop\WS\blog\blog-backend>npm init -y
Wrote to C:\Users\user\Desktop\WS\blog\blog-backend\package.json:

{
  "name": "blog-backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

3. koa 웹 프레임워크 설치
```
C:\Users\user\Desktop\WS\blog\blog-backend>npm add koa

{
  "name": "blog-backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "koa": "^2.13.4"
  }
}

```

4. ESLint 적용
```
C:\Users\user\Desktop\WS\blog\blog-backend>npm add --dev eslint
npm WARN install Usage of the `--dev` option is deprecated. Use `--include=dev` instead.

added 85 packages, and audited 128 packages in 6s

16 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
C:\Users\user\Desktop\WS\blog\blog-backend>npx eslint --init
√ How would you like to use ESLint? · problems
√ What type of modules does your project use? · commonjs
√ Which framework does your project use? · none
√ Does your project use TypeScript? · No / Yes
√ Where does your code run? · browser
√ What format do you want your config file to be in? · JSON
Local ESLint installation not found.
The config that you've selected requires the following dependencies:

eslint@latest
√ Would you like to install them now with npm? · No / Yes
Installing eslint@latest
npm WARN idealTree Removing dependencies.eslint in favor of devDependencies.eslint

up to date, audited 128 packages in 858ms

16 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Successfully created .eslintrc.json file in C:\Users\user\Desktop\WS\blog\blog-backend
ESLint was installed locally. We recommend using this local copy instead of your globally-installed copy.

```

5. 코아 서버 띄우기
```
서버 열기
//index.js
const Koa = require('koa');

const app = new Koa();

app.use(ctx => {
    ctx.body = "hello world";
});

app.listen(4000, () => {
    console.log('Listening to port 4000');
})


//cmd
C:\Users\user\Desktop\WS\blog\blog-backend>node src
Listening to port 4000


```

6. 미들웨어
koa 애플리케이션은 미들웨어의 배열로 구성되어 있다. 위의 index.js 파일 예시에서
app.use함수는 미들웨어 함수를 애플리케잇녀에 등록한다.
```
  //미들웨어 함수 구조
  (ctx, next)=>{

  }
```
koa의 미들웨어 함수는 두 개의 파라미터를 받는다. 첫 번째 파라미터는 ctx, 두번 째 파라미터는 next이다.
ctx는 Context의 약자로 웹 요청과 응답에 관한 정보를 지닌다. 
next는 현재 처리 중인 미들웨어의 다음 미들웨어를 호출하는 함수이다.
미들웨어의 다음 미들웨어를 호출하는 역할을 한다.
미들웨어를 등록하고 next함수를 호출하지 않으면 다음 미들웨어를 처리하지 않는다.

6-1, next함수는 Promise를 반환
next함수를 호출하면 Promise를 반환한다. next 함수가 반환하는 Promise 다음에 처리해야할 미들웨어가
완료된다.
```
const Koa = require('koa');

const app = new Koa();

app.use(ctx => {
    ctx.body = "hello world";
    if (ctx.query.authorized !== '1') {
        ctx.status = 401; //Unauthorized
        return;
    }
    next().then(() => {
        console.log('END');
    });
});

app.use((ctx, next) => {
    console.log(ctx.url);
    console.log(1);
    next();
})

app.use((ctx, next) => {
    console.log(2);
    next();
})

app.listen(4000, () => {
    console.log('Listening to port 4000');
})
```

6-2, async/await 사용하기
Koa는 async/await를 정식으로 지원하기 때문에 사용이 가능하다.
```
  const Koa = require('koa');

const app = new Koa();

app.use(async, (ctx, next) => {
    ctx.body = "hello world";
    if (ctx.query.authorized !== '1') {
        ctx.status = 401; //Unauthorized
        return;
    }
    await next();
    console.log('END');

});

app.use((ctx, next) => {
    console.log(ctx.url);
    console.log(1);
    next();
})

app.use((ctx, next) => {
    console.log(2);
    next();
})
app.listen(4000, () => {
    console.log('Listening to port 4000');
})

```
7, koa-router 사용하기
Koa를 사용할 떄도 다른 장소로 요청이 들어올 경우 다른 작업을 처리할 수 있도록 router를 사용한다
```
  C:\Users\user\Desktop\WS\blog\blog-backend>npm add koa-router

added 3 packages, and audited 226 packages in 999ms

28 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```
기본 사용법
```
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();

router.get('/', ctx => {
    ctx.body = "홈";
})

router.get('/about', ctx => {
    ctx.body = "소개";
})

app.use(router.routes()).use(router.allowedMethods());

app.listen(4000, () => {
    console.log('Listening to port 4000');
})
```
koa-router를 불러온 뒤 이를 사용하여 Router 인스턴스를 만들었다.
이처럼, 라우트를 설정할 때, router,get의 첫 번째 파라미터에는 라우트의 경로를 넣고, 두 번 째 파라미터에는 해당 라우트에 적용할 미들웨어 함수를 넣습니다. 여기서 get키워드는 해당 라우트에서
사용할 HTTP 메서드를 의미합니다. get 대신에 post,put,delete등을 넣을 수 있다.
11.20 TIL
