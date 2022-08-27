# Three Transform

mesh의 위치를 이동하거나 모양을 바꾸는 등 변환을 주는 방법에 대해 알아볼 것이다.

## mesh 위치이동

```jsx
const geometry = new THREE.BoxGeometry(1, 1, 1)
const material = new THREE.MeshBasicMaterial({
    color: "red"
})

const mesh = new THREE.Mesh(geometry, material)
scene.add(mesh)

renderer.render(scene, camera)
```

mesh의 이동을 관찰하기 위해서 geometry와 material을 설정해 mesh를 만들어 준다. 

```jsx
mesh.position.y = 2
// mesh.position.set(0, 2, 0)
```

mesh의 위치를 변경하기 위해서는 mesh.position을 사용한다. 각 이동할 x, y, z에 접근해 변경을 해주면 된다. x, y, z에 각각 접근을 할수도 있지만, set을 사용하면 한번에 x, y, z에 접근할 수 있다.

## mesh의 크기조절

```jsx
mesh.scale.x = 2
// mesh.scale.set(2, 0, 0)
```

mesh의 크기를 조절하기 위해서는 mesh.scale을 사용하면 된다. 각 조절한 x, y, z방향에 접근해 n배를 할수 있다. x, y, z에 각각 접근할수도 있지만, set을 사용해 한번에 x, y, z를 조절할 수 있다.

## mesh 회전

```jsx
mesh.rotation.x = 45 // radian값이 들어간다.
// mesh.rotation.x = THREE.MathUtils.degToRad(45) // 45도가 들어간다.\
// mesh.rotation.x = Math.PI / 4
```

mesh를 회전하기 위해서는 rotation을 쓰면된다. rotation의 값은 라디안값으로 우리가 사용하는 각도와는 차이가 있다. 각도를 라디안으로 바꾸기 위해서는 degToRad를 쓰면 된다. 45도를 표현하고 싶으면 degToRad(45)로 쓴다. 또다른 방법은 Math.PI를 이용하는 방법이다. Math.PI가 180도를 의마한다. 즉 45도를 돌리고 싶다면 Math.PI/4를 하면 된다. 

```jsx
mesh.rotation.reorder("YXZ")
mesh.rotation.y = THREE.MathUtils.degToRad(45)
mesh.rotation.x = THREE.MathUtils.degToRad(20)
```

rotation을 하게되면 축이 정면을 보고 rotation을 하게된다. rotation되는 축을 mesh의 정면을 기준으로 하기 위해서는 ratation.reorder를 사용해야 한다.

## mesh 그룹

```jsx
const geometry = new THREE.BoxGeometry(1, 1, 1)
const material = new THREE.MeshBasicMaterial({
    color: "red"
})

const group1 = new THREE.Group()
const box1 = new THREE.Mesh(geometry, matrial)

const group2 = new THREE.Group()
const box2 = box1.clone()
box2.scale.set(0.3, 0.3, 0.3)
group2.position.x = 2

const group3 = new THREE.Group()
const box3 = box2.clone()
box3.position.x = 0.5

group3.add(box3)
group2.add(box2, group3)
group1.add(box1, group2)
scene.add(group1)
```

mesh에서 position, rotation을 사용하는것을 group으로 묶어 group내에 있는 mesh들을 동시에 변환줄 수 있다.