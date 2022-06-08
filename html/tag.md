태그의 갯수는 대략 130개정도가 있다. 이중 웹에서 사용되는 태그는 20~30여개 정도이다. 자주 사용하는 태그를 위주로 알아보려 한다.

### 제목(HEADING)

```html
<h1>Hello</h1>
```

제목을 나타내는 태그로, h1~h6까지 있다. 숫자가 작을수록 대제목으로 h1은 페이지를 대표하는 제목이 된다. h6로 갈수록 소제목이 된다.

### 단락과 개행(PARAGRAPH, LINEBREAK)

```html
<p>
    This is first paragraph.
</p>
<p>
    This is second paragraph.<br>
    This is second paragraph.
</p>
```

`<p>`태그는 글의 단락을 나타낸다. `<br>`태그는 줄바꿈을 의미한다. `<p>`태그 내에서 개행을 하기 위해서는 `<br>`태그를 사용해야한다.

### 텍스트 관련 태그(B, I, U, S)

```html
<P>
    <b>Lorem</b> <i>ipsum</i> dolor sit amet<br>
    <u>Lorem</u> <s>ipsum</s> dolor sit amet<br>
</p>
```

`<b>`태그는 글자를 굵게 표현한다. `<i>`태그는 글자를 이탤릭체로 표현한다. `<u>`는 글자에 밑줄을 표현한다. `<s>`태그는 글자의 중간선을 표현한다.

### 앵커(ANCHOR)

```html
<a href="http://www.naver.com/" target="_blank">네이버</a>
```

`<a>`태그는 링크를 생성하는 태그이다. `<a>`태그는 반드시 href속성을 가지고 있어야한다. href속성의 값은 링크가 향하는 곳의 URL이다. target속성은 링크된 리소스를 어디에 표시할지를 나타내는 속성이다. 속성값으로는 _self, _black, _parent, _top이 있다. 

- _self: 화면에 표시한단 의미로 target 속성이 선언되지 않으면 기본적으로 _self와 같다.
- _black: 새로운 창에 표시한다는 의미로 외부 페이지가 나타나게하는 속성이다.
- _parent, _top: 프레임이라는 특정 조건에서 동작하는 속성으로, 최근에는 프레임을 잘 다루지 않는다.

페이지 내부의 특정 요소로 이동시킬 수 있다. href에 값으로 “#이동하고자하는 요소id “로 이동할 수 있다.

### 의미없이 요소를 묶기 위한 태그(CONTAINER)

```html
<div>
    <span>Lorem</span> ipsum dolor sit.
</div>
```

태그 자체에는 의미가 없고 어떤 요소를 묶기위한 태그가 존재한다. 스타일을 주기위해서나 서버에서 보내는 데이터를 담기위해 사용된다. 대표적으로 `<div>`과 `<span>`이 있다. `<div>`는 블록레벨로, 블록은 한줄 전체를 의미한다. `<span>`은 인라인레벨로 블록안에 요소단위를 말한다. 

### 리스트(ul, ol, dl)

```html
<ul> 
    <li> 콩나물</li> 
    <li> 파</li> 
    <li> 국  간장</li> 
    ... 
</ul>
```

태그의 순서가 업는 리스트를 표현할때 `<ul>`을 사용한다.

```html
<ol>
    <li>냄비에 국물용 멸치를 넣고 한소끔 끓여 멸치 육수를 7컵(1,400ml) 만든다.</li>
    <li>콩나물을 넣고 뚜껑을 덮어 콩나물이 익을 때까지 끓인다.</li>
    <li>뚜껑을 열고 대파, 마늘, 고춧가루를 넣고 끓인다.</li>
    ...
</ol>
```

순서가 있는 리스트를 표현할때 `<ol>`을 사용한다.

```html
<dl>
    <dt>리플리 증후군</dt>
    <dd>허구의 세계를 진실이라 믿고 거짓된 말과 행동을 상습적으로 반복하는 반사회적 성격장애를 뜻하는 용어</dd>
    <dt>피그말리온 효과</dt>
    <dd>타인의 기대나 관심으로 인하여 능률이 오르거나 결과가 좋아지는 현상</dd>
    <dt>언더독 효과</dt>
    <dd>사람들이 약자라고 믿는 주체를 응원하게 되는 현상</dd>
</dl>
```

용어와 정의를 설명할떄 `<ol>`을 사용한다. 용어와 설명이 하나의 세트를 이룬다. `<dt>`에는 용어, `<dd>`에는 설명이 들어간다.

### 이미지(IMAGE)

문서에 이미지를 삽입하기 위한 태그이다.

```html
<img src="./images/pizza.png" alt="피자">
```

이미지태그를 사용할때 src를 반드시 필요로 한다. src는 이미지의 경로를 의미한다. alt는 이미지의 대체 텍스트를 입력한다. 이미지에 대한 정보를 말하고 필수로 입력해야 한다. width/height 속성도 존재한다. 이미지의 넓이, 높이를 넣는다. width/height 속성을 선언하는 것이 성능적인 측면에서 좋다. 선언하지 않을경우 이미지의 원본 크기로 노출되며 하나만 선언하면 한 속성은 다른속성에 맞게 자동 비율로 계싼된다. 반면 두속성 모두를 선언하면 비율과 무관하게 선언된 크기로 변경된다.

cf. 이미지 파일 형식

- gif : 제한적인 색을 사용하고 용량이 적으며 투명 이미지와 애니메이션 이미지를 지원하는 형식
- jpg : 사진이나 일반적인 그림에 쓰이며 높은 압축률과 자연스러운 색상 표현을 지원하는 형식(투명을 지원하지 않는다.)
- png : 이미지 손실이 적으며 투명과 반투명을 모두 지원하는 형식

### 표(TABLE)

데이터 표를 나타내는 태그이다.

```html
<table>
    <tr>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
    </tr>
    <tr>
        <td>5</td>
        <td>6</td>
        <td>7</td>
        <td>8</td>
    </tr>
    <tr>
        <td>9</td>
        <td>10</td>
        <td>11</td>
        <td>12</td>
    </tr>
    <tr>
        <td>13</td>
        <td>14</td>
        <td>15</td>
        <td>16</td>
    </tr>
</table>
```

- <table> : 표를 나타내는 태그
- <tr> : 행을 나타내는 태그
- <th> : 제목 셀을 나타내는 태그
- <td> : 셀을 나타내는 태그

reference:

[https://developer.mozilla.org/en-US/docs/Web/HTML/Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

[http://html5doctor.com/element-index/](http://html5doctor.com/element-index/)

[https://www.w3schools.com/tags/default.asp](https://www.w3schools.com/tags/default.asp)
