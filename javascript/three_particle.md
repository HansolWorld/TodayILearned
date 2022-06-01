# three_particle

## Geomatry를 사용한 particle

```jsx
const geometry = new THREE.SphereGeomatry(1, 32, 32)
const material = new THREE.PointsMaterial({
    size: 0.02,
    sizeAttenuation: false
})
const points = new THREE.Points(geometry, material)
scene.add(points)
```

geometry의 points를 사용해 각 point마다 mesh를 배치한다. material을 pointMaterial을 사용하게 되면 지정한 points에 mesh를 넣을 수 있다. size로 mesh의 크기를 조절하고, sizeAttenuation을 통해 원근에 상관없이 일정한 크기로 넣을 수 있다. 

## Random한 위치에particle

```jsx
const geometry = new THREE.BufferGeometry()
const count = 1000
const positions = new Float32Array(count*3)

for (let i=0; i<position.length; i++) {
    position[i] = (Math.random() - 0.5) * 10
}
geometry.setAttribute(
    "position",
    new THREE.BufferAttribute(position, 3)
}

const material = new THREE.PointsMaterial({
    size: 0.02,
    sizeAttenuation: false
})
const particles = new THREE.Points(geometry, material)
scene.add(particles)
```

ramdom한 영역에 particle을 뿌릴 수 있다. 원하는 수의 particle을 지정한뒤 x,y,z의 순으로 랜덤한 위치를 지정한다. position에 랜덤한 위치로 담긴 매열을 geometry에 setAttribute로 담아준다. 담을떄는 position이 x, y, z로 3개 단위값으로 계싼되기에 (position, 3)으로 해줘야 한다. 만들어진 geometry를 pointsMaterial과 함께 THREE.Points로 particle을 만들어 준다.

## Image particle

```jsx
const textureLoader = new THREE.TextureLoader()
const particleTexture = textureLoader.load("star.png")

const geometry = new THREE.BufferGeometry()
const count = 1000
const positions = new Float32Array(count*3)

for (let i=0; i<position.length; i++) {
    position[i] = (Math.random() - 0.5) * 10
}
geometry.setAttribute(
    "position",
    new THREE.BufferAttribute(position, 3)
}

const material = new THREE.PointsMaterial({
    size: 0.1,
    map: particleTexture,
    transparent: true,
    alphaMap: particleTexture,
    depthWrite: false
})
const particles = new THREE.Points(geometry, material)
scene.add(particles)
```

사용할 이미지를 textureloader를 통해 불러온뒤 PointsMaterial에 Map 인자로 넣어준다. texture만 넣게되면 Png의 배경없음을 인식하지 못한다. transparent, alphaMap, depthWrite인자를 설정해 png의 배경을 없게 설정해준다. 

## Particle color transform

```jsx
const textureLoader = new THREE.TextureLoader()
const particleTexture = textureLoader.load("star.png")

const geometry = new THREE.BufferGeometry()
const count = 1000
const positions = new Float32Array(count * 3)
const colors = new Float32Array(count * 3)

for (let i=0; i<position.length; i++) {
    position[i] = (Math.random() - 0.5) * 10
    colors[i] = Math.random()
}
geometry.setAttribute(
    "position",
    new THREE.BufferAttribute(position, 3)
}
geometry.setAttrubyte(
    "color",
    new THREE.BufferAttribute(colors, 3)
}
const material = new THREE.PointsMaterial({
    size: 0.1,
    map: particleTexture,
    transparent: true,
    alphaMap: particleTexture,
    depthWrite: false
})
const particles = new THREE.Points(geometry, material)
scene.add(particles)
```

position과 마찬가지로 color의 배열을 만들어 준뒤 geometry에 setAttribute 매서드를 사용해 color인자에 colors배열을 넣어준다. 

## Points mesh

```jsx
const planeMesh = new THREE.Mesh(
    new THREE.PlaneGeometry(0.3, 0.3),
    new THREE.MeshBasicMaterial({
        color: "red",
        side: THREE.DoubleSide
    })
)

const sphereGeometry = new THREE.SphereGeometry(1, 8, 8)
const positionArray = sphereGeometry.attributes.position.array

let plane
for (let i=0; i<positionArrray.length; i+=3) {
    plane = planeMesh.clone()
    plane.position.x = positionArray[i]
    plane.position.y = positionArray[i+1]
    plane.position.z = positionArray[i+2]

    plane.lookAt(0, 0, 0)
    scene.add(plane)
}
```

