# 33장. 7번째 데이터 타입 Symbol
## 33.1 심벌이란?
ES6에서 도입된 7번째 타입으로 변경 불가능한 원시 타입의 값이다. 다른값과 중복되지 않는 유일무이한 값으로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용된다. 
## 33.2 심벌 값의 생성
### 33.2.1 Symbol 함수
심벌 값은 리터럴 표기법으로 생성할 수 없고 Symbol 함수를 호출해 생성한다. 심벌의 값은 외부로 노출되지 않고 **다른 값과 절대 중복되지 않는 유일무이한 값이다.**
```js
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol()
console.log(typeof mySymbol) // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol)        // Symbol()
```
Symbol 함수에는 인수로 문자열을 전달할 수도 있다. 이 문자열은 심벌 값에대한 설명으로 디버깅 용도로만 사용되며 심벌값에 영향을 주지 않는다. 
```js
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol('mySymbol')
const mySymbol2 = Symbol('mySymbol')

console.log(mySymbol1 === mySymbol2) // false
```
심벌값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 만든다.
```js
const mySymbol = Symbol('mySymbol')

// 심벌도 레퍼 객체를 생성한다
console.log(mySymbol.description) // mySymbol
console.log(mySymbol.toString())  // Symbol(mySymbol)
```
심벌은 문자나 숫자로 변경 불가능하다.
```js
const mySymbol = Symbol()

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + '') // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol)     // TypeError: Cannot convert a Symbol value to a number
```
심벌값은 불리언 타입으로 암묵적으로 타입 변환된다.
```js
const mySymbol = Symbol()

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol) // true

// if 문 등에서 존재 확인을 위해 사용할 수 있다.
if (mySymbol) console.log('mySymbol is not empty.')
```
### 33.2.2 Symbol.for / Symbol.keyFor 메서드
Symbol.for 매서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌값의 쌍들이 저장되어 있는 ***전역 심벌 레지스트리***에서 해당 키와 일치하는 심벌 값을 검색한다.
- 검색에 성공하면 새로운 심벌값을 생성하지 않고 검색된 심벌 값을 반환
- 검색에 실패하면 새로운 실벙값을 생성하여 Symbol.for메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후 생성된 심벌값 반환
```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol')
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol')

console.log(s1 === s2) // true
```
자바스크립트 엔진이 심벌값 저장소인 전역 심벌 레지스트리에서 심벌값을 검색할 수 있는 키를 지정할 수 없으므로 전역심벌 레지스트리에 등록되어 관리되지 않는다. 하지만 Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌값을 단 하나만 생성하여 전역심벌 레지스트리를 통해 공유할 수 있다.   
Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다,
```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol')
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1) // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo')
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2) // -> undefined
```
## 33.3 심벌과 상수
값에 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우 상수 값이 변경되거나 중복되는걸 막기위해 유일무이한 심벌 값을 사용한다.
```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right')
}

const myDirection = Direction.UP

if (myDirection === Direction.UP) {
  console.log('You are going UP.')
}
```
자바스크립트에서는 enum(열거형)을 지원하지 않는다. Symbol을 사용해 enum을 흉내낼 수 있다.
```js
// JavaScript enum
// Direction 객체는 불변 객체이며 프로퍼티는 유일무이한 값이다.
const Direction = Object.freeze({
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right')
})

const myDirection = Direction.UP

if (myDirection === Direction.UP) {
  console.log('You are going UP.')
}
```
## 33.4 심벌과 프로퍼티 키
객체의 프로퍼티키는 빈 문자열을 포함한 모든 문자열 또는 심벌값으로 만들 수 있다. 심벌값으로 프로퍼티 키를 만들기 위해서는 심벌값에 대괄호를 사용해야 한다. 
```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
}

obj[Symbol.for('mySymbol')] // -> 1
```
**심벌값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**
## 33.5 심벌과 프로퍼티 은닉
심벌값을 프로퍼티키로 사용하여 생성한 프로퍼티는 for ... in문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다. 즉, 외부에 노출할 필요가 없는 프로퍼티는 심벌값으로 은닉할 수 있다.
```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol('mySymbol')]: 1
}

for (const key in obj) {
  console.log(key) // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)) // []
console.log(Object.getOwnPropertyNames(obj)) // []
```
하지만 완전히 숨길 수 있는것은 아니다. ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌값을 프로퍼티 키로 사용한 프로퍼티를 찾을 수 있다.
```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol('mySymbol')]: 1
}

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj)) // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0]
console.log(obj[symbolKey1]) // 1
```
## 33.6 심벌과 표준 빌트인 객체 확장
표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 메서드와 사용자 정의 메서드간의 중복으로 문제가 생길 수 있기 때문이다. 중복을 없애기 위해 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 중복의 위험 없이 빌트인 객체를 확장할 수 있다.
```js
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0)
}

[1, 2][Symbol.for('sum')]() // -> 3
```
## 33.7 Well-known Symbol
자바스크립트가 기본 제공하는 빌트인 심벌값이 있다. 빌트인 심벌값은 Symbol 함수의 프로퍼티에 할당되어 있다. 자바스크립트가 기본 제공하는 빌트인 심벌값을 ECMAScript에서는 ***Well-know Symboll***이라 부른다.   
for ... of 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가지며, Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.   
빌트인 이터러블이 아닌 일반 객체를 이터러블 처럼 동작하고 싶다면 Symbol.iterator를 키로 갖는 메서드를 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.
```js
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1
    const max = 5
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 }
      }
    }
  }
}

for (const num of iterable) {
  console.log(num) // 1 2 3 4 5
}
```