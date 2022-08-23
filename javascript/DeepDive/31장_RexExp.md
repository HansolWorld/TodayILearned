# 31장. RexExp
정규 표현식은 일정한 패턴을 가진 문자열의 집합을 효현하기 위해 사용하는 형식언어이다. 자바스크립트 고유 문법이 아닌 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.</br>
정규표현식은 문자열을 대상으로 패턴매칭 기능을 제공한다. 패턴과 일치하는 문자열을 검색하거나 추출, 치환할 수 있는 기능을 말한다. 
## 31.2 정규 표현식의 생성
```js
const target = "Is this all there is?"

const regexp = /is/i

regexp.test(target)
```
정규표현식은 ***/패턴/플래그*** 형태로 생성한다. 또는 ***new RegExp*** 생성자 함수를 통해 생성할 수 있다.
```js
const target = "Is this all there is?"

const regexp = new RegExp(/is/i) // new RegExp("is", "i")

regexp.test(target)

```
## 31.3 RegExp 매서드
### 31.3.1 RegExp.protorype.exec
정규 표현식의 패턴을 검색해 매칭 결과를 배열로 변환, 매칭이 없는 경우 null 반환
```js
const target = "Is this all there is?"
const regExp = /is/

regExp.exec(target) // ["is" ~~]
```
### 31.3.2 RegExp.protorype.test
패턴을 검색해 매킹 결과를 불리언 값으로 반환
```js
const target = "Is this all there is?"
const regExp = /is/

regExp.test(target) // true
```
### 31.3.3 RegExp.protorype.match
정규표현식의 패턴과 매칭되는 결과를 배열로 반환. exec와 다른점은 g플래스를 사용하면 매칭되는 모든 결과를 반환한다.
```js
const target = "Is this all there is?"
const regExp = /is/g

regExp.match(target) // ["is", "is"]
```
## 31.4 플래그
총 6개의 플래그가 존재한다.
|플래그|의미|설명|
|---|---|---|
|i|ignore case|대소문자를 구별하지 않음|
|g|global|패턴과 일치하는 모든 문자열 전역 검색|
|m|multi line||행이 바뀌더라도 검색을 계속|
|u|unicode|유니코드를 사용해 문자열 검섹|
|y|sticky|인덱스를 인자로 받아 패턴이 시작하는 위치를 지정|
|s|dotAll|.을 사용할때 개행 문자인 \n도 포함하여 검색|
## 31.5 패턴
### 31.5.1 문자열 검섹
```js
const target = 'Is this all there is?'

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target) // -> true

// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp)
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```
대소문자를 구분하지 않으려면 i플래그를 사용한다.
```js
const target = 'Is this all there is?'

// 'is' 문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않는다.
const regExp = /is/i

target.match(regExp)
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
```
모든 문자열 전역을 검색 하려면 g 플래그를 사용한다.
```js
const target = 'Is this all there is?'

// 'is' 문자열과 매치하는 패턴.
// 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp = /is/ig

target.match(regExp) // -> ["Is", "is", "is"]
```
### 31.5.2 임의의 문자열 검색
.은 임의의 문자 한개를 의미한다.
```js
const target = 'Is this all there is?'

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g

target.match(regExp) // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```
### 31.5.3 반복 검색
{m, n} 최소 m번 최대 n번 박복되는 문자열을 의미한다.
```js
const target = 'A AA B BB Aa Bb AAA'

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g

target.match(regExp) // -> ["A", "AA", "A", "AA", "A"]
```
{n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉 {n}은 {n,n}과 같다
```js
const target = 'A AA B BB Aa Bb AAA'

// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g

target.match(regExp) // -> ["AA", "AA"]
```
{n,}은 최소 n번 이상 나오는 것을 의미한다.
```js
const target = 'A AA B BB Aa Bb AAA'

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g

target.match(regExp) // -> ["AA", "AAA"]
```
+는 앞선 패턴이 최소 한번이상 반복되는 문자열을 의미한다. +는 {1,}과 같다.
```js
const target = 'A AA B BB Aa Bb AAA'

// 'A'가 최소 한 번 이상 반복되는 문자열('A, 'AA', 'AAA', ...)을 전역 검색한다.
const regExp = /A+/g

target.match(regExp) // -> ["A", "AA", "A", "AAA"]
```
?는 앞선 패턴이 최대 0번 이상 반복되는 문자열을 의미한다. ?는 {0,1}과 같다. 
```js
const target = 'color colour'

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g

target.match(regExp) // -> ["color", "colour"]
```
### 31.5.4 or 검색
|은 or의 의미를 갖는다. /A|B/는 A또는 B를 의미한다.
```js
const target = 'A AA B BB Aa Bb'

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g

target.match(regExp) // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
```
[]내의 문자는 orfh ehdwkrgksek. rmenldp |를 사용하면 앞선 패턴을 한번이상 반복한다.
```js
const target = 'A AA B BB Aa Bb'

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g

target.match(regExp) // -> ["A", "AA", "B", "BB", "A", "B"]
```
범위를 지정하려면 -를 사용한다. 범위는 유니코드 기준으로 한다.
```js
const target = 'A AA BB ZZ Aa Bb'

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
const regExp = /[A-Z]+/g

target.match(regExp) // -> ["A", "AA", "BB", "ZZ", "A", "B"]
```
### 31.5.5 NOT 검색
[...]내의 ^는 not을 의미한다. 
```js
const target = 'AA BB 12 Aa Bb'

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g

target.match(regExp) // -> ["AA BB ", " Aa Bb"]
```
### 31.5.6 시작 위치로 검색
[...] 밖의 ^는 문자열의 시작을 의미한다. 단 [...]내의 ^는 not의 의미를 가지므로 주의해야한다.
```js
const target = 'https://poiemaweb.com'

// 'https'로 시작하는지 검사한다.
const regExp = /^https/

regExp.test(target) // -> true
```
### 31.5.7 마지막 위치로 검색
\$는 문자열의 마지막을 의미한다.
```js
const target = 'https://poiemaweb.com'

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/

regExp.test(target) // -> true
```
## 31.6 자주 사용하는 정규표현식
### 31.6.1 특정 단어로 시작하는지 검사
```js
const url = 'https://example.com'

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url) // -> true
```
^은 시작을 의미하고 ?은 최대 한번 반복되는지를 의미한다. 
### 31.6.2 특정 단어로 끝나는지 검사
```js
const fileName = 'index.html'

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName) // -> true
```
### 31.6.3 숫자로만 이루어진 문자열인지 검사
```js
const target = '12345'

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target) // -> true
```
### 31.6.4 하나 이상의 공백으로 시작하는지 검사
```js
const target = ' Hi!'

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target) // -> true
```
### 31.6.5 아이디로 사용 가능한지 검사
```js
const id = 'abc123'

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id) // -> true
```
### 31.6.6 메일 주소 형식에 맞는지 검사
```js
const email = 'ungmo2@gmail.com'

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email) // -> true
```
### 31.6.7 핸드폰 번호 형식에 맞는지 검사
```js
const cellphone = '010-1234-5678'

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone) // -> true
```
### 31.6.8 특수 문자 포함 여부 검사
```js
const target = 'abc#123'

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
(/[^A-Za-z0-9]/gi).test(target) // -> true
```

```js
(/[\{\}\[\]\/?.,:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target) // -> true
```

```js
target.replace(/[^A-Za-z0-9]/gi, '') // -> abc123
```
