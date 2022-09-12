# 39장. DOM
**DOM은 HTML문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 프로퍼티와 메서드를 API로 제공하는 트리 자료구조다.**
## 39.1 노드
### 39.1.1 HTML 요소와 노드 객체
HTML 요소(element)는 HTML 문서를 구성하는 개별적인 요소를 의미한다. 렌터링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환되고 요소의 어트리뷰트는 러트리뷰트 노드로 텍스트 콘텐츠는 텍스느 노드로 변환된다.   
HTML 요소는 중첩관계를 갖는다. HTML 요소의 콘텐츠 영역에는 텍스트 뿐 아니라 HTML 요소도 포함할 수 있다. HTML 요소간의 부자 관계를 분영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성한다.
- 트리자료구조   
	트리자료구조는 노드들의 계층 구조로 이뤄진다. 트리자료 구조는 부모노드와 자식노드로 구성된 계층적 구조를 표현하는 비선형 자료 구조를 말한다. 트리 자료구조는 최상위 노드에서 시작해 최상위 노드는 부모 노도가 없고, 루트노드라 부른다. 자식 노드가 없느 노드는 리프 노드라 한다.   
	**노드 객체들로 구성된 트리 자료구조를 DOM이라 한다. 노드 객체의 트리로 구화 되어 있기 때문에 DOM을 DOM트리라고 부르기도 한다.**
### 39.1.2 노드 객체의 타입
HTML 문서를 렌더링 엔진이 파싱해 DOM을 생성한다. 노드 객체는 총 12개의 종류가 있다. 이중 중요한 노드 타입은 4가지다.
- 문서 노드   
	문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로 documnet 객체를 가리킨다. 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로 widow documnet 프로퍼티에 바인딩되 window.document 또는 document로 참조할 수 있다.   
	브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되도 하나의 전역 객체 window를 공유한다. 모든 자바스크립트 코드는 document 프로퍼티에 바인진 되므로 HTML 문서당 document 객체는 유일하다.   
	DOM 트리는 노드들에 접근하기 위한 진입점으로 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.
- 요소 노드   
	요소 노드는 HTML 요소를 가리키는 객체로 HTML 요소간의 중첩에 의해 부자관꼐를 가지며 정보를 구조화 한다. 
- 어트리뷰트 노드   
	어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체로 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 요소 노드는 부모 노드와 연결되지만 어트리뷰트 노드는 부모 노드와 연결되지 않고 요소 노드에만 연결된다. 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 요소노드에 접근해야 한다.
- 텍스트 노드   
	텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다. 텍스트 노드는 문서의 정보를 표현한다. 텍스트 노드는 요소 노드의 자식 노드며 자식을 가질 수 없는 리프 노드다. 즉 텍스트 노드는 DOM 트리의 최종단이다. 

위 4가지 노드 타입 외에도 Comment, DOCTYPE, DocumentFragment 노드 등 총 12개가 있다.
### 39.1.3 노드 객체의 상속 구조
DOM API를 통해 어트리뷰트와 텍스트를 조작할 수 있다. DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저가 추가적으로 제공하는 호스트 객체다. 하지만 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.
![노드 객체의 상속 구조](img/%EB%85%B8%EB%93%9C%20%EA%B0%9D%EC%B2%B4%EC%9D%98%20%EC%83%81%EC%86%8D%20%EA%B5%AC%EC%A1%B0.png)
이 구조를 프로토타입 체인 관점에서 보면 input 요소를 파싱해 객체화한 input 요소 노드의 객체를 따라 prototype에 바인딩되어 있는 프로토타입 객체를 상속 받는다. input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.   
input 요소 노드 객체는 다양한 특성을 갖는 객체이다.
|input요소 노드 객체의 특성|프로토타입을 제공하는 객체|
|---|---|
|객체|Object|
|이벤트를 발생시키는 객체|EventTarget|
|트리 자료구조의 노드 객체|Node|
|브라우저가 렌더링할 수 있는 웹문서의 요소를 표현하는 객체|Element|
|웹 문서의 요소중에서 HTML 요소를 표현하는 객체|HTMLElement|
|HTML 요소중에서 input 요소를 표현하는 객체|HTMLInputElement|

