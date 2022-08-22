# 28장. number
## 28.1 Number 생성자 함수
Number객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다. 함수 내부의 인자를 숫자가 아닌 값을 전달하면 강제로 숫자로 변환후 내부 슬롯에 할당한다. 
## 28.2 Number 프로퍼티
### 28.2.1 Number.EPSILON 
1과 1보다 큰 가장 작은수의 차. 부동소수점으로 생기는 오차를 해결하기 위해 사용한다.
### 28.2.2 Number.MAX_VALUE
가장 큰 양수의 값
### 28.2.3 Number.MIN_VALUE
가장 작은 양수의 값
### 28.2.4 Number.MAX_SAFE_INTEGER
안전하게 표현할 수 있는 가장 큰 정수값
### 28.2.5 Number.MIN_SAFE_INTEGER
안전하게 표현할 수 있는 가장 작은 정수값
### 28.2.6 Number.POSITIVE_INFINITY
양의 무한대
### 28.2.7 Number.NEGATIVE_INFINITY
음의 무한대
### 28.2.8 Number.NaN
숫자가 아님을 나타내는 숫자값
## 28.3 Number 메서드
### 28.3.1 Number.isFinite
인수로 전달된 숫자값이 Infinity, -Infinity인지 검사하여 불리언 값을 반환한다.
### 28.3.2 Number.isInteger
인수로 절단될 숫자값이 정수인지 검사하여 불리언 값으로 반환한다.
```js
Number.isInteger(0) // true
Number.isInteger(123) // true
Number.isInteger(-123) // true

Number.isInteger(0.5) // true
Number.isInteger('123') // true
Number.isInteger(false) // true

Number.isInteger(Infinity) // true
Number.isInteger(-Infinity) // true
```
### 28.3.3 Number.isNaN
인수로 전달된 숫자값이 NaN인지 검사하여 결과를 불리언으로 반환한다.
### 28.3.4 Number.isSafeInteger
-(2^53-1)~2^53-1 사이의 정수값인지 검사하여 불리언 값으로 반환한다. 검사전 인수를 숫자로 암묵적 타입 변환하지 않는다.
### 28.3.5 Number.prototype.toExponential
숫자를 지수표현식 문자열로 반환한다. 
### 28.3.6 Number.prototype.toFixed
반올림하여 문자열로 반환한다. 인수로 정수값을 전달하여 반올림할 자릿수를 지정한다,
### 28.3.7 Number.prototype.toPrecision
인수로 전달받은 전체 자릿수까지 유효 하도록 반올림하여 문자열로 반환한다. 자리수로 표현할 수 없는 경우 지수표현식으로 결과를 반환한다.
### 28.3.8 Number.prototype.toString
문자열로 반환한다. 인수로 2~36사이의 정수를 넣으면 진법변환을 한다.