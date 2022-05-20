# var, let, const

자바스크립트에서 변수를 선언하기 위해서는 var, let, const를 사용한다. es5까지는 var을 사용했지만, es6에 let, const가 등장하면서 변수 선언에 방식이 달라졌다.

각 변수의 차이를 알아보고 이를 이해하기 위한 호이스팅과 스코프를 알아본다.

## 변수선언

우리가 변수를 선언하게 되면 컴퓨터는 메모리에 값을 저장한다. 메모리는 주소를 가지고 있고 각 값은 고유의 주소를 가지게 된다. 주소를 기억하기는 힘들다. 주소에 접근하기 위해 우리는 변수명을 사용한다. 변수를 선언함으로써 메모리 공간을 확보하고 해당 공간에 주소를 연결해 값을 저장한다. 

이처럼 변수에 값을 저장하는 것을 할당이라 하며, 변수에 저장된 값을 읽어 들이는 것을 참조라 한다.

## var

var을 알기위해 먼저 예제를 확인해보자

```jsx
var index = 0
var sum = 0
for (var index=0; index<11; index++) {
    sum = sum + index
}

console.log(sum) // 55
console.log(index) // 11
```

index는 11이 나온다. 11이 나오는 이유가 뭘까? var은 중복선언이 가능하다. 처음 index를 0으로 할당했지만 for loop에서 index를 다시 선언해준다. 

var에는 3가지 특징이 있다.

1. 중복선언을 허용한다. 
    
    ```jsx
    var index = 0
    // ... ... //
    
    var index
    
    console.log(index) // undefined
    ```
    
    중복선언은 예상하지 못한 결과를 가져올 수 있다. 여럿이 사용하는 코드에서 A가 처음 index를 0으로 초기화했다. 이후 B가 index를 재선언해 사용한다. A가 다시 index를 사용하려 할때 A의 의도와는 다른 결과를 가져올 수 있다. 
    
2. 함수레벨 스코프를 가진다.
    
    ```jsx
    var x = 1
    
    if (ture) {
        var x = 100
    }
    
    console.log(x) // 100
    ```
    
    if, for의 내부에서 변수를 선언한다면  함수레벨의 스코프로 전역변수로 선언한다. **스코프(scope)?**는 식별자(identifier)를 찾을떄 자바스크립트에서 확인하는 범위를 말한다. 상위 스코프는 하위 스코프에 접근할 수 있지만. 하위 스코프는 상위 스코프에 접근할 수 없다. ****
    
3. 변수 호이스팅
    
    ```jsx
    console.log(apple) // undefined
    
    var apple
    
    apple = "사과"
    console.log(apple) // 사과
    ```
    
    javascript에는 **hoisting?**란 특징이 있다. 변수의 선언문이 코드의 선두로 끌어 올려진것처럼 동작하는 것이다.  처음 console.log(apple)은 apple 변수가 선언되지 않았기 떄문에 애러가 나야 맞지만 javascript의 엔진이 변수선언인 var apple을 먼저 실행시키게 된다. 따라서 apple은 undefined로 선언이 먼저 된후 console.log(apple)을 실행하게 되 에러가 아닌 undefined를 출력하게 된다.
    
    이런 hoisting은 가독성이 떨어지고 오류의 발생 가능성을 증가시킨다. 
    

## let

var과 다르게 let은

1. 변수의 중복 선언을 금지한다.
    
    ```jsx
    let index = 0
    
    let index = 1 // SyntaxError
    ```
    
    let으로 선언한 변수와 동일 이름의 변수를 선언할 수 없다. 
    
