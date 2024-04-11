# display 속성이란?
 

* display 태그는 화면에 어떻게 드러나게 할지를 결정하는 속성입니다. 사실 이렇게 들으면 감이 잘 안오는데 요소 크기를 어떻게 결정할건가 를 결정하는 속성이라고 이해하시는게 조금 더 감이 잘 잡히는 것 같습니다!

 

## display 속성값의 종류
 

display 속성값은 4가지 입니다.

* none
* cblock
* inline
* inline-block


display: none;

화면에서 사라집니다. 크기 자체도 차지하지 않습니다!

 

display: block;

일반적으로 설정하지 않아도 div가 갖게되는 기본값입니다. (태그에 따라 기본값이 다를 수 있습니다.)

기본적으로 width 가 자신의 컨테이너의 100% 가 되게끔 합니다. 쉽게 말하자면, 가로 한 줄을 다 차지하게 됩니다.

 

display: inline;

컨텐츠를 딱 감쌀정도의 크기만 갖게됩니다. block태그와 다르게 줄바꿈이 되지 않고, 반드시 컨텐츠를 감싸게 되고, 크기를 변화시킬 수 없습니다. 예시 css에서도 width를 임의로 500px 로 바꾸어줬지만 크기는 여전히 글의 길이 만큼입니다.

 

display: inline-block;

inline과 block의 특성을 합쳐놓은 속성입니다. 기본적으로는 inline의 속성을 지니고 있지만, 임의로 크기를 바꿔줄 수 있습니다. 참고로 Explorer 7  이하에서는 사용할 수 없습니다!

ul과 ol에 대하 기본속성
ul {
  display: block;
  list-style-type: disc;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  padding-inline-start: 40px;
}

ol {
  display: block;
  list-style-type: decimal;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  padding-inline-start: 40px;
}

li {
  display: list-item;
  text-align: -webkit-match-parent;
}

 list-style-position를 통해서 마커를 변경할 수 도 있다.