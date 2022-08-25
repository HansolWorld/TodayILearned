# 15. let, const 키워드와 블록레벨 
## 15.1 var 키워드로 선언한 변수의 문제점
var 키워드로 만든 변수의 문제점을 살펴본다.
### 15.1.1 변수 중복 선언 허용
var 키워드는 중복 선언언을 허용한다.
```js
var x = 1
var y = 1

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100
// 초기화문이 없는 변수 선언문은 무시된다.
var y

console.log(x) // 100
console.log(y) // 1
```
x와 y 변수는 중복 선언되었다. 이처럼 var 키워드로 선언한 변수는 중복 선언이 가능하다.
### 15.1.2 함수 레벨 스코프
var 키워드로 선언한 변수는 오직 함수의 코드 블록만을 지역 스코프로 인정한다. 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.
```js
var x = 1
var i = 10

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0 i < 5 i++) {
  console.log(i) // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i) // 5
```
for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역 변수가 된다.
### 15.1.3 변수 호이스팅
var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 올라간것처럼 동작한다. 
```js
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo) // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123

console.log(foo) // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo
```
변수 선언문 이전에 변수를 참조하는 것은 호이스팅에 의해 에러를 발생시키지 않지만 프로그램상 맞지 않고 가독성이 떨어진다. 그리고 오류를 발생시킬 여지를 남긴다.
## 15.2 let 키워드
ES6에서 새로운 변선 키워드인 let, const를 도입했다.
### 15.2.1 변수 중복 선언 금지
var 키워드와 다르게 let은 중복 선언을 허용하지 않는다.
```js
var foo = 123
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456

let bar = 123
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456 // SyntaxError: Identifier 'bar' has already been declared
```
### 15.2.2 블록 레벨 스코프
let 키워드로 선언한 변수는 모든 코드 블록(함수, if, for, while ,try/catch 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
```js
let foo = 1 // 전역 변수

{
  let foo = 2 // 지역 변수
  let bar = 3 // 지역 변수
}

console.log(foo) // 1
console.log(bar) // ReferenceError: bar is not defined
```
let은 블록레벨 스코프를 따르기 때문에 bar변수는 참조할 수 없다.
### 15.2.3 변수 호이스팅
let 키워드로 선언한 벼수는 호이스팅이 발생하지 않는 것처럼 동작한다.
```js
console.log(foo) // ReferenceError: foo is not defined
let foo
```
let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조에러가 난다. **let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행 된다.** 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언단계가 먼저 실행되지만 초기화 단계는 선언문에 도달했을때 실행된다. 스코프의 시작 점부터 초기화 단계 시작 지점까지 변수를 참조할 수 없다. 이 구간을 ***일시적 사각지대***라고 부른다.
```js
let foo = 1 // 전역 변수

{
  console.log(foo) // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2 // 지역 변수
}
```
let 키워드로 선언한 변수의 경우 호이스팅이 발생하지 않는 것처럼 보인다. 만약 호이스팅이 발생하지 않는다면 위 전역 변수 foo 값이 출력되어야 한다. 하지만 참조 에러가 발생한다.
### 15.2.4 전역 객체와 let
let으로 선언한 변수는 전역 객체 프로퍼티가 아니다. window로 접근할 수 없다. let 전역 변수는 보이지 않는 개념적인 블록 내에 존재한다. 
```js
// 이 예제는 브라우저 환경에서 실행해야 한다.
let x = 1

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x) // undefined
console.log(x) // 1
```
## 15.3 const 키워드
const는 상수를 선언하기 위해 사용되지만, 상수만을 위해 사용되지는 않는다.
### 15.3.1 선언과  초기화
**const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.**
```js
const foo // SyntaxError: Missing initializer in const declaration
```
const로 선언한 변수는 let과 같이 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
```js
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo) // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1
  console.log(foo) // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo) // ReferenceError: foo is not defined
```
### 15.3.2 재할당 금지
**const 키워드로 선언한 변수는 재할당이 금지된다.**
```js
const foo = 1
foo = 2 // TypeError: Assignment to constant variable.
```
### 15.3.3 상수
const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수값을 변경할 수 없다. **상수는 재할당이 금지된 변수를 말한다.**</br>
일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 하고 언더스코어로 구분해 스네이크 케이스로 표현하는 것이 일반적이다.
```js
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1

// 세전 가격
let preTaxPrice = 100

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE)

console.log(afterTaxPrice) // 110
```
### 15.3.4 const 키워드와 객체
**const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.** 객체의 경우 재할당 없이 값을 변경할 수 있기 떄문이다.
```js
const person = {
  name: 'Lee'
}

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = 'Kim'

console.log(person) // {name: "Kim"}
```
const 키워드는 재할당을 금지할 뿐이지 불변은 아니다.
## 15.4 var vs. let vs. const
- ES6를 사용한다면 var 키워드는 사용하지 않는것이 좋다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는 원시값과 객체는 const를 사용한다. const는 재할당 금지로 var, let보다 안전하다.
