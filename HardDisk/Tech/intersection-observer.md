# intersection-observer

문득 가끔 TIL에 대한 주제는 나의 생각보다는 다른사람들이 개발이야기를 하는 도중에 캐치하는 경우가 많은 것 같다.  
이번에 캐치한 기술은 intersection-observer이다.  

이 기술을 캐치하게 된 계기는 이번에 새로 올라온 feconf2022레포를 보면서 인데  
상황을 설명해보면  
나 : 오 이번에 FEconf2022 레포 만들어졌네. 이번년도 라이브러리나 폴더구조는 이걸 참고해보면 어떨까?  
친구 : ㄴㄴ. 작년에는 intersection-observer를 쓰면서 새로운 패턴이다. 하면서 많이 좋아했는데 이번에는 딱히 새롭게 쓴 기술이 없어서 별로인듯함.  
  

뭐 참 별거 없고 1년전의 나는 왜 이런 소식이나 이야기를 못들은건지 이해할 수 없었다.  
이것이 중요한 기술인지 아닌지는 정확히 모르겠지만 공부해둬서 나쁠 것 없을 것 같다.   

모른다는 것을 알았으니 이제 공부해보자.  

## intersection-obeserver가 대체 뭐길래?
먼저 개념에 대해 정의하는게 먼저인 것 같아 MDN링크를 찾아보았다.

Intersection Observer API는 대상 요소와 상위 요소 또는 최상위 문서의 뷰포트와 교차하는 변경 사항을 비동기식으로 관찰하는 방법을 제공합니다. 

>*The Intersection Observer API provides a way to asynchronously observe changes in the intersection of a target element with an ancestor element or with a top-level document's viewport. fron.MDN* 

쉽게 풀어쓰면 이 기능은 비동기적 방식으로 실행되는 `scroll` 같은 이벤트 기반의 요소관찰에 발생하는 렌더링이나 이벤트의 연속호출을 더 매끄럽게 도와주는 기능이라고 할 수 있다.

## feconf레포에서의 적용방법
일단 2020년도 packge.json을 확인해보니 intersection-observer라는 단어가 눈에 들어왔고 2021년도에는 [npm](https://github.com/cats-oss/use-intersection)링크는 나중에 참고하면 좋을 것 같다.(Hook기능임)  
이제 본격적으로 레포를 분석해보자  
먼저 20년도 intersection-observer라는 기능이 처음 나온 것 같은데 한번 지켜보도록 하자.

2020년도를 지켜봤을 때는 intersection-obeserver 라이브러리 기능을 활용하여 훅을 구현하였고 이에 대한 훅을 각 세션에 적용한 형태였다.  
[2020FEconfHook](https://github.com/fedgkr/feconf2020/blob/master/src/utils/hooks/use-intersection.ts)

2021년도에는 [use-intersection](https://github.com/cats-oss/use-intersection)을 그냥 가져와서 사용한 것으로 보인다.

## 나도 한번 사용해보자 

---준비중---

