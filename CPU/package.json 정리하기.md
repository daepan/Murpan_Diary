## 글을 쓰게 된 이유
우리는 Yarn berry로 업데이트하면서 많은 편의성이 생겼었습니다. **Plug'n'Play (PnP) 모드**를 통해 프로젝트 자체의 node_modules 폴더를 완전히 제거하고 패키지 의존성 자체를 pnp.cjs로 관리함으로써 디스크 사용량과 패키지 설치 속도를 줄일 수 있었습니다.
하지만 이러한 업데이트는 우리가 프로젝트에서의 패키지에 대한 의존성의 명확한 정의와 일관성을 더욱 요구하게 되었습니다. Plug'n'Play(PnP) 모드의 도입으로 의존성 충돌 방지와 설정 관리의 중요성이 커졌습니다. 또한, 플러그인 기반 구조와 Zero-Installs 기능이 추가되면서, 보안 유지와 호환성 관리가 더욱 중요한 과제가 되었습니다. 따라서, package.json을 체계적으로 관리하지 않으면 프로젝트의 안정성과 보안이 위협받을 수 있습니다.
이에 따라서 현재 BCSD Lab에서 활동하고 있는 레거시 그룹에서, 이러한 Yarn Berry 업데이트 이후에 package.json에 대한 관리가 소홀했음을 확인하고 이에 대해 의존성을 정리하고, 정리 방식에 대해 작성해보겠습니다.

## 현재 우리의 package.json 문제점

### `dependencies`와 `devDependencies` 의 이해
현재 우리 프로젝트에서의 dependecies에 문제가 있었습니다.
dependecies에 `@types/~`나 `@testing/~`과 같은 개발 환경에서 코드 작성, 테스트, 타입 검사를 위해 필요한 내용을 넣었습니다. 이렇게 넣게 될 경우에는 발생할 수 있는 문제는 아래와 같습니다.

* 프로덕션 빌드 크기의 증가
	* `dependencies`에 포함된 패키지들은 프로덕션 빌드 시에도 함께 설치되기 때문에 개발에만 필요한 패키지를 불필요하게 설치하게 됨으로써 프로덕션 빌드 크기가 증가할 수 있습니다.
	* 이는 빌드 시간과 배포 시간에 영향을 줄 수 있습니다.
* 명확한 의존성 관리의 어려움
	* `dependencies`에 포함된 패키지들은 프로덕션 빌드를 진행하면서 위와 같은 개발환경에서 필요한 불필요한 내용들이 섞여있다면, 정작 에러를 발생하거나 업데이트 해야하는 패키지를 확인하는데 어려움이 발생합니다.

물론 현재 우리가 관리하는 프로젝트가 몇 없으며, 사용하는 라이브러리가 적어서 수월하다고 생각할 수 있지만, 앞으로의 프로젝트 관리에 있어서 명확한 package를 정리하는 것이 앞으로의 관리가 유용하다고 판단됩니다.

## 사용하지 않는 패키지 제거
이렇게 `@types/~`나 `@testing/~`과 같은 개발 환경에서 코드 작성, 테스트, 타입 검사를 위해 필요한 내용을 `devDependencies` 에 넣음으로써 수정하니 우리가 사용하지 않은 패키지들이 보입니다.
제거한 패키지로는 2가지로, josa와 recoil 입니다.

### 패키지를 제거한 이유
이렇게 갑자기 제거하니 맥락이 부족하여 우리의 프로젝트에서 2개의 라이브러리를 제거한 이유에 대해 작성해보겠습니다.

#### Recoil
Recoil은 프로젝트를 처음 시작했을때 사용했던 전역 상태관리 라이브러리였습니다. 당시 Recoil은 Redux 보다 action이나 store등을 연계하고 관리하는 코드들이 적어 직관적이며, 학습이 쉬워 동아리 내에서의 새로운 부원들도 쉽게 적응할 수 있다고 생각해서 도입하였습니다.