SphereGeometry.attributes.position을 사용하면 각 점의 좌표를 가져올 수 있디. 가져온 점의 좌표(x, y, z)를 이용해 planeMesh를 그점에 위치하게 해준다. 점에 위치에서 planeMesh가 직각으로 서있게 된다. 구의 중심을 바라보게 하기 위해 lookAt을 사용해 구의 중신으로 향하게 한다. 

## Image panel

```jsx
class ImagePanel {
    constructor(info) {
        const texture = info.textureLoader.load(info.imageSrc)
        const material = new MeshBasicMaterial({
            map: texture,
            side: DubleSide
       })
       this.mesh = new Mesh(info.geometry, material)
       this.mesh.position.set(info.x, info.y, info.z)
       this.mesh.lookAt(0, 0, 0)

       this.sphereRotationX = this.mesh.rotation.x
       this.sphereRotationY = this.mesh.rotation.y
       this.sphereRotationZ = this.mesh.rotation.z

       info.scene.add(this.mesh)
    }
)
```

```jsx
const planeGeometry = new THREE.PlaneGeometry(0.3, 0.3),
const textureLoader = new THREE.TextureLoader()

const sphereGeometry = new THREE.SphereGeometry(1, 8, 8)
const spherePositionArray = sphereGeometry.attributes.position.array
const randomPositionArray = []
for (let i=0; i<spherePositionArray.length; i++){
    randomPotisition.push((Marh.random() - 0.5)*10)

const imagePanels = []
let imagePanel
for (let i=0; i<spherePositionArray.length; i+=3) {
    imagePanel = new ImagePanel({
        textureLoader,
        scene,
        geometry: planeGeometry,
        imageSrc: `/images/0${Math.random() * 5)}.jpg`
        x: spherePositionArray[i],
        y: spherePositionArray[i+1],
        z: spherePositionArray[i+2],
    })
    imagePanels.push(imagePanel)
}
```

이미지를 불러와 mesh를 만드는 ImagePanel class를 만든다. ImagePanel class에서 만들어진 이미지를 x,y,z에 놓고 중심을 바라보게 만든다.  (?? ImagePanel class에서 position setting, lookat, scene.add를 하는게 좋은가????????)

### button으로 이미지 위치 변경

```jsx
const buttonWrapper = document.createElement("div")
buttonWrapper.classList.add("buttons")

const randomButton = document.createElement("button")
randomButton.dataset.type = "random"
randomButton.style.cssText = "position: absolute; left: 20px; top: 20px"
randomButton.innerHTML = "random"
buttonWrapper.append(randomButton)

const sphereButton = document.createElement("button")
sphereButton.dataset.type = "sphere"
sphereButton.style.cssText = "position: absolute; left: 20px; top: 50px"
sphereButton.innerHTML = "sphere"
buttonWrapper.append(sphereButton)

document.body.append(buttonWrapper)
```

```jsx
function setShape(e) {
    const type = e.target.dataset.type
    const array

    switch (type) {
        case "random":
            array = randomPositionArray
            break
        case "sphere":
            array = spherePositionArray
            break
    }
    for (let i=0; i<imagePanels.length; i++) {
        gsap.to(
            imagePanels[i].mesh.position,
            {
                duration: 2,
                x: array[i*3],
                y: array[i*3 + 1],
                z: array[i*3 + 2],
            }
        )
        if (type === "random") {
            gsap.to(  
                imagePanels[i].mesh.rotation,
                {
                    duration: 2,
                    x: 0,
                    y: 0,
                    z: 0
                 }
            )
        } else {
            gsap.to(  
                imagePanels[i].mesh.rotation,
                {
                    duration: 2,
                    x: imagePanels[i].sphereRotationX,
                    y: imagePanels[i].sphereRotationY,
                    z: imagePanels[i].sphereRotationZ
                }
            )
        }
    }
} 

buttonWrapper.addEventListner("click", setShape)
```