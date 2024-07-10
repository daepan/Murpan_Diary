# React Query
>react-query는 서버의 값을 클라이언트에 가져오거나, 캐싱, 값 업데이트, 에러핸들링 등 비동기 과정을 더욱 편하게 하는데 사용됩니다.

## 언제 사용해야할까요?
위의 정리에서 보았을 때 React-Query의 장점은 서버의 값을 클라이언트에 적용하는 비동기 과정을 편리하게 관리하게 해준다는 장점이 있다고 한다.
정리가 필요한 경우는 무엇이 있을까?
최근 프로젝트를 설계하면서 React-Saga를 통해 비동기 처리를 관리했는데 변경되고 response 결과값을 유지하기 위해서 해당 Redux에 initialState로 관리해주었는데
해당 부분이 너무 많은 양을 관리하게 되니 상당히 지저분할 뿐만 아니라 Saga를 사용해보신 분들 중 Ducks패턴을 하게 되면 해당 파일의 코드의 양이 매우 길어지는 것을
경험했었다. 

</br>
그래서 결국 store는 클라이언트 state 값을 유지해야 하는 것이 아닌 Store 내부에 서버 데이터가 공존하게 되고 결국 store의 안에는 
서버와 클라이언트 데이터의 경계가 애매해지며, 데이터의 구분이 명확하지 않은 단순한 데이터를 쌓아두는 창고가 되어버리게 된다.

</br>


>React-query는 서버와 클라이언트간의 데이터를 분리, 비동기 처리에 대한 편한 처리를 위한 기능등을 제공한다.

</br>

## React-query의 장점
* 캐싱
* get을 한 데이터에 대해 update를 하면 자동으로 get을 다시 수행한다.(ex. 게시판들의 글을 가져왔을때 게시판의 글을 생성하면 자동으로 게시판 글을 get api cal을 함)
* 데이터가 오래되었다고 판단되면 다시 get
* 동일 데이터가 여러번 요청되면 한번만 한다.
* 무한 스크롤([Infinite Queries](https://tanstack.com/query/v4/docs/guides/infinite-queries?from=reactQueryV3&original=https://react-query-v3.tanstack.com/guides/infinite-queries))
* 비동기 과정을 선언적으로 관리할 수 있다.
* react hook과 사용하는 구조가 비슷하다.



