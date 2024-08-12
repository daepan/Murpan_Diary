# React: Props
리액트는 `props`를 통해서 다른 컴포넌트들과 소통합니다. 
그렇다면 그 소통은 무엇일까요? 모든 부모컴포넌트들은 `props`를 전달함으로써 정보를 자식 컴포넌트에게 전달할 수 있습니다. 
전달되는 정보들은 값, 배열, 함수, 객체 즉, 자바스크립트 `value`를 전달할 수 있습니다.

일반적으로 우리가 HTML에서 `<img>`로 요소를 만들게 된다면 아마 여러가지 HTML속성을 부여할 것이다.

```js

	<img
	 	className="image"
		src="https://~~~.com/~~~.jpg"
		alt="img"
		width={100}
		height={100}
	/>

``` 


리액트에서는 우리가 가장 중요하게 생각하는 것은 재사용성이다. 
만약 내가 다양한 사이즈의 이미지를 100개 출력해야하는 UI를 개발할 때, 여러분이라면 저런 태그를 100개를 만들어야하는가? 아니다. 이를 간단하게 `props`로 해결하는 과정을 작성해보겠다.

```js
function Avatar({person, size}) {
  return (
    <img
      className={person.name}
      src={person.url}
      alt={person.id}
      width={size.width}
      height={size.height}
    />
  )
}

export default function Profile() {
  return (
    <div>
      <Avatar 
        person={{id:"one", img_url:"~~~.jpg", name:"one"}}
        size={{width:100, height:100}}
      />
      <Avatar 
        person={{id:"two", img_url:"~~~.jpg", name:"two"}}
        size={{width:50, height:50}}
      />
      <Avatar 
        person={{id:"one", img_url:"~~~.jpg", name:"one"}}
        size={{width:200, height:200}}
      />      
    </div>
  )
}
```


이렇게 나는 단순하게 `Profile` 함수에서 값만 변경했을 뿐인데 다양한 크기의 `Avatar`컴포넌트를 재사용할 수 있다. 이것이 `Props`의 값을 통한 재사용성을 높히는 방법이다.

```js
function Avatar(props) {
	let person = props.person;
	let size = props.size;
  return (
    <img
      className={person.name}
      src={person.url}
      alt={person.id}
      width={size.width}
      height={size.height}
    />
  )
}
```

> Props 사용 주의사항
> 위와 같이 단순하게 props라는 인자로 받게되는 예시를 보게 될 것이다.
> 이와 같은 방법은 정말 단순한 구조의 데이터가 아니라면 사용을 지양하는 것이 좋을 것이다.
> 아래에 한번 더 변수선언을 통해 처리해야하는 번거로움이 있기에 조금 더 디테일에 신경써야한다.

Reference: [Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)