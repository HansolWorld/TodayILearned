# three_control

## OrbitControls

```jsx
import {OrbitConstrols} from "three/example/jsm/constrols/OrbitConstrols"

const controls = new OrbitConstrols(camera, renderer.domElement)
```

마우스 드래그, 휠로 객체를 움직일 수 있다.

```jsx
controls.enableDamping = true
controls.enableZoom = false // 휠사용 금지
controls.maxDistance = 10 // 휠로 멀리 갔을떄 최대 거리
controls.minDistance = 10 // 휠 최소거리
controls.maxPolarAngle = Math.PI // 수직방향 최대 각도
controls.minPolarAngle = Math.PI // 수직방향 최소 각도
controls.target.set(2, 2, 2) //회전의 중심점 설정
controls.autoRotate = true // 자동 회전
controls.autoRotateSpeed = 5 // 회전속도

function draw() {
    controls.update()
}
```

controls.enableDamping을 사용하면 마우스 이동을 부드럽게 할 수 있다. 단, render가 업데이트 될대 controls.update()를 해야한다. Zoom, Distance, PolarAngle으로 카메라의 설정을 변경할 수 있다. 또한, target.set을 사용하면 카메라의 시점을 변경한다.

## TrackballControls

```jsx
import {TrackballControls} from "three/example/jsm/constrols/TrackballControls"

const controls = new TrackballControls(camera, renderer.domElement)

controls.maxDistance = 10
controls.minDistance = 10
controls.target.set(2, 2, 2)

function draw() {
    controls.update()
}
```

Trackball로 컨트롤하는 느낌으로 OrbitConstrols과 비슷하지만 수직방향으로 막히는거 없이 컨트롤 가능하다.  draw에서 controls.update()를 하지 않으면 작동하지 않는다.

## FlyControls

```jsx
import {FlyControls} from "three/example/jsm/constrols/FlyControls"

const controls = new FlyControls(camera, renderer.domElement)
controls.rollSpeed = 1
controls.dragToLook = true
controls**.**movementSpeed **= 3**

function draw() {
    const delta = clock.getDelta()
    
    controls.update(delta)
}
```

w, s, a, d를 사용해 비행하듯이 컨트롤 할 수 있다. q, e를 사용하면 기울이기가 되고 r, f를 누르면 위로 아래로 향하게 된다. 마우스를 포인트로 카메라를 회전하게 된다. controls.rollSpeed를 사용하면 회전 속도를 변경할 수 있다. controls.dragToLook=true할 경우 마우스를 따라 회전하는 것을 멈추고 드래그로 회전하게 된다. movementSpeed를 사용해 움직임의 속도를 조절할 수 있다. FlyControls는 update시 delta로 시간을 넣어줘야한다. 

## **FirstPersonControls**

```jsx
import {FirstPersonControls} from "three/example/jsm/constrols/FirstPersonControls"

const controls = new FirstPersonControls(camera, renderer.domElement)
controls.lookSpeed = 1
controls.activeLook = false
controls.autoForward = true

function draw() {
    const delta = clock.getDelta()
    
    controls.update(delta)
}
```

FlyControls과 같은 방식으로 작동한다. FlyControls에 몇가지 기능을 추가하거나 수정한 버전이다.  FlyControls의 rollSpeed는 lookSpeed로 구현되어 있다. autoForward=true일 경우 자동으로 앞으로 이동하게 할 수 있다. activaLook의 경우는 마우스로 회전은 할 수 없고 w, s, a, d를 사용한 이동만 가능하다.

## **PointerLockControls**

```jsx
import {PointerLockControls} from "three/example/jsm/constrols/PointerLockControls"

const control = new PointerLockControls(camera, renderer.domElement)
controls.domElement.addEventListener("click", () => {
    controls.lock()
}

function draw() {
    // constrols.update()
}

```

PointerLockControls에는 update가 없다. update가 아니라 event가 일어 났을때 lock메서드를 호출해줘야 한다. click을 했을때 controls이 작동하며 이동이 lock되며 마우스를 통해 회전할 수 있다.

## **DragControls**

```jsx
import {DragControls} from "three/example/jsm/constrols/DragControls"

const controls = new DragControls(meshs, camera, renderer,domElement)
```

물체를 이동할 수 있는 controls이다. mesh의 배열을 DargControls에 인자로 넣어주어야 한다. mesh를 드래그인 드랍으로 이동할 수 있다.