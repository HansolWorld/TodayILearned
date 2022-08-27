# three_raycaster

```jsx
const lineMaterial = new THREE.LineBasicMaterial({color: "white"})
const points = []
points.push(new THREE.Vector3(0,0,100))
points.push(new THREE.Vector3(0,0,-100))

const lineGeometry = new THREE.BufferGeometry().setFromPoints(points)
const guide = new THREE.Line(lingGeometry, lineMaterial)
scene.add(guide)
```

물체를 감지하는걸 확인하기 위해 임의의 선을 만든다. 두개의 점을 잇는 섬을 만들기 위해 vector3를 사용해 벡터를 만들고 line을 만들어 준다.

## Raycaster

```jsx
const raycaster = new THREE.Raycaster()

function draw() {
    const origin = new THREE.Vector3(0, 0, 100)
    const direction = new THREE.Vector3(0, 0, -1)
    raycaster.set(origin, direction)

    const intersects = raycaster.intersectObjects(meshs)
    intersects.forEach(item => {
        item.object.material.color.set("red")
    }
}
```

raycaster의 intersects를 사용해 object에 접근해 object의 정보를 가져올 수 있다. 

```jsx
const raycaster = new THREE.Raycaster()
const mouse = new THREE.Vector2()

function checkIntersects() {
    if (mouseMoved) return
    raycaster.setFromCamera(mouse, camera)

    const intersects = raycaster.intersectObjects(meshs)
    for (const item of intersects) {
        item.object.material.color.set("red")
    }
}

canvas.addEventListener("click", e => {
    mouse.x = e.clientX / canvas.clientWidth * 2 - 1
    mouse.y = e.clientY / canvas.clientHeight * 2 - 1
    checkIntersects()
})

let mouseMoved
let clickStartX
let clickStartY
canvas.addEventListener("mousedown", e => {
    clickStartX = e.clientX
    clickStartY = e.clientY
})
canvas.addEventListener("click", e => {
    const xGap = Math.abs(e.clientX - clickStartX)
    const yGap = Math.abs(e.clientY - clickStartY)
    if (xGap > 5 || yGap > 5) {
        mouseMoved = true
    } else { 
        mouseMoved = false
    }
})
```

마우스를 클릭해 마우의 위치를 가져온다. click envent를 이용해 마우스 위치를 가져오지만 renderer의 좌표와 일치해야 하기 때문에 canvas의 크기를 가져와 정규화를 해준다.  click 이벤트 함수를 만들고 raycaster를 사용해 마우스가 클릭된 곳의 object를 intersect를 통해 접근한다. 마우스의 드래그를 방지 할 수 있다. 마우스의 이동 거리를 계산해 일정 이상이 되면 마우스 이동으로 인식하고 그게 아니라면 클릭으로 인식하게 한다.