노드 객체는 노드 객체의 종류, 타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능과 각 노트 타입에 따라 고유한 기능을 가지고 있다. 모든 노드 객체는 공통적으로 이벤트를 발생 시킬 수 있는 EventTarget 인터페이스를 제공한다. 또한 트리탐색 기능, 노드 정보제공 기능을 제공하는 Node 인터페이스가 있다.   
HTML요소는 공톡적으로 Style 프로퍼티가 있다. HTML 요소가 갖는 공통적인 기능은 HTMLElement 인터페이스가 제공한다.   
노드 객체는 HTML 요소 종류에 따라 고유한 기능도 있고 필요에 따라 제공하는 인터페이스(HTMLInputElement, HTMLDivElement)가 HTML 요소의 종류에 따라 각각 다르다.   
**DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.
## 39.2 요소 노드 취득
### 39.2.1 id를 이용한 요소 노드 취득
Document.protytpe.getElementById 메서드는 인수로 id 어트리뷰트값을 갖는 하나의 요소 노드를 탐색하여 반환한다. getElementById메서드는 document.protytpe의 프로퍼티로 반드시 문서 노드인 document를 통해 호출해야 한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 'banana'인 요소 노드를 탐색하여 반환한다.
      // 두 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById('banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```
id는 HTML 문서 내에 유일한 값이여야하며 class 어트리뷰트와 달리 공백 문자로 구분해 여러 값을 가질 수 없다. 하지만 HTML문서 내에 중복된 id 값을 갖는 HTML 요소가 있다해도 에러를 발생하지 않는다. 이럴경우 getElementById 메서드는 인수로 전달된 id 값을 갖는 첫번쨰 요소 노드만 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="banana">Apple</li>
      <li id="banana">Banana</li>
      <li id="banana">Orange</li>
    </ul>
    <script>
      // getElementById 메서드는 언제나 단 하나의 요소 노드를 반환한다.
      // 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById('banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```
만약 id 값을 갖는 HTML 요소가 없다면 null을 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 'grape'인 요소 노드를 탐색하여 반환한다. null이 반환된다.
      const $elem = document.getElementById('grape');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
      // -> TypeError: Cannot read property 'style' of null
    </script>
  </body>
</html>
```
HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암목적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      // id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다.
      console.log(foo === document.getElementById('foo')); // true

      // 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않는다.
      delete foo;
      console.log(foo); // <div id="foo"></div>
    </script>
  </body>
</html>
```
단 id 값과 동일한 이름의 전역 번수가 이미 선언되어 있다면 이 변수에 노드 객체가 재할당 되지 않는다.
```js
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      let foo = 1;

      // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
      console.log(foo); // 1
    </script>
  </body>
</html>
```
### 39.2.2 태그 이름을 이용한 요소 노드 취득
Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 참색한다. getElementsByTagName메서드는 여러개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다. HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      // 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elems = document.getElementsByTagName('li');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
      [...$elems].forEach(elem => { elem.style.color = 'red'; });
    </script>
  </body>
</html>
```
HTML 문서의 모든 요소를 취득하려면 인수로 "*"를 전달한다.
```js
// 모든 요소 노드를 탐색하여 반환한다.
const $all = document.getElementsByTagName('*');
// -> HTMLCollection(8) [html, head, body, ul, li#apple, li#banana, li#orange, script, apple: li#apple, banana: li#banana, orange: li#orange]
```
 document.getElementsByTagName 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다. Document.prototype에 정의된 메서드는 DOM 전체에서 요소 노드를 탐색하지만  Element.prototype에 정의된 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
 ```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      const $lisFromDocument = document.getElementsByTagName('li');
      console.log($lisFromDocument); // HTMLCollection(4) [li, li, li, li]

      // #fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두
      // 탐색하여 반환한다.
      const $fruits = document.getElementById('fruits');
      const $lisFromFruits = $fruits.getElementsByTagName('li');
      console.log($lisFromFruits); // HTMLCollection(3) [li, li, li]
    </script>
  </body>
