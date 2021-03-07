---
title: "Webpack"
categories: 
  - frontend
  - env
  - webpack
toc: true
---

# Webpack이란?

Webpack은 여러개 파일을 하나의 파일로 합쳐주는 bundler다. 하나의 시작점(entry point)으로부터 의존적인 모듈을 전부 찾아내어 하나의 결과물을 만들어 낸다. 

# Webpack 설치 및 사용법

번들 작업을 하는 webpack 패키지와 웹팩 터미널 도구인 webpack-cli를 설치한다.

```
$ npm install -D webpack webpack-cli
```

번들 작업에 필요한 옵션은 `--mode`, `--entry`, `--output` 세 개 옵션이다.

* `--mode`는 웹팩 실행 모드를 의미하는데 개발 버전인 development를 지정한다.
* `--entry`는 시작점 경로를 지정하는 옵션이다.
* `--output`은 번들링 결과물을 위치할 경로다.

```
$ node_modules/.bin/webpack --mode development --entry ./src/app.js --output dist/main.js
```

터미널에 위와 같이 쓰지 않아도 webpack.config.js파일을 만들어 실행할 수도 있다.

webpack.config.js
```js
const path = require("path");

module.exports = {
    mode: "development",
    entry: {
        main: "./src/app.js"
    },
    output: {
        filename: "[name].js",
        path: path.resolve("./dist")
    }
}
```

* '[name]' 은 entry에 추가한 main이 문자열로 들어오는 방식이다.

웹팩 실행을 위한 NPM 커스텀 명령어를 추가한다.

```json
{
    "scripts": {
        "build": "./node_modules/.bin/webpack"
    }
}
```

모든 옵션을 웹팩 설정 파일로 옮겼기 때문에 단순히 webpack 명령어만 실행한다. 이제부터는 `npm run build`로 웹팩 작업을 지시할 수 있다.

# 로더
## 로더의 역할

웹팩은 모든 파일을 모듈로 바라본다. 자바스크립트로 만든 모듈 뿐만아니라 스타일시트, 이미지, 폰트까지도 전부 모듈로 보기 때문에 import 구문을 사용하면 자바스크립트 코드 안으로 가져올 수 있다.

이것이 가능한 이유는 웹팩의 **로더** 덕분이다. 로더는 타입스크립트 같은 다른 언어를 자바스크립트 문법으로 변환해 주거나 이미지를 data URL 형식의 문자열로 변환한다. 뿐만아니라 CSS 파일을 자바스크립트에서 직접 로딩할수 있도록 해준다.

## 로더 설정 방법
### 커스텀로더

myLoader.js:
```js
module.exports = function myLoader(content) {
    console.log("myLoader가 동작함");
    return content.replace("console.log(", "alert(")))
}
```

webpack.config.js
```js
module: {
    rules: [{
        test: /\.js$/.
        use: [path.resolve("./myLoader.js")]
    }]
}
```
`module.rules` 배열에 모듈을 추가하는데 test와 use로 구성된 객체를 전달한다.

* test에는 로딩에 적용할 파일을 지정한다. 파일명 뿐만아니라 파일 패턴을 정규표현식으로 지정할 수 있다.
* use에는 이 패턴에 해당하는 파일에 적용할 로더를 설정하는 부분이다.

### 자주 사용하는 로더

* css-loader - CSS 파일을 자바스크립트에서 불러와 사용하려면 CSS를 모듈로 변환하는 작업이 필요하다. <u>css-loader</u>가 그러한 역할을 하는데 우리 코드에서 CSS 파일을 모듈처럼 불러와 사용할 수 있게끔 해준다.
* style-loader - 자바스크립트로 변경된 스타일을 동적으로 돔에 추가하는 로더이다. CSS를 번들링하기 위해서는 css-loader와 style-loader를 함께 사용한다. style-loader를 css-loader보다 앞에 추가한다.
* file-loader - 파일을 모듈 형태로 지원하고 웹팩 아웃풋에 파일을 옮겨주는 역할을 한다.
* url-loader - 사용하는 이미지 갯수가 많다면 네트웍 리소스를 사용하는 부담이 있고 사이트 성능에 영향을 줄 수도 있다. 만약 한 페이지에서 작은 이미지를 여러개 사용한다면 <u>Data URI Scheme</u>을 이용하는 방법이 더 나은 경우도 있다. 이미지를 Base64로 인코딩하여 문자열 형태로 소스코드에 넣는 형식이다.


# 플러그인
## 플러그인의 역할 

웹팩에서 알아야 할 마지막 기본 개념이 플러그인이다. 로더가 파일 단위로 처리하는 반면 플러그인은 번들된 결과물을 처리한다. 번들된 자바스크립트를 난독화 한다거나 특정 텍스트를 추출하는 용도로 사용한다.

### 커스텀 플러그인 만들기

myPlugin.js:
```js
module.exports = class MyPlugin {
    apply(compiler) {
        comiler.hooks.done.tap("My Plugin", stats => {
            console.log("MyPlugin: done")
        })

        comiler.plugin("emit" , (compilation, callback) => {
            const source = compilation.assets["main.js"].source()
            console.log(source)
            callback();
        })
    }
}
```

webpack.config.js
```js
const MyPlugin = require("./myPlugin");

module.exports = {
    plugins: [new MyPlugin()]
}
```

### 자주 사용하는 플러그인

개발하면서 플러그인을 직접 작성할 일은 거의 없다. 웹팩에서 직접 제공하는 플러그인을 사용하거나 써드파티 라이브러리를 찾아 사용한다.

* BannerPlugin - 결과물에 빌드 정보나 커밋 버전같은 걸 추가할 수 있다.
* DefinePlugin - 빌드타임에 결정된 값을 어플리케이션에 전달할 때 사용한다.
* HtmlWebpackPlugin - 불필요한 주석을 제거하거나 빈칸을 제거할 수 있다. 또한, ejs문법으로 전역변수 값을 전달할 수 있다.
* CleanWebpackPlugin - 빌드 결과물은 아웃풋 경로에 모이는데 과거 파일이 남아있을수 있다. 불필요한 빌드내용을 정리해준다.
* MiniCssExtractPlugin - 개발 환경에서는 CSS를 하나의 모듈로 처리해도 상관없지만 프로덕션 환경에서는 분리하는 것이 효과적이다. CSS를 별도 파일로 뽑아내는 플러그인이다.