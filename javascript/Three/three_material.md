# three_material

## MeshBasicMaterial

```jsx
const material = new THREE.MeshBasicMaterial({
    color: "yellow"
})
```

MeshBasicMaterial은 입체감이 없다. 면에 따른 빛의 변화가 없고 한가지 색을 띈다. 빛, 그림자가 없기때문에 빛이 필요 없다.

## MeshLambertMaterial, MeshPhongMaterial

```jsx
const material = new THREE.MeshLambertMaterial({
    color: "yellow"
})
```

```jsx
const material = new THREE.MeshPhongtMaterial({
    color: "yellow",
    shininess: 1000
})
```

MeshLambertMaterial같은 경우는 하이라이트, 반사광이 없다. MeshPhongMaterial같은 경우는 하이라이트나 반사광의 표현이 가능하다. MeshPhongMaterial을 쓰게되면 반짝거리는 하이라이트를 볼수 있다. 따라서 shininess를 조절할 수 있다. 값이 클수록 반짝이는 값이 커진다. 

## MeshStandardMaterial

```jsx
const material = new THREE.MeshStandardMaterial({
    color: "yellow",
    roughness: 0.3,
    metalness: 1
})
```

MeshStandartMaterial은 하이라이트와 반사광이 있다. MeshPhongMaterial과 다르게 옵션값이 다르다. roughness를 사용해서 거친도를 변경할 수 있다. 1이 거칠고 0일수록 반사광이 커진다. metalness를 사용하면 금속 느낌을 낼수있다. 값이 클수록 금속효과가 커진다. 성능은 MeshPhongMaterial이 미세하게 빠르다.

## flatShading, side

```jsx
const material = new THREE.MeshStandardMaterial({
    color: "yellow",
    flatshding: true,
    // side: THREE.FrontSide
    // side: THREE.BackSide
    side: THREE.DubleSide
})
```

각지게 표현할 수 있다. geometry의 설정값에 따라서 면을 각지게 할수있다. side를 설정하면 어느면을 보이게 할지 설정할 수 있다. THREE.FrontSide의 경우 앞면만 보이고 THREE.BaskSide일 경우 뒷면만 보이게 된다. 양쪽면을 보이게 하려면 THREE.DubleSide로 설정해야한다.

## Mesh texture loading

```jsx
const textureLoader = new THREE.TextureLoader()
const texture = textureLoader.load(
    "/textures/brick.jpg",
    () => {
        console.log("로드 완료")
    },
    () => {
        console.log("로드 중")
    },
    () => {
        console.log("로드 에러")
    }
)

texture.wrapS = THREE.RepeatWrapping
texture.offset.x = 0.3

texture.wrapT = THREE.RepeatWrapping
texture.offset.y = 0.3

texture.repeat.x = 2

texture.rotation = THREE.MathUtils.degToRad(30)
texture.center.x = 0.5
texture.center.y = 0.5

const material = new THREE.MeshLambertMaterial({
    map: texture
})
```

3d texture를 입력하면 이미지들이 많다.  textureLoader.load(이지 경로, 완료 메서드, 로드중 메서드, 에러 메서드)로 만들 수 있다. 펑션을 만들어 로드에 따른 함수 실행을 할 수 있다. texture.offset으로 texture의 위치를 변경할 수 있다. 단, 위치를 변경할 경우 끝 pixel이 늘어지게 된다. RepeatWrapping을 통해 texture를 반복할 수 있다. texture.repeat을 사용하면 이미지를 반복할 수 있다. repeat을 할때 wrap을 사용하지 않으면 pixcel이 늘어나기 떄문에 써줘야 한다. texture.rotation을 사용하면 texture를 회전할 수있다. 회전을 할때 기준점을 잡기 위해서는 texture.center로 설정하면 된다.

## LoadManager

