# 49장. Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축
## 49.1 Babel
```js
[1, 2, 3].map(n => n ** n)
```
구현 브라우저에서는 ES6의 화살표 함수와 ES7의 지수 연산자를 지원하지 않을 수 있다. Babel을 사용하면 위 코드를 ES5 사양으로 변환할 수 있다.
```js
"use strict"

[1, 2, 3].map(function (n) {
  return Math.pow(n, n)
})
```
이처럼 Babel은 ES6/ES.NEXT(제안단계)로 구현된 최신 사양의 소스코드를 구형 브라우저에서 동작하는 ES5사양의 소스코드로 변환(트랜스파일링)할 수 있다. 
### 49.1.1 Babel 설치
npm을 사용해 설치할 수 있다.
```shell
$ mkdir esnext-project && cd exnext-project
$ npm init -y
$ npm install --save-dev @babel/core @babel/cli
```
package.json를 설정한다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3"
  }
}
```
Babel, Webpack, 플러그인의 버전은 빈번하게 업그레이드된다. npm install 은 언제나 최신 버전의 패키지를 설치하므로 버전을 명시하고 싶다면 패키지 이름뒤에 @버전을 지정한다.
```shell
npm install --save-dev @babel/core@7.10.3 @babel/cli@7.10.3
```
### 49.1.2 Babel 프리셋 설치와 babel.config.json 설정 파일 작성
Babel을 사용하려면 @babel/preset-env를 설치해야 한다. 이는 Babel 플러그인을 모아둔 것으로 Babel프리셋이라 한다. Babel 프리셋은 다음과 같다.
-  @babel/preset-env
-  @babel/preset-flow
-  @babel/preset-react
-  @babel/preset-typescript

 @babel/preset-env는 필요한 플러그인들을 지원 환경에 맞춰 동적으로 설치하고 프로젝트 지원 환경은 Browserslist형식으로 .browserslistrc파일에서 설정할 수 있다.   
 기본설정으로 셋팅할 수 잇다.
 ```shell
 $ npm install --sace-dev  @babel/preset-env
 ```
 설치가 완료되면 package.json 설정이 변경된다.
 ```json
 {
  "name": "esnext-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/preset-env": "^7.10.3"
  }
}
 ```
 설치가 완료되면 babel.config.json 설정파일을 생성하고 @babel/preset-env를 사용하겠다는 의미를 담아둔다.
 ```json
 {
  "presets": ["@babel/preset-env"]
}
 ```
### 49.1.3 트랜스파일링
Babel을 사용해 ES6/ES.NEXT 사양의 소스 코드를 ES5 사양의 코드로 트랜스파일링 할 수 있다. CLI를 사용할수도 있으나 번거로우므로 npm scripts에 Babel CLI 명령어를 등록해 사용할 수 있다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/preset-env": "^7.10.3"
  }
}
```
npm scripts의 build는 src/js 폴더에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후 그 결과물을 dist/js 폴더에 저장한다.
- -w: 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지해 자동으로 트랜스파일한다.
- -d: 트랜스파일링된 결과물이 저장된 폴더를 지정한다. 지정된 폴더가 존재하지 않는다면 자동 생성한다.
```js
// src/js/lib.js
export const pi = Math.PI // ES6 모듈

export function power(x, y) {
  return x ** y // ES7: 지수 연산자
}

// ES6 클래스
export class Foo {
  #private = 10 // stage 3: 클래스 필드 정의 제안

  foo() {
    // stage 4: 객체 Rest/Spread 프로퍼티 제안
    const { a, b, ...x } = { ...{ a: 1, b: 2 }, c: 3, d: 4 }
    return { a, b, x }
  }

  bar() {
    return this.#private
  }
}
```
```js
// src/js/main.js
import { pi, power, Foo } from './lib'

console.log(pi)
console.log(power(pi, pi))

const f = new Foo()
console.log(f.foo())
console.log(f.bar())
```
터미널에서 명령어를 입력해 트랜스파일링을 실행한다.
```shell
$ npm run build
```
### 49.1.4 Babel 플러그인 설치
설치가 필요한 Babel 플러그인은 Babel 홈페이지에서 검색할 수 있다. 검색된 Babel 플러그인중 public/private 클래스 필드를 지원하는 @babel/plugin-proposal-class-properties를 설치할 수 있다.
```shell
$ npm install --save-dev @babel/plugin-proposal-class-properties
```
설치완료후 package.json은 변경된다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3"
  }
}
```
설치한 플러그인은 babel.config.json 설정파일에도 추가해야 한다.
```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```
다시 명령어를 입력해 트랜스파일링을 할 수 있다.
```js
$ npm run build
```
트랜스 파일링이 성공하면 루트 폴더에 dist/js 폴더가 자동 생성되고 트랜스파일링된 main.js, lib.js가 저장된다. 
```shell
$ node dist/js/main
```
### 49.1.5 브라우저에서 모듈 로딩 테스트
위에서 실행된 모듈은 Node.js 환경에서 동작한 것이다. src/js/main.js가 Babel에 의해 트랜스파일린된 결과는 다음과 같다.
```js
// dist/js/main.js
"use strict"

var _lib = require("./lib")

