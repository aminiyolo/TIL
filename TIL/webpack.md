# webpack // 2021년11월 21일(일)

## webpack이란?

### 웹팩이란 최신 프런트엔드 프레임워크에서 가장 많이 사용되는 모듈 번들러(Module Bundler)이다. 모듈 번들러란 웹 애플리케이션을 구성하는 자원(HTML, CSS, Javscript, Images 등)을 모두 각각의 모듈로 보고 이를 조합해서 병합된 하나의 결과물을 만드는 도구를 의미한다.

### 모듈이란?

#### 모듈이란 프로그래밍 관점에서 특정 기능을 갖는 작은 코드 단위를 의미한다. 자바스크립트로 치면 아래와 같은 코드가 모듈이다.

```
// math.js
function sum(a, b) {
  return a + b;
}

function substract(a, b) {
  return a - b;
}

const pi = 3.14;

export { sum, substract, pi }
```

#### 더하기, 빼기, 파이값 총 3가지 기능을 갖고 있는 모듈이다.

## 웹팩의 4가지 주요 속성은 다음과 같다.

### entry

### output

### loader

### plugin

## Entry

#### entry 속성은 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점이자 자바스크립트 파일 경로이다.

```
// webpack.config.js
module.exports = {
  entry: './src/index.js'
}
위 코드는 웹팩을 실행했을 때 src 폴더 밑의 index.js 을 대상으로 웹팩이 빌드를 수행하는 코드이다.
```

#### entry 속성에 지정된 파일에는 웹 애플리케이션의 전반적인 구조와 내용이 담겨져 있어야 한다. 웹팩이 해당 파일을 가지고 웹 애플리케이션에서 사용되는 모듈들의 연관 관계를 이해하고 분석하기 때문에 애플리케이션을 동작시킬 수 있는 내용들이 담겨져 있어야 한다.

#### entry 유형은 위 코드처럼 1개가 될 수도 있지만 아래와 같이 여래 개가 될 수도 있다.

```
entry: {
  login: './src/LoginView.js',
  main: './src/MainView.js'
}
```

## Output

#### output 속성은 웹팩을 돌리고 난 결과물의 파일 경로를 의미한다.

```
// webpack.config.js
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
// entry 속성과는 다르게 객체 형태로 옵션들을 추가해야 한다.
```

#### 최소한 filename은 지정해줘야 하며 일반적으로 아래와 같이 path 속성을 함께 정의한다.

```
// webpack.config.js
var path = require('path');

module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
```

#### 여기서 filename 속성은 웹팩으로 빌드한 파일의 이름을 의미하고, path 속성은 해당 파일의 경로를 의미한다. 그리고 path 속성에서 사용된 path.resolve() 코드는 인자로 넘어온 경로들을 조합하여 유효한 파일 경로를 만들어주는 Node.js API이다.

## Loader

### 로더(Loader)는 웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images, 폰트 등)들을 변환할 수 있도록 도와주는 속성이다.

```
// webpack.config.js
module.exports = {
  module: {
    rules: []
  }
}
// 엔트리나 아웃풋 속성과는 다르게 module라는 이름을 사용한다.
```

### CSS Loader 적용하기

#### css 파일을 해석하기 위해서는 CSS Loader 설정을 해주어야 한다.

```
// webpack.config.js
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['css-loader']
      }
    ]
  }
}
// 위 코드는 해당 프로젝트의 모든 CSS 파일에 대해서 CSS 로더를 적용하겠다는 의미이다.
```

### 로더 적용 순서 (중요!)

#### 특정 파일에 대해 여러 개의 로더를 사용하는 경우 로더가 적용되는 순서에 주의해야 합니다. 로더는 기본적으로 <오른쪽에서 왼쪽 순>으로 적용된다

```
module: {
  rules: [
    {
      test: /\.scss$/,
      use: ['css-loader', 'sass-loader']
    }
  ]
}
// 위 코드는 scss 파일에 대해 먼저 Sass 로더로 전처리(scss 파일을 css 파일로 변환)를 한 다음 웹팩에서 CSS 파일을 인식할 수 있게 CSS 로더를 적용하는 코드이다.
```

## Plugin

### 플러그인(plugin)은 웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성이다. 로더랑 비교하면 로더는 파일을 해석하고 변환하는 과정에 관여하는 반면, 플러그인은 해당 결과물의 형태를 바꾸는 역할을 한다고 보면 된다.

```
// 플러그인 선언 방법
// webpack.config.js
module.exports = {
  plugins: []
}
```

### 플러그인의 배열에는 생성자 함수로 생성한 객체 인스턴스만 추가될 수 있다.

```
// webpack.config.js
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin(),
    new webpack.ProgressPlugin()
  ]
}
```