하지만 점차 시간이 지날수록 Recoil의 단점이 드러나기 시작했습니다.
[메모리 누수](https://github.com/facebookexperimental/Recoil/issues?q=is%3Aissue+is%3Aopen+memory) 이슈 등 여러가지가 있었지만, 가장 큰 이유는 업데이트 주기가 거의 멈춰있다싶어서 였습니다.
기술적 이슈가 많은데, 정기 버전 업데이트가 지연되고 있다면, 이것을 과연 유지하는 것에 의미가 있을까 싶어서 Zustand로 마이그레이션을 진행했습니다.

> 왜 Zustand인가?
> 위에서 이야기 했듯 동아리내에서 새로운 부원들이 쉽게 사용할 수 있는 것도 장점이였습니다.
> 또한 전역 상태가 많이 사용하지 않았기에 가벼운 것도 하나의 이유였습니다.
> ![](https://i.imgur.com/togyaKP.png)


#### Josa
해당 라이브러리는 한국어 뒤에 을/를, 은/는 등에 대해 문장 뒤에 어떤 것이 오는 것이 좋을지를 자동으로 판단해서 조사를 붙여주는 라이브러리였습니다. 이를 제거한 이유는 이러한 Josa 라이브러리를 우리 동아리에서 많이 사용할 것으로 판단이 되어 동아리 내부 라이브러리를 구축해서 활용하기로 하였습니다.

직접 관리하기 때문에 해당 라이브러리 이슈를 직접 대응할 수 있으며, 프로젝트 옮길때마다 따로 추가할 필요없는 것이 장점입니다.
![](https://i.imgur.com/BEVvsVP.png)


## axios 버전업 관련
우리 프로젝트가 사용하고 있던 axios의 버전은 `"axios": "^0.27.2",`였습니다. 24년도 8월 26일 기준으로 최신 버전은 1.7.5 입니다.

1.x.x 버전으로 올린다고 생각했지만, 잠시 주춤했습니다. 왜냐하면 앞에 1이 붙은 순간 메이저 업데이트를 진행한 것으로 현재 있는 프로젝트에서 많은 이슈가 있을꺼라는 생각이 들었습니다. 그래서 천천히 메이저 업데이트 이후에 발생할 수 있는 이슈를 먼저 정리해두기로 하였습니다.

### headers에 대한 Content-Type에 대한 정의
`코인 - 한기대 사람들`은 원래 단순 정보 전달용으로 일방적인 수신의 입장에서 웹을 구성해뒀습니다. 하지만 이제는 실제로 리뷰하기 기능이라는 사용자가 직접 내용을 올릴 수 있는 기능이 추가 됨으로써 Content-Type에 대한 변경이 필요했습니다.

```js
// 이전 Content-Type의 경우
if (request.method === HTTP_METHOD.POST 
	|| request.method === HTTP_METHOD.PUT) {
	headers['Content-Type'] = 'application/json'
}
```

TypeScript에 맞게 적용하는 방법을 해봤습니다.
먼저 기본적으로 직접 관리하는 API Request에 대한 bodyType에 대한 타입을 변경하여 다른 부원들도 하드 코딩을 지양하도록 이를 수정하였습니다. 

```js
export const BODY_TYPE = {
	MULTIPART: 'multipart',
	JSON: 'json',
} as const;
// ...

export type HTTPBodyType = typeof BODY_TYPE[keyof typeof BODY_TYPE];
  
//...
export type APIRequest<R extends APIResponse> = {
	response: R
	path: string
	method: HTTPMethod
	params?: any
	data?: any
	baseURL?: string
	authorization?: string;
	bodyType?: HTTPBodyType;
	headers?: Record<string, string>
	parse?: (data: AxiosResponse<R>) => R
	convertBody?: (data: any) => any
};

// 사용 시

export class UploadFile<R extends UploadImage> implements APIRequest<R> {
	method = HTTP_METHOD.POST;
	
	response!: R;

	path = '~~';

	bodyType = BODY_TYPE.MULTIPART; // 간단하게 추가 가능!

	data: ~~;

	constructor() : ~~
}
```
