# Next.js // 2021년 9월 19일(Sun)

## Next.js

### Next.js는 React의 SSR(Server Side Rendering)을 쉽게 구현할 수 있게 도와주는 프레임워크이다.

#### 과거의 웹사이트들은 대부분 SSR로 동작 되어 왔기 때문에, 페이지가 여러개로 구성된 Multi Page Form 방식을 사용했다. 하지만, 스마트폰이 등장하게 되며, 과거의 웹들은 모바일에 최적화가 되어 있지 않기 때문에 사용에 불편함이 커졌다. 그러므로, 사용자들은 모바일 앱과 같은 형태의 웹페이지가 필요하게 되었다.

#### 이러한 문제를 해결하기 위해 React, Angular, Vue 등 여러 라이브러리, 프레임워크가 생겨났고, CSR(Client Server Rendering)이 가능한 SPA(Single Page Application)가 등장하게 되었다.

### SSR

#### SSR이란 Server Side Rendering 약자로 처음 클라이언트가 접속했을때 브라우저에서 자바스크립트 코드를 다운로드 받아 해석할 때 까지 기다리지 않고 서버에서 보여질 HTML을 미리 준비해 클라이언트한테 응답해주는 방식이다.

## Next.js 핵심기능

### 코드 스플리팅

#### 일반적인 리액트 싱글 페이지에서는 초기 렌더링 때 모든 컴포넌트를 내려 받는다. 하지만 규모가 커지고, 용량이 커지면 로딩 속도가 지연될 가능성이 있다. Next는 이러한 문제점을 개선하여 필요에 따라 파일을 불러올 수 있게 여러 개의 파일을 분리하는 코드 스플리팅을 사용한다.

#### 이외에도 검색엔진 최적화, 초기 로딩 성능 개선, SEO(Search Engine Optimization)등이 있다.

### Next.js 장점

#### 1.Typescript 지원: Next.js는 기본적으로 Typscript를 지원하고 있어 따로 모듈을 설치하지 않고도 바로 Typescript를 사용할 수 있다.

#### 2.스마트 번들링: 기본적으로 웹팩이나 바벨을 사용하고 있어 따로 설정을 해주지 않고도 바로 사용할 수 있다.

#### 3. 정적 및 서버 렌더링: 기본적으로 빌드 시에 만든 페이지를 미리 렌더링하여 사용자에게 빠르게 보여준다. 그 후 페이지에 필요한 최소한의 자바스크립트 코드를 불러와 페이지를 사용할 수 있게 해준다.
