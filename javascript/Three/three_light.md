# three_light

## AmbientLight

```jsx
const light = new THREE.AmbientLight("white", 0.5) // 색상, 강도
scene.add(light)
```

전체적으로 은은하게 뿌려주는 빛이다. 보통 다른 조명과 같이 사용한다. 전체적으로 뿌려주는 빛이기 때문에 위치값이 없다.

## DirectionalLight

```jsx
const light = new THREE.DirectionalLight("white", 0.5)
light.position.y = 3
scene.add(light)
```

태양광같은 빛이다.

- 빛의 위치를 확인할 수 있다
    
    ```jsx
    const lightHelper = new THREE.DirectionalLightHelper(light)
    scene.add(lightHelper)
    ```
    

## Shadow

```jsx
renderer.shadowMap.enabled = true
// renderer.shadowMap.type = THREE.PCFShadowMap
renderer.shadowMap.type = THREE.BasicShadowMap //픽셀같은 그림자, 성능이 제일 좋다. radius 적용 안됨
renderer.shadowMap.type = THREE.PCFSoftShadowMap // PCFShadowMap보다 부드럽다. radius 적용 안됨

const light = new THREE.DirectionalLight("red", 0.5)
light.position.y = 3
scene.add(light)

light.castShadow = true
light.shadow.mapSize.width = 512
light.shadow.mapSize.height = 512
light.shadow.radius = 5
light.shadow.camera.near = 1
light.shadow.camera.far = 5

const planeGeometry = new THREE.planeGeometry(10, 10)
const material = new THREE.MeshStandardMaterial({color: "gold"})
const plane = new THREE.Mesh(planeGeometry, material)

plane.receiveShadow = true
```

그림자를 노출시키기 위해서는 renderer.shadowMap.enabled를 true로 설정해줘야 한다. 그후 물체에 각각 그림자 설정을 해줘야한다. castShadow는 그림자를 만들수 있는 물체를 의미하고. receiveShadow는 그림자가 생길 수 있는 물체를 의미한다. shadow의 퀄리티를 조절하기 위해서는 mapSize를 조절하면 된다. 기본값은 512이다. 값이 클수록 그림자가 그려지는 판의 크기가 커지며 고해상도의 의미를 가진다. light의 radius를 조절하면 그림자의 그라데이션 효과를 줄 수 있다. 또는 renderer.shdowMap.type으로 그림자의 퀄리티를 조절할 수 있다. light.shadow.camera를 통해 그림자가 그려지는 범위를 설정할 수 있다.

## PointLight

```jsx
const light = new THREE.PointLight("white", 1) // 색상, 강도, 거리
```

구체의 모양의 빛이 사방으로 퍼지는 모양

## Spotlight

```jsx
const light = new THREE.SpotLight("while", 1, 100, Math.PI/4) //색상, 강도, 거리, 각도
```

원뿔의 모양으로 빛이 강조되 퍼지는 모양

## HemisphereLight

```jsx
const light = new THREE.HemisphereLight("while", "black", 1) //색상1, 색상2, 강도
```

AmbientLight와 같이 전체적으로 빛을 뿌리지만 조명에서 가까운쪽을 색상1, 먼쪽을 색상2로 해서 색상의 변화를 준다. 

## RectAreaLight

```jsx
import {RectAreaLightHelper} from "three/examples/jsm/helpers/RectAreaLightHelper"

const light = new THREE.RectAreaLight("white", 10, 2, 2) //색상, 강도, width, height
```

사각형 모양의 빛이다. RecAreaLight의 경우에는 THREE에 없고 emxaples에서 불러와야 한다.