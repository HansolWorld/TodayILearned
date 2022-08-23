# 31장. RexExp
정규 표현식은 일정한 패턴을 가진 문자열의 집합을 효현하기 위해 사용하는 형식언어이다. 자바스크립트 고유 문법이 아닌 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.</br>
정규표현식은 문자열을 대상으로 패턴매칭 기능을 제공한다. 패턴과 일치하는 문자열을 검색하거나 추출, 치환할 수 있는 기능을 말한다. 
## 31.2 정규 표현식의 생성
```js
const target = "Is this all there is?"

const regexp = /is/i

regexp.test(target)
```
정규표현식은 ***/페턴/플래스*** 형태로 생성한다. 또는 ***new RegExp*** 생성자 함수를 통해 생성할 수 있다.
```js
const target = "Is this all there is?"

const regexp = new RegExp(/is/i) // new RegExp("is", "i")

regexp.test(target)

```
## 31.3 RegXep 매서드
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
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); // -> true

// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

```js
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않는다.
const regExp = /is/i;

target.match(regExp);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
```

```js
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴.
// 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp = /is/ig;

target.match(regExp); // -> ["Is", "is", "is"]
```

```js
const target = 'Is this all there is?';

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

```js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
```

```js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp); // -> ["AA", "AA"]
```

```js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // -> ["AA", "AAA"]
```

```js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 한 번 이상 반복되는 문자열('A, 'AA', 'AAA', ...)을 전역 검색한다.
const regExp = /A+/g;

target.match(regExp); // -> ["A", "AA", "A", "AAA"]
```

```js
const target = 'color colour';

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // -> ["color", "colour"]
```

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;

target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
```

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

```js
const target = 'A AA BB ZZ Aa Bb';

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
const regExp = /[A-Z]+/g;

target.match(regExp); // -> ["A", "AA", "BB", "ZZ", "A", "B"]
```

```js
const target = 'AA BB Aa Bb 12';

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Za-z]+/g;

target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]
```

```js
const target = 'AA BB 12,345';

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9]+/g;

target.match(regExp); // -> ["12", "345"]
```

```js
const target = 'AA BB 12,345';

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9,]+/g;

target.match(regExp); // -> ["12,345"]
```

```js
const target = 'AA BB 12,345';

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // -> ["12,345"]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // -> ["AA BB ", ","]
```

```js
const target = 'Aa Bb 12,345 _$%&';

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", " $%&"]
```

```js
const target = 'AA BB 12 Aa Bb';

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;

target.match(regExp); // -> ["AA BB ", " Aa Bb"]
```

```js
const target = 'https://poiemaweb.com';

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // -> true
```

```js
const target = 'https://poiemaweb.com';

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;

regExp.test(target); // -> true
```

```js
const url = 'https://example.com';

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true
```

```js
/^(http|https):\/\//.test(url); // -> true
```

```js
const fileName = 'index.html';

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

```js
const target = '12345';

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

```js
const target = ' Hi!';

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

```js
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

```js
const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true
```

```js
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```

```js
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

```js
const target = 'abc#123';

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
(/[^A-Za-z0-9]/gi).test(target); // -> true
```

```js
(/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // -> true
```

```js
target.replace(/[^A-Za-z0-9]/gi, ''); // -> abc123
```