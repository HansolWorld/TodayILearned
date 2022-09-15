# 44장. REST API
REST는 HTTP 기반으로 크라이언트가 서버 리소스에 접근하는 방식을 규정한 아키텍처로 REST API는 REST를 기반으로 서비스 API를 구현한 것을 말한다.
## 44.1 REST API의 구성
REST API는 자원, 행위, 표현으로 구성된다. REST는 자체 표현구조로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.
|구성 요소|내용|표현 방법|
|---|---|---|
|지원|자원|URI(엔드포인트)|
|행위|자원에 대한 행위|HTTP 요청 메서드|
|표현|자원에 대한 해위의 구체적 내용|페이로드|
## 44.2 REST API 설계 원칙
REST에서 가장 기본 원칙은 URI는 리소스를 표현하는데 집중하고 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것이다.
#### 1. URI는 리소스를 표현해야 한다.
URI는 리소스를 표현하는데 중점을 둬야하며 이름은 동사보다는 명사를 사용해야 한다. 따라서 get같은 행위에 대한 표현이 들어가서는 안된다.
```shell
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```
#### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.
|HTTP 요청 매서드|종류|목적|페이로드|
|---|---|---|---|
|GET|index/retrieve|모든/특정 리소스 취득|X|
|POST|create|리소스 생성|O|
|PUT|replace|리소스의 전체 교체|O|
|PATCH|modify|리소스의 일부 수정|O|
|DELETE|delete|모든/특정 리소스 삭제|X|

리소스에 대한 행위는 URI에 표현하지 않는다. 리소스를 취득해야 하는 경우 GET, 리소스를 삭제해야하는 경우 DELETE를 사용해 리소스에 대한 행위를 명확히 표현한다.
```shell
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```
## 44.3 JSON Server를 이용한 REST API 실습
### 44.3.1 JSON Server 설치
HTTP 요청을 받으려면 서버가 필요하다. JSON server는 json 파일을 사용하여 가상의 REST API 서버를 구축할 수 있는 툴이다. 터미널에서 JSON server 설치한다.
```shell
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```
### 44.3.2 db.json 파일 생성
프로젝트 폴더에 리소를 제공하는 데이터베이스 역할의 db.json파일을 생성한다.
```js
{
  "todos": [
    {
      "id": 1,
      "content": "HTML",
      "completed": true
    },
    {
      "id": 2,
      "content": "CSS",
      "completed": false
    },
    {
      "id": 3,
      "content": "Javascript",
      "completed": true
    }
  ]
}
```
### 44.3.3 JSON Server 실행
서버를 실행해 db.json 파일의 변경을 감지하는 옵션 watch를 추가한다.
```shell
## 기본포트(3000) 사용/ watch 옵션 적용
$ json-server --watch db.json
```
기본포트는 3000이고 변경을 하려면 port 옵션을 추가한다.
```shell
## 기본포트(3000) 사용/ watch 옵션 적용
$ json-server --watch db.json --port 5000
```
package.json 파일의 scripts를 수정하여 JSON Server를 실행할 수 있다.
```js
{
  "name": "json-server-exam",
  "version": "1.0.0",
  "scripts": {
    "start": "json-server --watch db.json"
  },
  "devDependencies": {
    "json-server": "^0.16.1"
  }
}
```
npm start 명령어를 입력하면 JSON server를 실행한다.
### 44.3.4 GET 요청
todos 리소스에서 모든 todo를 취득한다. JSON Server의 루투 폴더에 public 폴더를 생성하고 JSON Server를 중단한 후 재실행한다. public 폴더에 get_index.html을 추가하고 브라우저에서 http://localhost:3000/get_index.html로 접속한다.
```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest()

    // HTTP 요청 초기화
    // todos 리소스에서 모든 todo를 취득(index)
    xhr.open('GET', '/todos')

    // HTTP 요청 전송
    xhr.send()

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response
      } else {
        console.error('Error', xhr.status, xhr.statusText)
      }
    }
  </script>
</body>
</html>
```
todos 리소스에서 id를 사용해 특정 todo를 취득한다. public 폴더에 get_retrieve.html을 추가하고 http://localhost:3000/get_retrieve.html로 접속한다.
```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest()

    // HTTP 요청 초기화
    // todos 리소스에서 id를 사용하여 특정 todo를 취득(retrieve)
    xhr.open('GET', '/todos/1')

    // HTTP 요청 전송
    xhr.send()

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response
      } else {
        console.error('Error', xhr.status, xhr.statusText)
      }
    }
  </script>
</body>
</html>
```
### 44.3.5 POST 요청
todos 리소스에 새로운 todo를 생성한다. post 요청시에는 setRequestHeader 메서드를 사용해 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.
```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest()

    // HTTP 요청 초기화
    // todos 리소스에 새로운 todo를 생성
    xhr.open('POST', '/todos')

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json')

    // HTTP 요청 전송
    // 새로운 todo를 생성하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }))

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200(OK) 또는 201(Created)이면 정상적으로 응답된 상태다.
      if (xhr.status === 200 || xhr.status === 201) {
        document.querySelector('pre').textContent = xhr.response
      } else {
        console.error('Error', xhr.status, xhr.statusText)
      }
    }
  </script>
</body>
</html>
```
### 44.3.6 PUT 요청
특정 리소스 전체를 교체할 떄 사용한다. todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스를 교체한다면 PUT 요청시에 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전솔할 페이로드의 MIME 타입을 지정해야 한다.
```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest()

    // HTTP 요청 초기화
    // todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체
    xhr.open('PUT', '/todos/4')

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json')

    // HTTP 요청 전송
    // 리소스 전체를 교체하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }))

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response
      } else {
        console.error('Error', xhr.status, xhr.statusText)
      }
    }
  </script>
</body>
</html>
```
### 44.3.7 PATCH 요청
특정 리소스의 일부를 수정할떄 사용한다. PATCH 요청시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.
```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest()

    // HTTP 요청 초기화
    // todos 리소스의 id로 todo를 특정하여 completed만 수정
    xhr.open('PATCH', '/todos/4')

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json')

    // HTTP 요청 전송
    // 리소스를 수정하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ completed: false }))

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response
      } else {
        console.error('Error', xhr.status, xhr.statusText)
      }
    }
  </script>
</body>
</html>
```
### 44.3.8 DELETE 요청
특정 리소스를 삭제한다.
```html
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest()

    // HTTP 요청 초기화
    // todos 리소스에서 id를 사용하여 todo를 삭제한다.
    xhr.open('DELETE', '/todos/4')

    // HTTP 요청 전송
    xhr.send()

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response
      } else {
        console.error('Error', xhr.status, xhr.statusText)
      }
    }
  </script>
</body>
</html>
```