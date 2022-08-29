# 20장. strict mode
## 20.1 strict mode란?
```js
function foo() {
  x = 10
}
foo()

console.log(x) // ?
```
x는 foo안에서 정의되었기 때문에 전역 스코프에 x는 존재하지 않지만 위 결과는 10이 나온다. 자바스크립트는 암묵적으로 전역 객체에 x 프로퍼티는 전역변수처럼 사용할 수 있다. 이런 현상을 ***암묵적 전역***이라 한다.   
개발자의 의도와 다르게 사용될 수 있으므로 반드시 var, let, const를 사용해 변수를 선언해야 한다. 하지만 오타나 문법적 지식 미비로 실수가 생길수 있어 장치적으로 해결책을 찾아야 한다.   
ES5부터 strict mode가 추가되어 문법적으로 오류를 발생시킬 가능성이 높거나 최적화 작업에 문제를 일이킬 수 있는 코드에 대해 명시적 에러를 발생 시킨다.   
ESLint 같은 린트 도구를 사용해도 유사한 효과를 얻을 수 있다. 
## 20.2 strict mode의 적용
전역의 선두 또는 함수 몸체의 선두에 use strict를 추가하면 된다.
```js
'use strict'

function foo() {
  x = 10 // ReferenceError: x is not defined
}
foo()
```
함수 몸체 안에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.
```js
function foo() {
  'use strict'

  x = 10 // ReferenceError: x is not defined
}
foo()
```
코드 선두에 위치하지 않으면 제대로 동작하지 않는다.
```js
function foo() {
  x = 10 // 에러를 발생시키지 않는다.
  'use strict'
}
foo()
```
## 20.3 전역에 strict mode를 적용하는 것은 피하자
전역에 적용한 strict mode는 스크립트 단위로 전용된다.
```js
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict'
  </script>
  <script>
    x = 1 // 에러가 발생하지 않는다.
    console.log(x) // 1
  </script>
  <script>
    'use strict'

    y = 1 // ReferenceError: y is not defined
    console.log(y)
  </script>
</body>
</html>
```
스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에만 영향을 준다.   
strict mode와 non-strict mode를 혼용해서 쓰는건 바람직 하지 않다. 전역으로 strict mode를 쓸경우 외부 모듈이 non-strict mode면 에러를 발생시킬수 있다. 이럴경우는 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실햄 함수의 선두에 strict mode를 적용한다.
```js
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict'

  // Do something...
}())
```
## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자
함수단위로 strict mode를 적용하는 것도 바람직 하지 않다. strict mode는 즉시 실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.
```js
(function () {
  // non-strict mode
  var lеt = 10 // 에러가 발생하지 않는다.

  function foo() {
    'use strict'

    let = 20 // SyntaxError: Unexpected strict mode reserved word
  }
  foo()
}())
```
## 20.5 strict mode가 발생시키는 에러
### 20.5.1 암묵적 전역
선언하지 않은 변수를 참조
```js
(function () {
  'use strict'

  x = 1
  console.log(x) // ReferenceError: x is not defined
}())
```
### 20.5.2 변수, 함수, 매개변수의 삭제
delete 연산자로 변수, 함수, 매개변수를 삭제
```js
(function () {
  'use strict'

  var x = 1
  delete x
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}())
```
### 20.5.3 매개변수 이름의 중복
매개변수 이름을 중복
```js
(function () {
  'use strict'

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x
  }
  console.log(foo(1, 2))
}())
```
### 20.5.4 with 문의 사용
with문을 사용하면 스코프 체인에 추가하기 때문에 동일한 객체 프로퍼티를 반복해서 사용할 떄 객체이름을 생략할 수 있다. 하지만 성능과 가독성이 나빠지는 문제가 있어 사용하지 않는것이 좋다.
```js
(function () {
  'use strict'

  // SyntaxError: Strict mode code may not include a with statement
  with({ x: 1 }) {
    console.log(x)
  }
}())
```
## 20.6 strict mode 적용에 의한 변화
### 20.6.1 일반 함수의 this
일반함수르 호출하면 일반함수 내부에서는 this를 사용할 필요가 없기 떄문에 undefined를 할당한다. 
```js
(function () {
  'use strict'

  function foo() {
    console.log(this) // undefined
  }
  foo()

  function Foo() {
    console.log(this) // Foo
  }
  new Foo()
}())
```
### 20.6.2 arguments 객체
매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에는 반영되지 않는다.
```js
(function (a) {
  'use strict'
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments) // { 0: 1, length: 1 }
}(1))
```