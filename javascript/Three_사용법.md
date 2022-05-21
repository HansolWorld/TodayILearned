# Three 사용법

webGL을 사용하기 쉽게 도와주는 라이브러리로 웹브라우저에 3차원 그래픽스 애니메이션을 만들고 응용하기 위한 라이브러리이다.([https://threejs.org/](https://threejs.org/))

three.js를 사용하는 3가지 방법을 알아 볼것이다. three.js 공식 홈페이지에서 기본 source 코드를 받을 수 있다. github에서 clone하여 진행하거나, download로 예제를 진행해본다.

- 방법 1
    
    three github : [https://github.com/mrdoob/three.js/](https://github.com/mrdoob/three.js/)
    
    download : [https://github.com/mrdoob/three.js/archive/master.zip](https://github.com/mrdoob/three.js/archive/master.zip)
    
    source 폴더에 three.js 파일을 확인할 수 있다. 해당 파일을 작업 디렉토리에 넣고 index.htm에서 <script scr=”three.js”></script>를 사용해 three를 불러온다.(예제, [https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene))
    

- 방법 2
    
    모듈 방식으로 three를 사용할 수 있다. 모듈 방식을 사용하면 index.html에서 여러 script src를 전부 불러올 필요가 없다. 
    
    ```jsx
    <script type="module" src="main.js"></script>
    ```
    
    main.js에서 해당 모듈을 불러온다.
    
    ```jsx
    import * as THREE from "./three.js"
    ```
    
- 방법 3
    
    webpack을 사용하는 방법이 있다. 번들링은 각각의 파일을 하나의 모듈로보고 배포용으로 병합하고 포장하는 작업을 말한다. 이런 작업을 수행하는 것들을 번들러라한다. 그중 대표적인것으로 webpack이 있다.
    
    ```bash
    npm i -D @babel/cli @babel/core @babel/preset-env babel-loader clean-webpack-plugin copy-webpack-plugin core-js cross-env html-webpack-plugin source-map-loader terser-webpack-plugin webpack webpack-cli webpack-dev-server
    npm i three
    ```
    
    webpack 플러그인을 npm을 사용해 설치한다. webpack에 대한 자세한 설명은 따로 준비할것이다. 플러그인들을 설치하고 three를 설치한다.
