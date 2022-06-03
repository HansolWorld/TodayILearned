# HTML?

**H**yper **T**ext **M**arkup **L**anguge의 약자로, 웹페이지를 만드는 언어이다. hyper text는 인터넷에서 말하는 링크를 말한다. markup languge는 정보를 구조적으로 표현 가능한 언어로 html은 이런 특징을 가지는 언어이다. 파일 확장자는 html을 사용하고 안에는 html언어로 작성되어 있다.

웹페이지는 전부 html로 되어있다. html을 작성하면 브라우저가 해석해 표현해준다. html은 90년대 초반에 개발된 언어로 팀 버너스리에 의해 정보나 문서를 신속하게 공유하기 위해 만들어 졌다. 

html의 문법에는 태그, 속성, 중첩, 빈태그, 공백, 주석으로 이루어져 있다.

## 태그란?

html은태그의 집합이다. 태그는 <>로 감싸져 있으며 사이에 이름이 들어간다. 대부분의 태그는 시작과 종료로 이루어 지며 종료 태그는 이름 앞에 ‘/’가 붙는다.

```html
<h1>Hello, HTML</h1>
```

## 요소란?

내용을 포함한 태그 전체를 요소라 한다. 많은 사람들이 태그와 요소를 같은 의미로 사용해 혼동이 일어난다. 주의해야 한다. 

## 속성이란?

태그에 추가 정보를 제공하거나 동작이나 표현을 제어할 수 있는 설정값을 의미한다. 이름과 값으로 이루어져 있으며 시작퇴그에서 이름뒤에 공백 구분후 이름=”속성값"으로 표현한다. 속성값은 “”로 감싼다. 

```html
<h1 id="title">Hello, HTML</h1>
```

id속성을 추가해 title값을 선언하는 코드이다. 의미와 용도에 따라 여러 속성이 존재하며 여러 속성을 선언할 수 있다. 속성은 공백으로 구분한다.

```html
<h1 id="title" class="main">Hello, HTML</h1>
```

속성의 순서는 태그에 영향을 미치지 않는다. 속성의 종류에 따라 모든 태그에 사용할 수 있는 글로벌 속성과 특정 태그에만 사용할 수 있는 속성으로 구분된다. 또한, 선택적으로 쓸수 있는 속성과 필수로 필요한 필수속성으로 구분된다.

## 태그의 중첩

태그는 중첩이 가능하다. 즉, 태그안에 다른 태그를 선언할 수 있다. 주의할점은 안에 선언되는 태그는 부모태그를 벗어나면 안된다. 

```html
<h1>Hello, <i>HTML</i></h1>
```

i태그를 종료한 뒤 h1태그를 종료해야 한다. 태그 유효성을  검사합는 검증 페이지를 이용할 수 있다([http://validator.kldp.org/](http://validator.kldp.org/))

## 빈태그란?

태그는 기본적으로 시작태그와 종료태그 2개가 1쌍으로 이루어져 있다. 그 사이에 내용이 들어가게 된다. 예외적으로 시작태그만 존재하고 종료태그가 없는 태그가 존재한다. 이런 태그를 빈태그라 한다.

- <br>
- <img src=””>
- <input type=””>

빈태그는 내용이 없어 종료태그를 필요로 하지 않는다. 이미지나 비디오등 외부 요소를 삽입하는 경우가 빈태그를 사용한다. 

## 빈태그의 특징

빈태그의 대표적인 경우는 브라우저가 직접 화면에 내용을 그려줘야 하는 경우이다. 이런 태그는 브라우저가 내용을 대체한다 해서 replacement태그, 대체태그라 한다. 빈태그에 대체태그만 있는것은 아니며 실제로 화면에 출력될 내용이 없어 다른 용도로 사용되는 태그도 존재한다. <br> 태그의 경우이다. 

## 공백

html을 작성하다보면 공백을 처리하는 방법에대해 이해가 가지 않는다. html에서 2칸 이상의 공백은 무시해버린다.

```html
<h1>Hello, HTML</h1>
<h1>Hello,     HTML</h1>
<h1>
    Hello,
    HTML
</h1>
```

위에 경우가 모두 같게 출력된다.

## 주석

주석은 화면에 노출되지 않고 메모의 목적으로 사용하는 것을 말한다.

```html
<!-- 여기 작성되는 내용은 주석처리 된다. -->
```

<!—  —>의 사이에 작성되는 내용은 화면에 표시되지 않으며 주석처리 된다. 

## HTML 기본구조

기본구조는 웹 문서를 작성할때 반드시 들어가야하는 기본적인 내용으로 크게 문서타입 정의와 <html>요소로 구분한다.

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>HTML</title>
    </head>
    <body>
        <h1>Hello, HTML</h1>
    </body>
</html>
```

## 문서 타입 정의

문서타입 정의는 보통 DTD라고 부른다. 이문서가 어떤 버전으로 작성됬는지 브라우저에게 알려주는 선언문이며 최상단에 선언되어야 한다.

## <HTML>요소

문서타입의 선언 후에는 <html>태그가 나와야한다. 자식으로는 <head>, <body>가 있다. lang속성은 문서가 어느 언어로 작성되었는지를 의미한다. <head>에 위치하는 태그는 브라우저에 표시되지 않는다. 문서의 기본 정보 설정이나 외부 스타일시트 파일 및 js파일을 연결하는 역할을 한다. <meta>태그의 charset속성은 문자의 인코딩 방식을 지정한다. <body>태그는 실제 브라우저에 나타내는 내용이 들어간다. 

reference:

[https://developer.mozilla.org/ko/docs/Web/HTML](https://developer.mozilla.org/ko/docs/Web/HTML)

[https://developer.mozilla.org/ko/docs/Web/HTML/Element](https://developer.mozilla.org/ko/docs/Web/HTML/Element)

[https://developer.mozilla.org/en-US/docs/Glossary/Empty_element](https://developer.mozilla.org/en-US/docs/Glossary/Empty_element)

[https://www.w3schools.com/tags/tag_comment.asp](https://www.w3schools.com/tags/tag_comment.asp)