```jsx
const loadingManager = new THREE.LoadingManager()

loadingManager.onStart = () => {
    console.log("로드 시작")
}
loadingManager.onProgress = img => {
    console.log(img + " 로드")
}
loadingManager.onLoad = () => {
    console.log("로드 완료")
}
loadingManager.onError = () => {
    console.log("에러")
}

const textureLoader = new THREE.TextureLoader(loadingManager)
const texture1 = textureLoader.load("/textures/bric1.jpg")
const texture2 = textureLoader.load("/textures/bric2.jpg")
const texture3 = textureLoader.load("/textures/bric3.jpg")
const texture4 = textureLoader.load("/textures/bric4.jpg")
const texture5 = textureLoader.load("/textures/bric5.jpg")
const texture6 = textureLoader.load("/textures/bric6.jpg")

texture1.magFilter = THREE.NearestFilter // pixcel을 깨끗하게 한다. 

const materials = [
    new THREE.MeshBasicMaterial({map: texture1}), // right
    new THREE.MeshBasicMaterial({map: texture2}), // left
    new THREE.MeshBasicMaterial({map: texture3}), // top
    new THREE.MeshBasicMaterial({map: texture4}), // bottom
    new THREE.MeshBasicMaterial({map: texture5}), // front
    new THREE.MeshBasicMaterial({map: texture6}), // back
}

const geometry = new THREE.BoxGeometry(2, 2, 2)
const mesh = new THREE.Mesh(geometry, materials)
```

LoadingManager 사용으로 여러 이미지를 loading할 수 있다. 이미지 loading을 하면서 작업할 펑션을 onstart, inProgress, onLoad, onError에 설정할 수 있다. 여러 meterial을 하나의 geometry에 담기 위해 material 배열을 만든다. 배열의 순서는 [right, left, top, bottom, front, back] 순서로 한다. 배열로 만든 material을 mesh객체를 만들때 사용한다.

## MeshToonMaterial

```jsx
const material = new THREE.MeshToontMaterial({
    color: "yellow",
    gradientMap: gradientTexture
})
```

2D애니메이션의 느낌으로 기본값은 2톤으로 되어 있다. 색상의 단계를 추가하기 위해서는 색단계 이미지를 만들어야 한다. textureLoader를 사용해 이미지를 불러온 후 MeshToonMaterial의 gradientMap에 넣어줘야한다.

## MeshNormalMaterial

```jsx
const material = new THREE.MeshNermalMaterial({
})
```

법선의 따라서 색상의 톤이 변경한다. 

## MeshMatcapMaterial

```jsx
const matcapTexture = textureloader.load("/textures/matcap/material.jpg")

const material = new THREE.MeshMatcaptMaterial({
    matcap: matcaptexture
})
```

matcap을 사용하면 입체감을 줄 수 있다.  matcap 속성을 사용해 mapcaptexture를 입혀준다.

## MeshStandardMaterial

```jsx
const textureLoader = new THREE.TextureLoader(loadingManager)
const normalTexture = textureLoader.load("/textures/normal.jpg")
const roughnessTexture = textureLoader.load("/textures/roughness.jpg")
const ambientTexture = textureLoader.load("/textures/ambient.jpg")

const material = new THREE.MeshStandardMaterial({
    color: "yellow",
    normalMap: normalTexture, // 입체감이 살아난다.
    roughnessMap: roughnessTexture, // 거칠기를 표현할 수 있다.
    aoMap: ambientTexture, // 그림자 부분을 어둡게 할 수 있다.
    aoMapIntensity: 5 // 어두운 정도를 나타낸다.
    
}) 
```

texture이미지를 사용해 입체감, 그림자처리를 할 수 있다.

## EnvironmentMap

```jsx
const cubeTextureLoader = new THREE.CubeTextureLoader()
const envTexture = cubeTextureLoader
    .setPath("/textures/cubemap/")
    .load([
        "px.png", "nx.png",
        "py.png", "ny.png",    
        "pz.png", "nz.png",
    ])

const material = new THREE.MeshStandardMaterial({
    envMap: envTexture,
    metalness: 2,
    roughness: 0.1
})
```

배경 이미지를 설정할 수 있다.(download site : poly haven) HDR이미지를 다운 받는다. HDR이미지를 6개로 나눠줘야 한다. hdri to cubemap 사이트를 이용하자. cube를 사용하기 위해서는 CubetextureLoader로 이미지를 불러와야 한다. 6개의 이미지를 [px, nx, py, ny, pz, nz] 순서로 넣어준다. envMap을 사용하기 위해서는 metalness, roughness를 조절해줘야 한다. 

## Skybox

```jsx
const cubeTextureLoader = new THREE.CubeTextureLoader()
scene.background = cubeTextureLoader
    .setPath("/textures/cubemap/")
    .load([
        "px.png", "nx.png",
        "py.png", "ny.png",    
        "pz.png", "nz.png",
    ])

```

scene.background에 cubeTexture를 넣어주면 배경을 넣어준다.