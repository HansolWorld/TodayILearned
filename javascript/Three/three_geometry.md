# three geometry

geometry는 점, 선, 면으로 이루어진 모양을 말한다.

## BoxGeometry

```jsx
const geometry = new THREE.BoxGeometry(1, 1, 1, 2, 2)
const material = new THREE.MeshStandardMaterial({
    color: "pick",
    wireframe: true, // 뼈대만 볼 수 있다.
    side: THREE.DoubleSide // 기본값은 내부는 면이 없다. 더블사이드를 설정하면 내부에서도 볼 수 있다.
}) 

const mesh =  new THREE.Mesh(geometry, material)
scene.add(mesh)
```

box 형태로 (x, y, z)의 길이로 만들 수 있다. (x, y, z) 이후에 2개의 인자는 변을 얼마나 나눌지를 설정한다. 값이 클수로 더 많이 나누고 좌표로 접근해 모양을 바꿀 수 있다.

## SphereGeometry

```jsx
const geometry = new THREE.SphereGeometry(1, 1, 1, 2, 2)
const material = new THREE.MeshStandardMaterial({
    color: "pick",
    side: THREE.DoubleSide, // 기본값은 내부는 면이 없다. 더블사이드를 설정하면 내부에서도 볼 수 있다.
    flatShading:true // 라운드처리 제거, wireframe기준의 면단위
}) 

const mesh =  new THREE.Mesh(geometry, material)
scene.add(mesh)

console.log(geometry.attributes.position.array)
```

SphereGeometry를 사용하면 구를 그릴 수 잇다. geometry.attributes에 접근해 geometry를 구성하는 요소를 확인할 수 있다. position에는 3개식 잘라서 (x, y, z)의 좌표를 담고 있다.

## positioon transform

```jsx
const positionArray = geometry.attributes.position.array
const randomArray = []
for (let i=0; i < positionArray.length; i+=3) {
    positionArray[i] = positionArray[i] + Math.random() - 0.5
    positionArray[i+1] = positionArray[i+1] + Math.random() - 0.5
    positionArray[i+2] = positionArray[i+2] + Math.random() - 0.5

		randomArray[i] = Math.random() - 0.5
		randomArray[i+1] = Math.random() - 0.5
		randomArray[i+2] = Math.random() - 0.5
}
```

geometry에서 position을 받아와 위치에 변화를 준다. position은 3개식 (x, y, z)가 담겨 있기 떄문에 3개 단위로 잘라서 변화를 준다. Math.random()을 사용하면 0~1 사이의 값이 나온다. -0.5~ 0.5까지 위치 변화를 준다. 0~1사이의 값으로 하게되면 한쪽으로 쏠림 현상을 확인할 수 있다.

```jsx
function draw() {
    const time = clock.getElapsedTime()
    for (let i=0; i < positionArray.length; i+=3) {
        positionArray[i] += Math.sin(time + randomArray[i]) - 0.5
        positionArray[i+1] += Math.sin(time + randomArray[i+1]) - 0.5
        positionArray[i+2] += Math.sin(time + randomArray[i+2]) - 0.5
    } 
	  geometry.attributes.position.needsUpdate = true
    
    renderer.render(scene, camera)
    renderer.setAnimationLoop(draw)
}
```

positionArray에 Math.sin을 사용해 물결 무늬를 만들 수 있다. sin은 -1~1값으로 position의 변화를 준다. 단 x, y, z방향으로 모두 같은 값을 이동하면 구 자체가 움직이게 된다. 따라서 random의 값을 더해 물결무늬를 만들어 줘야한다. position의 값이 변경 되었다면 needsUpate를 사용해 변경된 position이 mesh에 적용되게 해줘야 한다.