</html>
 ```
### 39.2.3 class를 이용한 요소 노드 취득
Document.prototype/Element.prototype.getElementByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 요소를 반환한다. class 값은 공백으로 구분해 여러 class를 지정할 수 있다. 
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="fruit apple">Apple</li>
      <li class="fruit banana">Banana</li>
      <li class="fruit orange">Orange</li>
    </ul>
    <script>
      // class 값이 'fruit'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName('fruit');

      // 취득한 모든 요소의 CSS color 프로퍼티 값을 변경한다.
      [...$elems].forEach(elem => { elem.style.color = 'red'; });

      // class 값이 'fruit apple'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $apples = document.getElementsByClassName('fruit apple');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      [...$apples].forEach(elem => { elem.style.color = 'blue'; });
    </script>
  </body>
</html>
```
document.getElementsByTagName 메서드와 마찬가지로 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다. Document.prototype에 정의된 메서드는 DOM 전체에서 요소 노드를 탐색하지만  Element.prototype에 정의된 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <div class="banana">Banana</div>
    <script>
      // DOM 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $bananasFromDocument = document.getElementsByClassName('banana');
      console.log($bananasFromDocument); // HTMLCollection(2) [li.banana, div.banana]

      // #fruits 요소의 자손 노드 중에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환한다.
      const $fruits = document.getElementById('fruits');
      const $bananasFromFruits = $fruits.getElementsByClassName('banana');

      console.log($bananasFromFruits); // HTMLCollection [li.banana]
    </script>
  </body>
</html>
```
### 39.2.4 CSS 선택자를 이용한 요소 노드 취득
CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.
```css
/* 전체 선택자: 모든 요소를 선택 */
* { ... }
/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p { ... }
/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo { ... }
/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo { ... }
/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type=text] { ... }
/* 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p { ... }
/* 자식 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p { ... }
/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul { ... }
/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }
/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover { ... }
/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택
   일반적으로 content 프로퍼티와 함께 사용된다. */
p::before { ... }
```
Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 노드를 탐색한다.
- CSS 선택자를 만족시키는 요소 노드가 여러개인 경우 첫번째 요소 노드만 반환
- CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null 반환
- CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환한다.
      const $elem = document.querySelector('.banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```
Document.prototype/Element.prototype.querySelectorAll 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색 반환한다. 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. NodeList 객체는 유사 배열 객체이면서 이터러블 이다.
- CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체 반환
- CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환한다.
      const $elems = document.querySelectorAll('ul > li');
      // 취득한 요소 노드들은 NodeList 객체에 담겨 반환된다.
      console.log($elems); // NodeList(3) [li.apple, li.banana, li.orange]

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // NodeList는 forEach 메서드를 제공한다.
      $elems.forEach(elem => { elem.style.color = 'red'; });
    </script>
  </body>
</html>
```
모든 요소를 취득하려면 인수로 "*"를 전달
```js
// 모든 요소 노드를 탐색하여 반환한다.
const $all = document.querySelectorAll('*');
// -> NodeList(8) [html, head, body, ul, li#apple, li#banana, li#orange, script]
```
getElementsByTagName, getElementsByClassName 메서드와 마찬가지로 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다. Document.prototype에 정의된 메서드는 DOM 전체에서 요소 노드를 탐색하지만  Element.prototype에 정의된 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.
querySeletor, querySeletorAll 메서든느 gerElementById, getElementsBy*** 메서드보다 다소 느리지만 CSS 선택자 문법을 사용하여 좀더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 사용할 수 있다는 장점이 있다. ID가 있다면 getElementByID 메서드를 사용하고 그외에는 querySelector를 사용하는 것을 권장한다.
### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인
Element.protytpe.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.
```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    const $apple = document.querySelector('.apple');

    // $apple 노드는 '#fruits > li.apple'로 취득할 수 있다.
    console.log($apple.matches('#fruits > li.apple'));  // true

    // $apple 노드는 '#fruits > li.banana'로 취득할 수 없다.
    console.log($apple.matches('#fruits > li.banana')); // false
  </script>
