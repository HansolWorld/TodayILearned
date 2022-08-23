# 30장. Date
날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다. UTC(협정 세꼐시), GMT(그리니치 평군시)가 있다. 초의 소수점 단위에서만 차이가 나기 떄문에 일상에서는 혼용되어 사용된다.</br>
KST(한국 표준시)는 UTC에 9시간을 더한 시간이다. KST는 UTC보다 9시간 빠르다.
## 30.1 Date 생성자 함수
Date는 생성자 함수다. Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다. UTC는 1970년 1월 1일 00:00:00을 기준으로 증가한다. Date객체는 UTC 기점으로 날짜와 시간까지의 밀리초를 나타낸다. Date 생성자 함수로 객체를 생성하는 방법은 4가지가 있다.
### 30.1.1 new Date()
Date 생성자 함수를 인수없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다. 내부적으론 날짜와 시간을 나타내는 정수값을 갖지만, Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
```js
new Date() // Mon Jul 06 2022 01:01:01 GMY+0900
```
new 없이 Date 생성자 함수를 사용하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.
```js
Date() // "Mon Jul 06 2022 01:01:01 GMY+0900"
```
### 30.1.2 new Date(milliseconds)
밀리초를 인수로 전달하면 UTC를 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.
### 30.1.3 new Date(dateString)
Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다. 문자열은 Date.parse메서드에 의해 해석이 가능해야 한다.
```js
new Date("Jan 01, 2020 00:00:00")
new Date("2022/01/01/00:00:00")
```
### 30.1.4 new Date(year, month[, day, hour, minute, second, millisecond])
연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다. 연과 월은 반드시 지정해야 한다.
|인수|내용|
|---|---|
|year|연을 나타내는 1900 이후의 정수, 0-99는 1900-1999로 처리됨|
|month|월을 나타내는 0-11 정수|
|day|일을 나타내는 1-31 정수|
|hour|시를 나타내는 0-23 정수|
|minute|분을 나타내는 0-59 정수|
|second|초를 나타내는 0-59 정수|
|millisecond|밀리초를 나타내는 0-999 정수|
## 30.2 Date 메서드
### 30.2.1 Date.now
UTC 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환
### 30.2.2 Date.parse
UTC 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
### 30.2.3 Date.UTC
UTC 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환. new Date(year, month[, day, hour, minute, second, millisecond])와 같은 형식으로 인수를 사용해야 한다.
### 30.2.4 Date.prototype.getFullYear
Date 객체의 연도를 나타내는 정수를 판환한다.
```js
new Date("2020/07/24").getFullYear() // 2020
```
### 30.2.5 Date.prototype.setFullYear
연도를 나타내는 정수를 설정한다.
```js
const today = new Date().setFullYear(2000)
today.getFullYear() // 2000

today.setFullYear(1900, 0, 1)
today.getFullYear() // 1900
```
### 30.2.6 Date.prototype.getMonth
객체의 월을 나타내는 0-11의 정수를 반환
### 30.2.7 Date.prototype.setMonth
월을 나타내는 0-11의 정수를 설정, 월 이외의 옵션으로 일도 설정 가능
### 30.2.8 Date.prototype.getDate
객체의 날짜를 나타내는 정수를 반환
### 30.2.9 Date.prototype.setDate
날짜를 나타내는 1-31를 설정
### 30.2.10 Date.prototype.getDay
요일을 나타내는 0-6 정수를 반환
### 30.2.11 Date.prototype.getHours
시간을 나타내는 0-23 정수를 반환
### 30.2.12 Date.prototype.setHours
0-23을 나타내는 정수를 사용해 시간을 설정, 이외 분, 초, 밀리초도 설정할 수 있음
### 30.2.13 Date.prototype.getMinutes
분을 나타내는 0-59 정수를 반환
### 30.2.14 Date.prototype.setMinutes
분을 나타내는 0-59 정수로 설정
### 30.2.15 Date.prototype.getSeconds
초를 나타내는 0-59 정수를 반환
### 30.2.16 Date.prototype.setSeconds
초를 나타내는 0-59 정수를 설정
### 30.2.17 Date.prototype.getMilliseconds
밀리초를 나타내는 0-999 정수를 반환
### 30.2.18 Date.prototype.setMilliseconds
밀리초를 나타내는 0-999 정수를 설정
### 30.2.19 Date.prototype.getTIme
UTC를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환
### 30.2.20 Date.prototype.setTime
UTC를 기점으로 경과된 밀리초를 설정
### 30.2.21 Date.prototype.getTimezoneOffset
UTC와 Date 객체에 저장된 locale t시간차이를 분단위로 반환
### 30.2.22 Date.prototype.toDateString
문자열로 Date 객체의 날자를 반환
### 30.2.23 Date.prototype.toTimeString
문자열로 Date 객체의 시간을 번환
### 30.2.24 Date.prototype.toISOString
ISO 8601 형식으로 Data 객체의 날짜와 시간을 표현한 문자열을 반환
- YYYY-MM-DD
- YYYYMMDD
- YYYY-Www-D
- YYYYWwwD
- YYYY-DDD
- YYYYDDD
### 30.2.25 Date.prototype.toLocaleString
locale을 기준으로 Date 객체의 날짜와 시간을 표현한 문자를 반환, 인수를 생략할 경우 브라우저가 동작 중인 시스템 locale이 적용
### 30.2.26 Date.prototype.toLocaleTimeString
인수로 전달된 locale을 기준으로 date 객체의 시간을 문자열로 반환
## 30.3 Date를 활용한 시계 예제
