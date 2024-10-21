## 들어가며

리액트에서는 상위 노드가 `re-render`되면, 하위 컴포넌트들이 전부 `re-render` 됩니다.  
`virtual DOM`이 변경을 감지를 위한 작업을 하는데, 이 비교연산을 스킵하기 위해 `memoization`을 사용할 수 있습니다.
![](https://i.imgur.com/LF2kHl9.png)

아래 예시로 사용될 색상 팔레트에서 색상을 선택하거나, 인풋폼에 텍스트를 입력할때에, 연관된 하위 컴포넌트들을 전부 `re-render`하게 된다면 성능 이슈가 있을 수 있기 때문에 `memoization`이나 `debounce`등의 테크닉이 사용됩니다.