</html>
```
### 39.2.6 HTMLCollection과 NodeList
DOM 컬렉션 객체인 HTMLCollection과 NodeList는 DOM API가 여러개의 결과값을 반환하기 위한 DOM 컬렉션 객체다. 두개 모두 유사 배열 객체이면서 이터러블이다. 따라서 for ... of 문으로 순회할 수 있으며 스프레드 문법 사용이 가능하다.   
HTMLCollection과 NodeList의 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 **살아있는 객체**라는 것이다. HTMLCollection는 언제나 live 객체로 동작하고 NodeList는 대부분 실시간으로 반영하지 않고 경우에 따라 live객체로 동작한다.   
#### HTMLCollection
getElementsByTagName, getElementsByVlassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영한다. 
```js
<!DOCTYPE html>
<head>
  <style>
    .red { color: red; }
    .blue { color: blue; }
  </style>
</head>
<html>
  <body>
    <ul id="fruits">
      <li class="red">Apple</li>
      <li class="red">Banana</li>
      <li class="red">Orange</li>
    </ul>
    <script>
      // class 값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName('red');
      // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
      console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

      // HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
      for (let i = 0; i < $elems.length; i++) {
        $elems[i].className = 'blue';
      }

      // HTMLCollection 객체의 요소가 3개에서 1개로 변경되었다.
      console.log($elems); // HTMLCollection(1) [li.red]
    </script>
  </body>
</html>
```
위코드에서 li요소의 class값이 blue로 변경되어야 하지만 2번재 요소는 변경되지 않는다. $elems.length는 3이므로 for문의 코드블록은 3번 실행된다.
1. 첫번쨰 반복   
	첫번쨰 li 요소의 class 값이 red에서 blue로 변경된다. 이떄 class name 값이 red에서 blue로 변경되어 getElementsbyClassName 메서드의 인자로 전달한 red와 일치하지 않기 때문에 $elems에서 실시간으로 제거된다. 
2. 두번째 반복   
	첫번째 반복에서 $elems의 값이 제거 됫다. $elems[1]은 세번째 li의 요소가 된다. li 요소의 class값이 blue로 변경되고 마찬가지로 $elems에서 실시간으로 제외된다.
3. 세번째 반복   
	$elems에는 두번쨰 li 요소만 남아 있다. 이때 $elems.length는 1이 되므로 for문 조건식을 만족하지 않아 반복이 종료된다.

이처럼 HTMLCollection 객체는 실시간으로 노드 객체의 상태를 변경을 반영하여 요소를 제거하기 떄문에 주의해야 한다. 이는 for문을 역순회하는 방법으로 회피한 수 있다.
```js
// for 문을 역방향으로 순회
for (let i = $elems.length - 1; i >= 0; i--) {
  $elems[i].className = 'blue';
}
```
while을 사용해 노트 객체가 남아 있지 않을 때까지 무한 반복하는 방법으로 회피할 수도 있다.
```js
// while 문으로 HTMLCollection에 요소가 남아 있지 않을 때까지 무한 반복
let i = 0;
while ($elems.length > i) {
  $elems[i].className = 'blue';
}
```
이러한 문제를 방지하기 위해 HTMLCollection을 사용하지 않고 이터러블인 점을 사용해 고차함수를 사용할 수 있다.
```js
// 유사 배열 객체이면서 이터러블인 HTMLCollection을 배열로 변환하여 순회
[...$elems].forEach(elem => elem.className = 'blue');
```
#### NodeList
HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메서드를 사용하는 방법도 있다.
```js
// querySelectorAll은 DOM 컬렉션 객체인 NodeList를 반환한다.
const $elems = document.querySelectorAll('.red');

// NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
$elems.forEach(elem => elem.className = 'blue');
```
NodeList는 **childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태를 변경을 반영하는 live 객체로 동작하므로 주의해야 한다.**
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
    const { childNodes } = $fruits;
    console.log(childNodes instanceof NodeList); // true

    // $fruits 요소의 자식 노드는 공백 텍스트 노드(39.3.1절 "공백 텍스트 노드" 참고)를 포함해 모두 5개다.
    console.log(childNodes); // NodeList(5) [text, li, text, li, text]

    for (let i = 0; i < childNodes.length; i++) {
      // removeChild 메서드는 $fruits 요소의 자식 노드를 DOM에서 삭제한다.
      // (39.6.9절 "노드 삭제" 참고)
      // removeChild 메서드가 호출될 때마다 NodeList 객체인 childNodes가 실시간으로 변경된다.
      // 따라서 첫 번째, 세 번째 다섯 번째 요소만 삭제된다.
      $fruits.removeChild(childNodes[i]);
    }

    // 예상과 다르게 $fruits 요소의 모든 자식 노드가 삭제되지 않는다.
    console.log(childNodes); // NodeList(2) [li, li]
  </script>
</html>
```
**노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다.**
## 39.3 노드 탐색
### 39.3.1 공백 텍스트 노드
HTML 요소 사이의 스페이스, 탭, 줄바꿈 둥의 공백 문자는 텍스트 노드를 생성한다.
```HTML
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
</html>
```
텍스트 에디터에서 HTML 문서에 스페이스키, 탭키, 엔터키등을 입력하면 공백 문자가 추가된다. 위 HTML 문서에도 공백 문자가 포함되어 있다.   
노드를 탐색할때 공백 노드를 주의해야한다. 인위적인 HTML 문서의 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않는다. 하지만 가독성이 좋지 않으므로 권장하지 않는다.
```js
<ul id="fruits"><li
  class="apple">Apple</li><li
  class="banana">Banana</li><li
  class="orange">Orange</li></ul>
```
### 39.3.2 자식 노드 탐색
자식 노드를 탐색하기 위해서 노드 탐색 프로퍼티를 사용한다.
|프로퍼티|설명|
|---|---|
|Node.prototype.childNodes|자식 노드를 모두 탐색해 NodeList에 담아 반환한다. ChildNodes 프로퍼티가 반환한 NodeList에는 요소노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다.|
|Element.prototype.children|자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환한다. Children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않는다.|
|Node.prototype.firstChild|첫번쨰 자식 노드를 반환한다. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.|
|Node.prototype.lastChid|마지막 자식 노드를 반환한다. lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.|
|Element.prototype.firstElementChild|첫번쨰 자식 노드를 반환한다. firstElementChild 프로퍼티는 요소 노드만 반환한다.|
|Element.prototype.lastElementChild|마지막 자식 요소 노드를 반환한다. lastElementChild 프로퍼티는 요소 노드만 반환한다.|
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
    const $fruits = document.getElementById('fruits');

    // #fruits 요소의 모든 자식 노드를 탐색한다.
    // childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있다.
    console.log($fruits.childNodes);
    // NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

    // #fruits 요소의 모든 자식 노드를 탐색한다.
    // children 프로퍼티가 반환한 HTMLCollection에는 요소 노드만 포함되어 있다.
    console.log($fruits.children);
    // HTMLCollection(3) [li.apple, li.banana, li.orange]

    // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
    // firstChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
    console.log($fruits.firstChild); // #text

    // #fruits 요소의 마지막 자식 노드를 탐색한다.
    // lastChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
    console.log($fruits.lastChild); // #text

    // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
    // firstElementChild 프로퍼티는 요소 노드만 반환한다.
    console.log($fruits.firstElementChild); // li.apple

    // #fruits 요소의 마지막 자식 노드를 탐색한다.
    // lastElementChild 프로퍼티는 요소 노드만 반환한다.
    console.log($fruits.lastElementChild); // li.orange
  </script>
</html>
```
### 39.3.3 자식 노드 존재 확인
Node.prototype.hasChildNodes 메서드를 사용해 자식 노드가 존재하는지 불리언 값으로 반환한다. 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
    const $fruits = document.getElementById('fruits');

    // #fruits 요소에 자식 노드가 존재하는지 확인한다.
    // hasChildNodes 메서드는 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.
    console.log($fruits.hasChildNodes()); // true
  </script>
</html>
```
자식노드중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 children.length 또는 Element 인터페이스 childElementCount 프로퍼티를 사용한다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
    const $fruits = document.getElementById('fruits');

    // hasChildNodes 메서드는 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.
    console.log($fruits.hasChildNodes()); // true

    // 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지는 확인한다.
    console.log(!!$fruits.children.length); // 0 -> false
    // 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지는 확인한다.
    console.log(!!$fruits.childElementCount); // 0 -> false
  </script>
