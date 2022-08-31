# 22장 this
## 22.1 this 키워드
객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리 단위로 묶어놓은 복합적인 자료구조이다. 동작을 나타내는 메서드는 자신이 속한 상태를 나타내는 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 자신이 속한 객체의 프로퍼티를 참조하려면 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**   
객체 리터럴 방식으로 생성한 객체는 메서드 내부에서 재귀적으로 자신이 속한 객체를 가리키는 식별자를 참조할 수 있다.
```js
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * circle.radius
  }
}

console.log(circle.getDiameter()) // 10
```
객체 리터럴은 circle 변수에 할당되기 직전에 평가된다. getDiameter 메서드가 호출되는 시점에 이미 객체 리터럴의 평가가 완료되어 객체가 만들어져 있고 circle 식별자에 생성된 객체가 할당된 이후이기 때문에 내부에서 circle 식별자를 잠조할 수 있다.   
```js
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius
}

Circle.prototype.getDiameter = function () {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  return 2 * ????.radius
}

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5)
```
생성자 함수 내부에서 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다. 하지만 생성자 함수에 의한 객체 생성 방식은 생성자 함수를 정의하고 new 연상자를 사용해 객체를 만드는 단계가 필요하기 때문에 인스턴스가 생성되기 전에 인스턴스가 누군지 모른다. 따라서 자신이 속한 깩체, 자신이 생성할 인스턴스를 가리키기 위한 특수한 식별자가 this가 필요하다.   
**this는 자신이 속한 객체, 생성할 인스턴스를 가리키는 참조 변수로 this를 통해 자신이 속한 객체나 생성할 인스턴스의 프로퍼티, 메서드를 참조할 수 있다.**   
자바스크립트 엔진은 암묵적으로 this를 생성하고 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다. **this가 가리키는 값은 호출 방식에 따라 동적으로 결정된다.**
```js
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius
  }
}

console.log(circle.getDiameter()) // 10
```
객체 리터럴 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
```js
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius
}

// 인스턴스 생성
const circle = new Circle(5)
console.log(circle.getDiameter()) // 10
```
생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.   
```js
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this) // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this) // window
  return number * number
}
square(2)

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this) // {name: "Lee", getName: ƒ}
    return this.name
  }
}
console.log(person.getName()) // Lee

function Person(name) {
  this.name = name
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this) // Person {name: "Lee"}
}

const me = new Person('Lee')
```
this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미를 가진다. strict mode가 적용된 일반 함수에서는 this를 사용할 필요가 없기 때문에 undefined가 바인딩 된다.
## 22.2 함수 호출 방식과 this 바인딩
**this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.**   
랙시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에서 상위 스코프를 결정하지만 this 바인딩은 함수 호출 시점에서 결정된다.  
함수 호출 방식은 4가지가 있다
1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
```js
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this)
}

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo() // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo }
obj.foo() // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo() // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' }

foo.call(bar)   // bar
foo.apply(bar)  // bar
foo.bind(bar)() // bar
```
### 22.2.1 일반 함수 호출
**this에는 전역 객체가 바인딩 된다.**
```js
function foo() {
  console.log("foo's this: ", this)  // window
  function bar() {
    console.log("bar's this: ", this) // window
  }
  bar()
}
foo()
```
**함수 내부의 this는 전역객체가 바인딩된다.** 하지만 this는 객체의 프로퍼티나 메서드를 참조하기 위한 특수한 식별자이기 떄문에 일반함수에서는 this가 무의미하다. 따라서 strict mode가 적용된 일반함수 내부에서는 undefined가 바인딩 된다.
```js
function foo() {
  'use strict'

  console.log("foo's this: ", this)  // undefined
  function bar() {
    console.log("bar's this: ", this) // undefined
  }
  bar()
}
foo()
```
중첩함수, 콜백함수도 일반함수로 호출된다면 내부의 this에도 전역 객체가 바인딩 된다.
```js
var value = 1

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this) // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this) // window
      console.log("callback's this.value: ", this.value) // 1
    }, 100)
  }
}

obj.foo()
```
**일반 함수로 호출되는 모든 함수 내부의 this에는 전역 객체가 바인딩된다.**   
콜백함수나 중첩함수에 this가 전역 객체를 바인딩 하는것은 오ㅚ부함수인 메서드와 중첩함수 또는 콜백함수의 this가 일치하지 않는다는 것은 헬퍼 함수로 동작하기 어렵게 만든다.   
메서드 내부의 중첩 함수나 홀백함수의 this 바인딩을 메서드의 this 바인딩과 일치시키는 작업이 필요하다.
```js
var value = 1

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value) // 100
    }, 100)
  }
}

obj.foo()
```
또는, this를 명시적으로 바인딩 할수 있는 Function.prototype.apply/vall/bind 메서드를 사용하는 방법도 있다.
```js
var value = 1

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(function () {
      console.log(this.value) // 100
    }.bind(this), 100)
  }
}

obj.foo()
```
화살표 함수를 사용해 this를 바인딩 할수도 있다.
```js
var value = 1

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100) // 100
  }
}

obj.foo()
```
### 22.2.2 메서드 호출
메서드 내부의 this에는 메서드를 호출한 객체가 바인딩 된다. this를 소유한 메서드가 아닌 호출한 객체가 바인딩 된다.
```js
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name
  }
}

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()) // Lee
```
getName 메서스는 person 객체의 메서드로 프로퍼티에 바인딩된 함수다. 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하고 getName 프로퍼티가 이 함수 객체를 가리키고 있는거다.   
getName 프로퍼티가 가리키는 함수 객체는 다른 객체의 메서드가 될수도 있고 일반 함수로 호출 될수도 있다.
```js
const anotherPerson = {
  name: 'Kim'
}
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()) // Kim

// getName 메서드를 변수에 할당
const getName = person.getName

// getName 메서드를 일반 함수로 호출
console.log(getName()) // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```
this는 프로퍼티로 메서드를 가리키고 있는 객체와 상관없이 호출한 객체에 바인딩 된다. 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 호출한 객체에 바인딩 된다.
```js
function Person(name) {
  this.name = name
}

Person.prototype.getName = function () {
  return this.name
}

const me = new Person('Lee')

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()) // ① Lee

Person.prototype.name = 'Kim'

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()) // ② Kim
```
### 22.2.3 생성자 함수 호출
생성자 함수 내부의 this는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.
```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius
  this.getDiameter = function () {
    return 2 * this.radius
  }
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5)
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10)

console.log(circle1.getDiameter()) // 10
console.log(circle2.getDiameter()) // 20
```
### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출
apply. bind, call 메서드는 Function.prototype의 메서드로 상속받아 사용할 수 있다.
```js
function getThisBinding() {
  return this
}

// this로 사용할 객체
const thisArg = { a: 1 }

console.log(getThisBinding()) // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)) // {a: 1}
console.log(getThisBinding.call(thisArg)) // {a: 1}
```
**apply, call 메서드의 본질적인 기능은 함수를 호출하는 것이다.** apply, call 메서드는 첫번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩 한다. appply, call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다. 
```js
function getThisBinding() {
  console.log(arguments)
  return this
}

// this로 사용할 객체
const thisArg = { a: 1 }

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]))
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3))
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}
```
apply는 인수를 배열의 형태로 전달하고 call은 전달할 인수를 ,로 구분하여 전달한다.   
```js
function convertArgsToArray() {
  console.log(arguments)

  // arguments 객체를 배열로 변환
  // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성한다.
  const arr = Array.prototype.slice.call(arguments)
  // const arr = Array.prototype.slice.apply(arguments)
  console.log(arr)

  return arr
}

convertArgsToArray(1, 2, 3) // [1, 2, 3]
```
arguments는 유사배열이지 배열이 아니기 때문에 slice와 같은 배열 메서드를 사용할 수 없다. 이떄 apply, call을 사용해 사용하게 된다.   
bind는 apply,call과 달리 함수를 호출하지 않는다. 첫번째 인수로 this 바인딩으로 교체된 함수를 새롭게 생성해 반환한다.
```js
function getThisBinding() {
  return this
}

// this로 사용할 객체
const thisArg = { a: 1 }

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)) // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()) // {a: 1}
```
bind 메서드는 this와 메서드 내부의 중첩, 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.
```js
const person = {
  name: 'Lee',
  foo(callback) {
    // ①
    setTimeout(callback, 100)
  }
}

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`) // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
})
```
person.foo의 콜백함수가 호출되기 전에 1의 시점에서 this는 foo메서드를 호출한 person 객체를 가리키지만, 콜백함수와 힐반 함수로서 호출된 2의 시점에서는 this는 전역 객체 window를 가리킨다. 이때 외부함수 person.foo의 this와 콜백 함수 내부의 this를 일치 시켜주기 위해 bind 메서드를 사용한다.
```js
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100)
  }
}

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`) // Hi! my name is Lee.
})
```
|함수 호출 방식|this 바인딩|
|---|---|
|일반 함수 호출|전역 객체|
|메서드 호출|메서드를 호출한 객체|
|생성자 함수 호출|생성자 함수가 생성할 인스턴스|
|Function.prototype.apply/call/bind 메서드에 의한 간접 호출|Function.prototype.apply/call/bind 메서드의 첫번쨰 인수로 전달된 객체|