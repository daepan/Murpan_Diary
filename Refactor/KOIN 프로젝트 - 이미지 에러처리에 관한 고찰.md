## 서론
이전에 제작했던 Portal을 통해 이미지를 처리한 기억이 있었는데 해당 부분에서 최근 사장님 서비스를 출시하면서 이미지의 안정성이 우리쪽에서 올리는 것이 아닌 클라이언트가 생겨버렸다. 그렇다면 이러한 이미지가 혹시라도 깨져버린다면? 현재는 이를 막을 방법이 없을 것이다. 실제로 이미지 업로드 방식이 변경되면서 이미지가 여럿 깨지는 현상이 있었는데 미리 크러쉬 이미지가 나오는 것을 막고 에러 이미지를 통해 이를 확인할 수 있는 방법을 생각하게 되었습니다.

## 이미지 에러 추적문제

근본적으로 이미지가 깨졌을 때 에러 이미지를 찾고 싶은 것인데 그렇다면 이미지의 에러를 확인하고 싶다.
현재 방식에서는 `<img />` 를 활용해서 보여주고 있는데 에러를 추적하기 위해 태그 내부에 있는 `onerror`를 통해 추적할 수 있습니다.

해당 내용은 아래와 같습니다

> 이미지를 로드하거나 렌더링하는 동안 오류가 발생하고 오류 이벤트에 대한 onerror 이벤트 핸들러가 설정된 경우, 해당 이벤트 핸들러가 호출됩니다. 이는 여러 상황에서 발생할 수 있으며, 다음을 포함합니다:
> 
> - src 속성이 비어 있거나(`""`) `null`입니다.
> - src URL이 사용자가 현재 방문 중인 페이지의 URL과 동일합니다.
> - 이미지가 로드되지 않게 하는 방식으로 손상되었습니다.
> - 이미지의 메타데이터가 그 차원을 검색할 수 없을 정도로 손상되었고, `<img>` 요소의 속성에 차원이 지정되지 않았습니다.
> - 이미지가 사용자 에이전트에서 지원하지 않는 형식입니다.
> refrence: [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#image_loading_errors)

```tsx
간단 코드 작성
```

그렇다면 여기서 onError의 throw를 통해 ErrorBoundary를 통해 에러를 캐치하여 해당 문제를 해결하려했습니다. 
```tsx
<ErrorBoundary fallbackClassName="store-detail-image">
	<img 
	  className={styles.image}
	  src="" alt="상점 이미지"
	  onError={(e) => { 
		  if(e.type === 'error') 
			  throw new Error('Image load error'); 
		}}
	/>
</ErrorBoundary>
```

그러나...
![[스크린샷 2024-04-10 오후 6.18.58.png]]


저의 예상에서는 ErrorBoudary 코드에서 throw Error를 캐치하여 <img> `<img>`를 대체하여 ErrorBoundary에서 제공되는 에러를 표시하는 컴포넌트를 반환하고 싶었습니다.

```ts

```

![[Pasted image 20240410182603.png]]


![[Pasted image 20240410230329.png]]