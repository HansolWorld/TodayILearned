# Three 기본요소

three는 크게 scene, mesh, camera, light, renderer로 구성되어있다. 5개의 간단한 특징에 대해 알아보려한다.

scene : 장면/무대로 물체를 올려놓는 곳을 말한다.

camera : 어떤 부분을 보여줄 것인가를 정한다.

mesh : 장면에 올리는 물체를 말한다. mesh는 geometry와 meterial로 이루어져 있다.

light : 재질에 따라 조명의 효과를 줄수 있다.

renderer : 화면에 보여준다.

## Renderer

```jsx
const renderer = new THREE.WebGLRenderer()
renderer.serSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)
```

renderer는 우리가 볼수 있게 three의 구성요소를 그려주는 역할을 한다. rederer를 만든후 render size를 지정해준다. renderer가 브라우저 사이즈와 동일한 사이즈를 갖게한다.

renderer에는 domElement요소가 있다. domElement가 renderer가 그림을 그린 부분이다. domElement가 브라우저에 보일수 있도록 document에 추가한다. body에 append를 할수도 있지만, canvas를 만들고 canvas에 추가하는 방법이 있다.

```jsx
const canvas = document.querySelector("#three-canvas")
const renderer = new THREE.WebGLRenderer({canvas})
renderer.serSize(window.innerWidth, window.innerHeight)
```

querySelector를 사용해 dom요소에서 canvas를 가져온다. renderer를 만들때 canvas를 넣어 canvas에 renderer가 들어가게 한다. 

```jsx
renderer.setClearAlpha(0.5)
renderer.setClearColor("red")
```

renderer.setClearAlpha를 사용해 renderer의 투명도를 줄수 있다. 투명하면 css의 background color를 볼수 있다. renderer자체에 color를 준다면 setClearColor를 사용해 색을 줄 수 있다.

- 참고. render를 부드럽게
    
    ```jsx
    const renderer = new THREE.WebGLRenderer({
        canvas,
        antialias: ture
    })
    ```
    
    antialias설정을 true로 해주면 renderin이 부드럽게 된다. 단, 성능이 떨어진다.
    
- 참고. 창사이즈 변경에 반응하도록 renderer설정
    
    ```jsx
    function serSize() {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
        renderer.render(scence, camera)
    }
    
    window.addEventListener("resize", setSize)
    ```
    
    window의 사이즈 변경 이벤트가 발생한다면 renderer의 사이즈가 변경하도록하는 function을 만들어준다. 이벤트 발생시 window.innerWidth, window.innerHeight를 가지고 camera.aspect와 renderer.setSize를 변경해준뒤, camera.updateProjectionmatrix, renderer.render를 업데이트 해준다. 
    
- 참고. renderer.setPixelRatio(window.devicePixelRatio)

## Scene

```jsx
const scene = new THREE.Scene()
```

객체와 카메라, 빛이 위치한 scene을 만들어 준다.

```jsx
scene.background = new THREE.Color("blue")
```

scene.background를 사용해 color를 설정할수 있다. scene의 설정한 색상은 renderer위에 있기 때문에 renderer의 배경색은 보여지지 않는다.

## Camere

```jsx
const camera = new THREE.PerspectiveCamera(
    75, // 시야각
    window.innerWidth / window.innerHeight, // 종횡비
    0.1, // near
    1000 // far
)

camera.position,x = 1
camera.position.y = 2
camera.position.z = 5
//camera.position.set(1, 2, 5)

camera.lookAt(0, 0, 0)
camera.zoom = 0.5
camera.updateProjectionMatrix()
scene.add(camera)
```

시야각 : camera가 비추는 좌우 각도를 말한다. 

종횡비 : camerar가 비추는 좌우 각도와 위아래 각도 비율을 말한다.

near/far : near부터 far까지가 보이게 된다.

camera를 만들었으면 scene에 add해야한다. camera의 기본 position은 0,0,0이다. position을 변경하기 위해서는 각각 x, y, z로 접근하는 방법과 set을 사용해 접근하는 방법이 있다.

camera의 메소드를 몇가지 알아보고 이후에 자세히 알아볼 것이다. camera.lookAt을 하게되면 해당 좌표를 카메라가 비추게 된다. camera.zoom을 사용하면 x,y,z의 비율을 가지고 거리를 조절할 수 잇다. 단 camera의 요소를 변경 했을때는 camera.updateProjectionMatrix()를 해줘야한다.

## Mesh

```jsx
const geometry = new THREE.BoxGeometry(1, 1, 1)
const material = new THREE.MeshBasicMaterial({
    color: "red"
})

const mesh = new THREE.Mesh(geometry, material)
scene.add(mesh)

renderer.render(scene, camera)
```

mesh는 geometry와 material로 이루어져 있다. 형태를 지정해준뒤 형태의 재질을 뭘로할지를 설정한다. geometry와 material을 만들었다면 THREE.Mesh를 이용하여 mesh를 만들어 준다. 만들어준 mesh는 scene에 추가를 해준뒤 renderer가 mesh를 그릴수 있게 camera와 scene을 넣어 rendering할 수 있게 한다.

## Light

```jsx
const light  = new THREE.DirectionalLight(0xffffff, 1)
scene.add(light)
```

light를 써서 scene에 빛을 추가할 수 있다. 빛은 material에 따라 다른 결과를 보여준다.

```jsx
light.position.x = 2
light.position.z = 2
light.position.y = 2

//light.position.set = (2, 2, 2)
```

position을 사용해 빛의 위치를 변경할 수 있다.

## 애니메이션 넣기

```jsx
function draw() {
    meesh.rotation.y += 0.1
    renderer.render(scene, camera)

    // window.requestAnimationFrame(draw)
    renderer.setAnimationLoop(draw)
}

draw()
```

requestAnimationFrame을 사용해 리페인트가 되기 전에 해당 애니메이션을 업데이트 하는 함수를 호출한다. mesh에 변화를주고 renderer를 통해 그리게되면 바뀐 renderer를 브라우저에 보여주게된다. 보여주기 전 함수를 호출하게 된다. renderer에도 setAnimationLoop가 있다 내부적으로 requestAnimationFrame를 사용한다.

```jsx
const clock = new THREE>.Clock()

mesh.rotation.y = clock.getElapsedTime()
// mesh.rotation.y += clock.getDelta()
// const time = Date.now()
```

clock.getElapsedTime()을 사용하면 경과시간을 출력해준다. 경과시간을 기준으로 rotation을 하게되면 클라이언트 컴퓨터마다 생기난 차이를 보정할 수 있다. clock.getDelta()를 사용하게 되면 시간 차를 출력한다. 일정 간격의 시간이 출력되므로 rotation에 더해줘야된다. js에는 Date가 존재한다. Date.now를 하게되면 현재 시간을 출력하게 된다. 시간을 저장한뒤 Date.now() - time을 하게되면 시간 간격을 확인할 수 있다.