2. 블록 레벨 스코프이다.
    
    ```jsx
    function foo() {
        let a = "local"
        if (true) {
            let a = "block"
            console.log(a) // if 블록 안에 있기 떄문에 block이 찍힌다.
        }
        console.log(a); // 함수 스코프 안에 변수이기 떄문이 local이 찍힌다.
    }
    
    let a = "global"
    foo()
    
    console.log(a) // 전역 스코프 안에 변수이기 때문에 global이 찍힌다.
    ```
    
    let 은 블록레벨 스코프를 가지기 떄문에 if, for에서 선언한 let a를 참조하기 때문이 block가 찍힌다.  만약 a를 선언하지 않으면 local이 찍히게 된다. es5에서는 global, local scope만 가능했지만 es6로 오면서 let, const로 block scope도 가능하게 됬다.
    
3. 변수 호이스팅이 다르다.
    
    ```jsx
    console.log(index) // referenceError
    let index
    ```
    
    let은 호이스팅이 발생하지 않는 것처럼 작동한다. var과 다르게 참조에러가 나면서 코드가 작동하지 않는다. javascript의 엔진은 소스평가 과정에서 변수, 함수 선언문등을 위로 올리는 평가 과정을 가진다. 이 평가 과정에도 선언 단계와 초기화 단계가 있다. 선언 단계를 통해 스코프에 변수 식별자를 등록해 javascript에 해단 식별자의 존재를 알린다. 이후 초기화 단계에서 스코프에 등록된 변수 식별자의 값을  초기화 한다. 
    
     var은 코드 평가과정에서 선언, 초기화 단계가 이루어져 해당 변수에 접근하기 전에 호출하더라도 에러가 발생하지 앉는다.  let은 선언 단계계와 초기화 단계가 분리되어 이루어 진다. 선언 단계에서는 변수의 존재를 알리지만 어떤 값을 가진지 모른다. 따라서 일시적 사각지대(TDZ)가 발생하게 된다. let의 초기화 단계는 변수가 선언된 곳에서 일어나게 된다. 따라서 변수에 undefined가 할당되게 되고 사용할 수 있게 된다. 
    

## const

const는 let의 기본 성질을 모두 포함한다. 추가로 3가지의 특징이 있다.

1. 선언과 초기화를 동시에 한다.
    
    ```jsx
    const a // SyntaxError
    
    const a = "a"
    ```
    
    const는 선언과 동시에 초기화가 이루어진다. 초기화를 위해 값을 넣어줘야 한다.
    
2. 재할당을 금지한다.
    
    ```jsx
    const a = 100
    a = 10 // Uncaught TypeError
    ```
    
    const는 재할당은 할수가 없다. const는 상수를 선언하기위해 만들어 졌다. 
    
3. 상수 선언을 한다.
    
    ```jsx
    const B = {
        b: 123
    }
    B.b = 999
    console.log(B.b) // 999
    ```
    
    **상수란?** 재선언이 불가능한 값으로 초기값을 필요로한다. 대입연산자로 재할당이 불가능하며, 동일한 이름으로 같은 스코프내의 변수 및 함수로 사용 불가능하다. 상수로 할당된 객체 속성은 보호되지 않는다는 특징을 가지고 있다. const로 선언하면 원시값을 보호할 수 있다는 점에서 유지, 보수에 좋다. 단, 객체로 만들어 참조 타입으로 만들경우 객체 내부의 값은 변경이 가능하다.
    

## 왜 var을 사용하면 안될까?

1. var을 사용하게 되면 변수의 중복을 허용하기 떄문에 예기치 못한 결과를 가져올 수 있다. 여럿이 개발하는 곳에서는 변수의 혼동은 예상과 다른 결과를 가져오기 때문에 var을 지양하는 것이 좋다.
2. 변수가 할당 되기전에 참조할 수 있기 때문에 혼란을 줄수가 있다. 호스팅으로인해 원하는 위치에서만 사용하는 것이 아닌 이전에 참조가 될 수 있기때문에 var을 지양하는것이 좋다.
3. 변수간의 접근을 허용한다. var은 함수레벨 스코프로 변수의 사용 범위가 let에 비해 넓다. 함수 레벨 스코프인 let을 사용하는 것을 지향한다.

### reference

[https://youtu.be/ZU4MXkwDb9g](https://youtu.be/ZU4MXkwDb9g)