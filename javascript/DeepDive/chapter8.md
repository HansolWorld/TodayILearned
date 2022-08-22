# 8장. 제어문
제어문은 조건에 따라 코드블록을 실행하거나 반복실행 할 떄 사용한다. 
## 8.1. 블록문
0개 이상의 문을 중괄호로 묶는것으로 코드블록, 블록이라고 부른다. 자바스크립트는 블록문을 하나의 실행 단위로 취급한다. 블록문은 단독으로 사용할 수 있으나 일반적으로 제어문이나 함수를 정의할때 사용한다.
## 8.2. 조건문
조건문은 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다. 조건식은 불리언 값으로 평가될 수 있는 표현식이다.
### 8.2.1 if ... else 문
조건식의 평가 결과, 논리적 참 또는 거짓에 따라 실행할 코드 블록을 결정한다. 조건식의 평가 괄과가 true인 경우 if문의 코드 블록이 실행되고 false의 경우 else의 코드 블록이 실행된다. 코드 블록을 늘리고 싶으면 else if문을 사용한다.
```js
if (조건식1) {
    // 조건식1이 참이면 이 코드 블록 실행
} else if (조건식2) {
    // 조건식2이 참이면 이 코드 블록 실행
} else {
    // 조건식1과 조건식2가 모두 거짓이면 이 코드 블록 실행
}
```
else if와 else는 옵션이다. 사용할수도 있고 사용하지 않을 수도 있다. if문과 else는 2번이상 사용할 수 없지만 else if는 여러번 사용할 수 있다.
### 8.2.2 switch문
switch문은 주어진 표현식을 평가하여 그값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다. 표현식과 일치하는 case문이 없다면 실행 순서는 default 문으로 이동한다.
```js
switch (표현식) {
    case 표현식1:
        switch 문의 표현식과 표현식1이 일치하면 실향될 문
        break
    case 표현식2:
        switch 문의 표현식과 표현식2가 일치하면 실행될 문
        break
    default:
        switch 문의 표현식과 일치하는 case문이 없을 떄 실행될 문
}
```
논리적 참,거짓보다 다양한 상황에 따라 실행할 코드 블록을 결정할 떄 사용한다. 
```js
var month = 11
var monthName

switch(month) {
    case 1: monthName = "jan"
    case 2: monthName = "feb"
    case 3: monthName = "mar"
    case 4: monthName = "apr"
    case 5: monthName = "may"
    case 6: monthName = "jun"
    case 7: monthName = "jul"
    case 8: monthName = "aug"
    case 9: monthName = "sep"
    case 10: monthName = "oct"
    case 11: monthName = "nov"
    case 12: monthName = "dec"
    default: monthName = "Invalid month"
}
```
위 코드를 실행하면 nov가 아닌 invalid month가 나온다. swich문이 11에서 nov를 할당한뒤 멈추지 않고 dec가 할당되고 invalid month가 할당된 것이다. 이를 폴스루라고 한다.</br>
break문을 사용해 블록을 탈출해야 한다. break문이 없다면 case문의 표현식과 일치하지 않더라도 실행 흐름이 다음 case문으로 연이어 이동한다.
```js
var month = 11
var monthName

switch(month) {
    case 1: monthName = "jan"
        break
    case 2: monthName = "feb"
        break
    case 3: monthName = "mar"
        break
    case 4: monthName = "apr"
        break
    case 5: monthName = "may"
        break
    case 6: monthName = "jun"
        break
    case 7: monthName = "jul"
        break
    case 8: monthName = "aug"
        break
    case 9: monthName = "sep"
        break
    case 10: monthName = "oct"
        break
    case 11: monthName = "nov"
        break
    case 12: monthName = "dec"
        break
    default: monthName = "Invalid month"
}
```
## 8.3. 반복문
조건식의 평가 결과가 참인 경우 코드 블록을 실핸한다. 그 조건식을 다시 평가하여 여전히 참인경우 코드 블록을 다시 실행한다. 자바스크립트는 for, while, do...while이 있다.
### 8.3.1 for문
for문은 조건식이 거짓으로 평가도리 떄까지 코드 블록을 반복 실행한다.
```js
for (변수 선언문 또는 항당문; 조건식; 증감식) {
    조건식이 참인 경우 반복 실행될 문
}
```
### 8.3.2 while문
주어진 조건식의 평가 결과가 참이면 코드 블록을 계속 반복 실행한다. 
```js
while(조건식) {
    조건식이 참인 경우 반복 실행될 문
}
```
while문은 if문으로 탈출 조건을 만들고 break문으로 코드 블록을 탈출한다.
### 8.3.3 do...while문
코드 블록을 먼저 실행하고 조건식을 평가한다. 따라서 코드 블록은 무조건 한번 이상 실행된다.
```js
do {
    조건식이 참인경우 반복 실행될 문
} while (조건식)
```
## 8.4. break문
코드 블록을 탈출한다. 
## 8.5. continue문
반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동한다