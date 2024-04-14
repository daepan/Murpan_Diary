웹페이지 내부에서 3D 요소 파일을 렌더링하는 경우가 있을 수 있습니다.
그러한 렌더링을 위해서는 그에 대한 3D 이미지와 이를 렌더링할 ThreeJS에서의 기능을 활용하는 것으로 간단하게 구현할 수 있습니다. 

이미지를 다운로드를 위해 해당 [링크](https://sketchfab.com/3d-models/shiba-faef9fe5ace445e7b2989d1c1ece361c)에 접속합니다.
![[Pasted image 20240414211621.png]]

이를 실제 웹페이지에 띄우기 위해서 ThreeJS를 다운로드해야합니다.

```shell
npm i @types/three
```

저는 TS 환경에서 제작했기 때문에 @types를 활용했습니다.

다음으로는 해당 파일을 렌더링 하기위해 Three.js 라는 라이브러리를 통해 렌더링하겠습니다.
```typescript
import { useEffect, useRef } from 'react';
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

export default function AnimateSection() {
  const mountRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    // Scene, Camera, and Renderer setup
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ alpha: true }); // allow transparency
    renderer.setClearColor(0xffffff, 1); // background color white
    renderer.setSize(window.innerWidth, window.innerHeight);

    if (mountRef.current) {
      mountRef.current.appendChild(renderer.domElement);
    }

    // Lighting
    const light = new THREE.AmbientLight(0xffffff, 1); // soft white light
    scene.add(light);

    let model: THREE.Object3D;

    // GLTF Model loading
    const loader = new GLTFLoader();
    loader.load(
      process.env.PUBLIC_URL + 'images/animate/shiba/scene.gltf',
      (gltf) => {
        model = gltf.scene;
        scene.add(model);
      },
      undefined,
      (error) => {
        console.error('An error happened', error);
      }
    );

    camera.position.z = 5;

    // Animation loop
    const animate = () => {
      requestAnimationFrame(animate);
      if (model) {
        model.rotation.y += 0.01; // rotate the model
      }
      renderer.render(scene, camera);
    };

    animate();

    // Clean up
    return () => {
      if (mountRef.current) {
        mountRef.current.removeChild(renderer.domElement);
      }
    };
  }, []);

  return <div ref={mountRef} />;
};
```

위에서 threejs에서 지원하는 GLTF 파일을 호출해주는 threejs 만의 로더를 활용해서 해당 파ㅣㅇㄹ을 렌더링해서 animate 함수 호출을 통해 애니메이션을 넣어줄 수 있다.
