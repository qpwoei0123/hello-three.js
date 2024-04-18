# hello-three.js

<aside>
💁‍♂️ Three.js란 WebGL을 기반으로 하며 브라우저에서 3D 콘텐츠를 만들고 렌더링하는 데 사용되는 JavaScript 라이브러리다.
</aside>

## 맛보기 준비.

먼저 렌더링할 멋진 3D 객체가 필요하다.

무료 사이트는 많으니 마음에 드는 파일을 다운받자. ( **확장자는 GLTF로 다운받길!** )

<img width="706" alt="스크린샷 2024-04-11 오후 2 04 56" src="https://github.com/qpwoei0123/hello-three.js/assets/85989215/5ce88462-3405-436d-bb11-04eb7127195e">

<img width="681" alt="스크린샷 2024-04-06 오전 10 07 51" src="https://github.com/qpwoei0123/hello-three.js/assets/85989215/ab9c2fb3-de88-4627-8359-6ecef9530e6f">

[Korean Fire Extinguisher 3D 모델. 무료 다운로드. | Creazilla](https://creazilla.com/ko/nodes/7863932-korean-fire-extinguisher-3d-model)

## 프로젝트 생성.

프로젝트를 생성하고 canvas태그 하나를 만들자

canvas태그는 이제 소화기를 그려줄 것이다.

```html
<body>
    <canvas id="canvas" width="2000" height="2000"></canvas>
</body>
```

그다음 필요한 라이브러리들을 가져온 다음.

```html
 <script type="importmap">
      {
        "imports": {
          "three": "https://unpkg.com/three@0.141.0/build/three.module.js",
          "GLTFLoader": "https://unpkg.com/three@0.141.0/examples/jsm/loaders/GLTFLoader.js",
          "OrbitControls": "https://unpkg.com/three@0.141.0/examples/jsm/controls/OrbitControls.js"
        }
      }
 </script>
```

이제 Three.js를 사용할 준비가 되었다.

THREE를 가져온 다음 GLTF파일을 로드해주는 GLTFLoader를 가져오자.

```jsx
  <script type="module">
      // 라이브러리에서 필요한 모듈들을 가져옵니다.
      import * as THREE from "three";
      import { GLTFLoader } from "GLTFLoader";
```

## THREE로 무엇을 만들 수 있나?

3D객체가 있으면 공간또한 3D이여야 인지상정이다.

이러한 3D공간을 만들어주는 scene을 생성해보자. 

```jsx
 	let scene = new THREE.Scene();
 	scene.background = new THREE.Color("white");
```

이제 다운받은 GLTFL파일을 불러와서 scene에 추가하자.

```jsx
 let loader = new GLTFLoader();
		 // 경로로 로드된 파일을 콜백함수로 사용하자.
      loader.load("asset/korean_fire_extinguisher_01_4k.gltf", function (gltf) {
        let model = gltf.scene; // 로드된 gltf파일에서 scene이 곧 model이다.
        scene.add(model); // 만들어둔 공간객체에 추가.
      });
```

그리고 확인해보면~~~ 아무것도 나오지 않는다 왜냐하면 scene을 렌더링 하지 않았기 때문이다.

그렇다면 렌더러( WebGL로 렌더 해주는 녀석 )를 만들자! 

```jsx
let renderer = new THREE.WebGLRenderer({
        canvas: document.getElementById("canvas"), // 어디에 렌더링할 것인가?
        antialias: true,  // 테두리 계단현상 완화 ( 매끄러워짐 )
      });
 renderer.outputEncoding = THREE.sRGBEncoding; // 일반적인 컴퓨터 화면에 올바르게 표시되는 색상을 얻기 위해
```

추가적으로 렌더링된 객체를 관찰할 수 있는 카메라객체를 생성하고.

```tsx
	    // PerspectiveCamera는 원근법을 사용하여 시야를 표현하는 카메라다.
			// 첫 번째 매개변수는 시야 각도를 지정하고, 두 번째 매개변수는 종횡비(aspect ratio)를 지정한다.
			// 종횡비는 화면의 가로와 세로 비율을 의미한다.
      let camera = new THREE.PerspectiveCamera(40, 1); 
      camera.position.set(0, 0, 3); // 카메라의 위치를 벡터로.
```

그리고 렌더러로 객체와 카메라를 렌더 하면!

```jsx
 let loader = new GLTFLoader();
      loader.load("asset/korean_fire_extinguisher_01_4k.gltf", function (gltf) {
        let model = gltf.scene; 
        scene.add(model); 
        renderer.render(scene, camera) // 추가됨!
      });
```
<img width="199" alt="스크린샷 2024-04-11 오후 2 48 33" src="https://github.com/qpwoei0123/hello-three.js/assets/85989215/54dc2c06-8058-4f11-bbb0-d40a3c195ff4">

오늘의 포켓몬은 뭘까요…?

<br/>
<br/>

scene객체 안에 오로지 소화기객체밖에 없다. 현실에서는 당연히 빛이 있다는 생각 때문에 생각과 다른 결과가 나왔다.

빛이 없기때문에 전혀 보이지 않으므로 빛을 추가해주자.

```jsx
    	// DirectionalLight는 모든 방향에서 동일한 방향으로 빛을 내보내는 빛이다. 
    	// 매개변수로는 빛의 색상과 세기를 지정한다.      
      let light = new THREE.DirectionalLight(0xffffff, 1); 
      light.position.set(10, 10, 10); // 빛의 위치를 설정한다. DirectionalLight는 위치 대신 방향을 나타내는 벡터를 사용한다.
      scene.add(light); 
```
<img width="227" alt="스크린샷 2024-04-11 오후 2 53 14" src="https://github.com/qpwoei0123/hello-three.js/assets/85989215/346136cd-0730-4316-b9f8-508b45ca7a88">

짜잔 그냥 소화기 등장! 👏👏👏

## 하지만 그냥 렌더링은 심심하다.

3D라이브러리를 사용한 김에 움직이는 효과도 넣어보자.

animate함수를 정의해보자.

```jsx
loader.load("asset/korean_fire_extinguisher_01_4k.gltf", function (gltf) {
        let model = gltf.scene;
        scene.add(model);

        function animate() {
          requestAnimationFrame(animate);  // requestAnimationFrame을 사용하여 애니메이션을 반복 호출.
          model.rotation.y += 0.01; // 기울기 조절
          renderer.render(scene, camera); // 바뀐 모습을 렌더링
        }
        animate();
      });
```
![Apr-11-2024 14-56-50](https://github.com/qpwoei0123/hello-three.js/assets/85989215/9843991e-3db7-4a22-86ee-c3b92aeaf568)

잘~ 돈다~

<br/>
<br/>

근데 다른 Three.js 예제를 보면, 직접 마우스로 조작이 가능하다.

OrbitControls라는 컨트롤러로 조작을 하면 가능하니까 도전해보자!

눈치 챘겠지만 초반에 스크립트에서 OrbitControls를 불러왔었는데 이 때 사용하면 된다.

```jsx
import { OrbitControls } from "OrbitControls";
let controls = new OrbitControls(camera, renderer.domElement); // 여기서 renderer.domElement은 렌더러에서 설정한 HTML 요소.
```

컨트롤러를 만들었다면 애니메이션 함수에 추가하면 끝이다!

```jsx
    function animate() {
          requestAnimationFrame(animate);
          // model.rotation.y += 0.01;
          controls.update(); // 추가됨
          renderer.render(scene, camera);
        }
```

![Apr-11-2024 15-23-51](https://github.com/qpwoei0123/hello-three.js/assets/85989215/93e786e9-0b96-4572-b404-a92789257fe7)

마구마구 조작.

## 흐름 정리

뭔가 새로운 지식이 엄청 들어와서 헷갈릴 수도 있지만 흐름만 정리하자면.

1. 3D공간을 만들고, 공간에 빛과 물체를 넣는다.
2. 그리고 관찰하기위한 카메라를 생성한다.
3. 생성한 렌더러로 3D공간과 카메라로 화면에 보여준다. 

## 후기

일단 깊게 들어가진 않았다. ( 깊이가 얼마나 깊은지 모르기에… )

나중에 Three.js에 대한 프로젝트를 하나 만들어보고 싶은 마음에 살짝 맛을 보았다.

굉장히 매력적인 라이브러리이고, 리소스를 굉장히 많이 잡아먹을 것 같기도 하고 초반 렌더링도 너무나 느리기에 조금더 공부해봐야할 것 같다!

추가적으로 GLTF파일 굉장히 큰데 **레벨 오브 디테일(LOD), 스트리밍 등등** 흥미로운 성능개선 방법이 많으니 나중에 찾아보자!