</html>
```
### 39.3.4 요소 노드의 텍스트 노드 탐색
텍스트 노드는 요소 노드의 자식 노드로 firstChild 프로퍼티로 접근할 수 있다.
```html
<!DOCTYPE html>
<html>
<body>
  <div id="foo">Hello</div>
  <script>
    // 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
    console.log(document.getElementById('foo').firstChild); // #text
  </script>
</body>
</html>
```
### 39.3.5 부모 노드 탐색
Node.protorype.parentNode 프로퍼티를 사용해 부모 노드를 탐색할 수 있다. 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드로 부모 노드가 텍스트 노드인 경우는 없다.
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다.
    const $banana = document.querySelector('.banana');

    // .banana 요소 노드의 부모 노드를 탐색한다.
    console.log($banana.parentNode); // ul#fruits
  </script>
</html>
```
### 39.3.6 형제 노드 탐색
부모 노드가 같은 형제 노드를 탐색하려면 노드 탐색 프로퍼티를 사용한다. 어트리뷰트 노드는 요소노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 떄문에 반환되지 않고 텍스트 노드 또는 요소 노드만 반환된다.
|프로퍼티|설명|
|---|---|
|Node.prototype.previousSibling|부모 노드가 같은 형제 노드 중 자신 이전의 형제 노드를 탐색해 반환한다. 요소 노드 또는 텍스트 노드를 반환한다.|
|Node.prototype.nextSibling||부모 노드가 같은 형제 노드 중 자신 다음의 형제 노드를 탐색해 반환한다. 요소 노드 또는 텍스트 노드를 반환한다.|
|Element.prototype.previousElementSibling|부모 노드가 같은 형제 노드 중 자신 이전의 형제 노드를 탐색해 반환한다. 요소 노드를 반환한다.|
|Element.prototype.nextElementSibling|부모 노드가 같은 형제 노드 중 자신 다음의 형제 노드를 탐색해 반환한다. 요소 노드를 반환한다.|
```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
    const $fruits = document.getElementById('fruits');

    // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
    // firstChild 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { firstChild } = $fruits;
    console.log(firstChild); // #text

    // #fruits 요소의 첫 번째 자식 노드(텍스트 노드)의 다음 형제 노드를 탐색한다.
    // nextSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { nextSibling } = firstChild;
    console.log(nextSibling); // li.apple

    // li.apple 요소의 이전 형제 노드를 탐색한다.
    // previousSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { previousSibling } = nextSibling;
    console.log(previousSibling); // #text

    // #fruits 요소의 첫 번째 자식 요소 노드를 탐색한다.
    // firstElementChild 프로퍼티는 요소 노드만 반환한다.
    const { firstElementChild } = $fruits;
    console.log(firstElementChild); // li.apple

    // #fruits 요소의 첫 번째 자식 요소 노드(li.apple)의 다음 형제 노드를 탐색한다.
    // nextElementSibling 프로퍼티는 요소 노드만 반환한다.
    const { nextElementSibling } = firstElementChild;
    console.log(nextElementSibling); // li.banana

    // li.banana 요소의 이전 형제 요소 노드를 탐색한다.
    // previousElementSibling 프로퍼티는 요소 노드만 반환한다.
    const { previousElementSibling } = nextElementSibling;
    console.log(previousElementSibling); // li.apple
  </script>
</html>
```
## 39.4 노드 정보 취득
## 39.5 요소 노드의 텍스트 조작
### 39.5.1 nodeValue
### 39.5.2 textContent
## 39.6 DOM 조작
### 39.6.1 innerHTML
### 39.6.2 insertAdjacentHTML 메서드
### 39.6.3 노드 생성과 추가
### 39.6.4 복수의 노드 생성과 추가
### 39.6.5 노드 삽입
### 39.6.6 노드 이동
### 39.6.7 노드 복사
### 39.6.8 노드 교체
### 39.6.9 노드 삭제
## 39.7 어트리뷰트
### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티
### 39.7.2 HTML 어트리뷰트 조작
### 39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티
### 39.7.4 data 어트리뷰트와 dataset 프로퍼티
## 39.8 스타일
### 39.8.1 인라인 스타일 조작
### 39.8.2 클래스 조작
### 39.8.3 요소에 적용되어 있는 CSS 스타일 참조
## 39.9 DOM 표준
