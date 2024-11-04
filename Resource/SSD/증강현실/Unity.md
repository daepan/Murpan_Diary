## 초기 화면
![](https://i.imgur.com/irQFZvG.png)
![](https://i.imgur.com/6VbJJX8.png)
![](https://i.imgur.com/BtWQe5U.png)


## UI 컴포넌트
- Image: 그림을 표현하거나 나타날때 사용되는 컴포넌트
- Text: 글씨를 나타낼때 사용되는 컴포넌트
- Button: 버튼을 통해 이벤트를 실행시키고 싶을때 사용되는 컴포넌트
- UI들은 기본적으로 Canvas의 자식 오브젝트로 지정된다
	- Canvas의 Render Mode를 통해 UI가 Unity 가상환경 내에 배치되는 방식을 정할 수 있다.
	- UI들은 3D 오브젝트와 달리 RectTransform 컴포넌트를 사용

## 호출 순위
![](https://i.imgur.com/j1UjHSZ.png)


## 증강현실 이란?
![](https://i.imgur.com/UHrExoL.png)

- 실제 세계에 가상의 이미지를 실시간으로 증강 시키는 기술
## 유니티 물리엔진
- 유니티는 물리엔진이 내장되어 컴포넌트의 형태로 제공됨
- 따라서 물리를 구현하지 않고 컴포넌트를 사용 가능하다
- 3D와 2D는 다름

### 물리엔진의 계산 비용
- 계산비용이 높으며
- 일반적인 FPS보다 빠르게 설정됨
- 고정된 시간 0.02 1초에 50번 호출한다
	- 물리엔진을 사용할때 Update가 아닌 FixedUpdate를 호출
- 물리연산이 중요하지 않으면 조정해서 최적화함

### 강체의 뜻
- 일반적인 물리에서의 강체와는 다르다
- 유니티에서 강체
	- Rigidbody component가 없다 = 정적인 물체
	- Rigidbody component가 있다 = 동적인 물체

### Rigidbody를 쓰는 이유
- 물리엔진에서 오브젝트의 이동/회전을 표현하기 위해
- 정적인지, 동적인지 구분하기 위해
	- 정적인 것을 미리 계산하여 연산량을 줄인다.

### Rigidbody의 요소
- 질량
- 공기저항
- 토크로 회전할때 공기저항
- 질량 중심 자동계산
- 선형 자동 적용
- 중력 작용 여부ㅋ
- 물리엔진으로 제어되지 않고 오로지 Transform으로만 조작

## 물리 미세 조정
- Dynamic Friction 
	- 이미 움직이고 있을 때 사용되는 마찰입니다. 일반적으로 0과 1 사이의 값입니다. 값이 0이면 얼음처럼 미끄러워지 고 1이면 오브젝트가 큰 힘이나 중력에 밀리지 않는 한 아주 빨리 정지합니다. 
- Static Friction 
	- 오브젝트가 표면 위에 가만히 놓여 있을 때 사용되는 마찰입니다. 일반적으로 0-1사이의 값입니다. 0이면 얼음처럼 미끄러워지고, 1이면 오브젝트를 움직이기가 매우 힘들어집니다.
- Bounciness 
	- 표면이 얼마나 탄성이 있는지 나타냅니다. 값이 0이면 물체가 바운스하지 않고 값이 1이면 물체가 에너지 손실 없이 바운스합니다. 
- Friction Combine : 충돌하는 두 오브젝트의 마찰이 합쳐지는 방법입니다. 
	- Average 두 마찰 값의 평균이 사용됩니다. 
	- Minimum 두 값 중 작은 값이 사용됩니다. 
	- Maximum 두 값 중 큰 값이 사용됩니다. 
	- Multiply 두 마찰 값을 서로 곱합니다. 
- Bounce Combine 
	- 충돌하는 두 오브젝트의 탄성을 합치는 방법입니다. 마찰 결합 모드와 동일한 모드가 있습니다

##  충돌 Collider 컴포넌트
- 충돌체라는 뜻
- 종류
	- 연산량 :  구 < 캡슐 < 박스 <<< 매쉬
- Collision
	- Collider의 Is Trigger를 사용하지 않을 경우
	- Collision을 함
	- 물리적인 충돌을 한다

## Enter/Stay/Exit
- Enter : 충돌을 시작할 때 
- Stay : 충돌 중일 때 
- Exit : 충돌이 끝날 때

- OntriggerEnter(충돌되는 오브젝트) 
- OntriggerStay(충돌되는 오브젝트) => 충돌하지 않고 그냥 지나감
- OntriggerExit(충돌되는 오브젝트) 

- OncollisionEnter(충돌되는 오브젝트) 
- OncollisionStay(충돌되는 오브젝트) => 두 물체가 충돌함
- OncollisionExit(충돌되는 오브젝트)

## Tag
- 두 물체가 충돌 했을 떄 어떤 오브젝트와 충돌했는지 확인할 수 있는 컴포넌트
	- 계층뷰에서 설정한 이름과 달리 코드내에서 확인할 수 있느 ㄴ이름
- Enemy라는 Tag를 가진 오브젝트와 충돌했을 경우에만 처리하고 싶은 경우
	- col.Tag === "Enemy" 라는 조건으로 할 수 있다.