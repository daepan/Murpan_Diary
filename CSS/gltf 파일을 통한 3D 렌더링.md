웹페이지 내부에서 3D 요소 파일을 렌더링하는 경우가 있을 수 있습니다.
그러한 렌더링을 위해서는 그에 대한 3D 이미지와 이를 렌더링할 ThreeJS에서의 기능을 활용하는 것으로 간단하게 구현할 수 있습니다. 

이미지를 다운로드를 위해 해당 [링크](https://sketchfab.com/3d-models/shiba-faef9fe5ace445e7b2989d1c1ece361c)에 접속합니다.
![[Pasted image 20240414211621.png]]

이를 실제 웹페이지에 띄우기 위해서 ThreeJS를 다운로드해야합니다.

```shell
npm i @types/three
```

저는 TS 환경에서 제작했기 때문에 @types를 활용했습니다.

다음으로는 해당 파일을 렌더링합니다.