// src/js/main.js
console.log(_lib.pi)
console.log((0, _lib.power)(_lib.pi, _lib.pi))
var f = new _lib.Foo()
console.log(f.foo())
console.log(f.bar())
```
브라우저는 CommonJS 방식의 require함수를 지원하지 않아 그대로 브라우저에서 실행하면 에러가 발생한다.
```html
<!DOCTYPE html>
<html>
<body>
  <script src="dist/js/lib.js"></script>
  <script src="dist/js/main.js"></script>
</body>
</html>
```
브라우저의 ES6 모듈을 사용하도록 Babel을 설정할수도 있으나 ESM을 사용하는것은 문제가 있다. 이를 Webpack을 통해 해결할 수 있다.
## 49.2 Webpack
Webpack은 의존 관계에 있는 자바스크립트, CSS, 이미지등의 리소스를 하나의 파일로 번들링하는 모듈 번들러다. 하나의 파일로 번들링 되므로 별도의 모듈로더가 필요없다. 여러 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러개의 자바스크립트 파일을 로드해야하는 번거로움도 사라진다.   
Webpack과 Babel을 사용해 개발환경을 구축하고 Webpack이 자바스크립트 파일을 번들링하기 전에 Babel을 로드해 트랜스파일링 작업을 실행하도록 설정해보자.
### 49.2.1 Webpack 설치
```shell
$ npm install --seve-dev webpack webpack-cli
```
완료된후 package.json는 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```
### 49.2.2 babel-loader 설치
Webpack 모듈을 번들링할 때 Babel을 사용해 트랜스파일링하도록 babel-loader를 설치한다.
```shell
$ npm install --save-dev babel-loader
```
npm script를 변경하여 Babel 대신 Webpack을 실행하게 수정하고 package.json 파일에 script를 변경한다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "webpack -w"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "babel-loader": "^8.1.0",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```
### 49.2.3 webpack.config.js 설정 파일 작성
webpack.config.js는 Webpack이 실행될 떄 참조하는 설정파일이다.
```js
const path = require('path')

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: './src/js/main.js',
  // 번들링된 js 파일의 이름(filename)과 저장될 경로(path)를 지정
  // https://webpack.js.org/configuration/output/#outputpath
  // https://webpack.js.org/configuration/output/#outputfilename
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js'
  },
  // https://webpack.js.org/configuration/module
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [
          path.resolve(__dirname, 'src/js')
        ],
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties']
          }
        }
      }
    ]
  },
  devtool: 'source-map',
  // https://webpack.js.org/configuration/mode
  mode: 'development'
}
```
Webpack을 실행해 트랜스파일링 및 번들링을 실행할수 있다.
```shell
$ npm run build
```
dist/js 폴더에 bundle.js가 생성되고 이는 main.js, lib.js 모듈이 하나로 번들링된 결과물이다.
```html
<!DOCTYPE html>
<html>
<body>
  <script src="./dist/js/bundle.js"></script>
</body>
</html>
```
index,html을 위와같이 변경하고 실행하면 문제없이 실행되는것을 볼수 있다.
### 49.2.4 babel-polyfill 설치
Babel이 지원하지 않는 코드가 남아있을 수 있다.
```js
// src/js/main.js
import { pi, power, Foo } from './lib'

console.log(pi)
console.log(power(pi, pi))

const f = new Foo()
console.log(f.foo())
console.log(f.bar())

// polyfill이 필요한 코드
console.log(new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 100)
}))

// polyfill이 필요한 코드
console.log(Object.assign({}, { x: 1 }, { y: 2 }))

// polyfill이 필요한 코드
console.log(Array.from([1, 2, 3], v => v + v))
```
위 코드를 트랭스파일링과 번들링하면 아래와 같이 된다.
```js
...
// 190 line
console.log(new Promise(function (resolve, reject) {
  setTimeout(function () {
    return resolve(1)
  }, 100)
})) // polyfill이 필요한 코드

console.log(Object.assign({}, {
  x: 1
}, {
  y: 2
})) // polyfill이 필요한 코드

console.log(Array.from([1, 2, 3], function (v) {
  return v + v
}))
...
```
ES5 사양으로 댗체할수 없는 기능은 트랜스파일링 되지 않고 이를 사용하기 위해서는 @babel/polyfill을 설치해야한다.
```shell
$ npm install @babel/polyfill
```
설치후 package.json은 다음과 같다.
```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "webpack -w"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "babel-loader": "^8.1.0",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  },
  "dependencies": {
    "@babel/polyfill": "^7.10.1"
  }
}
```
@babel/polyfill은 개발환경에서만 사용하는것이 아니라 실제 운영 환경에서도 사용해야 한다. 따라서 의존성으로 설치하는 --save-dev 옵션을 지정하지 않는다.   
ES6의 import를 사용하는 경우 진입점의 선두에서 먼저 폴리필을 로드하도록 한다.
```js
// src/js/main.js
import "@babel/polyfill"
import { pi, power, Foo } from './lib'
...
```
Webpack을 사용하는 경우 webpack.config.js파일의 entry 배열에 폴리필을 추가한다.
```js
const path = require('path')

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: ['@babel/polyfill', './src/js/main.js'],
...
```
드음 명령어를 실행해 Webpack을 실행한다.
```shell
$ npm run build
```