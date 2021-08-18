# Code Splitting // 2021년 8월 18일(Wed)

## What is Code Splitting ?

### 자바스크립트로 애플리케이션을 개발하게 되면, 기본적으로 하나의 파일에 모든 JS 로직들이 들어가게 된다. 그러므로 프로젝트 규모가 커지면 커질수록 당장 필요하지 않은 자바스크립트 파일 용량도 커질 것이다. 용량이 커지므로 당연히 페이지의 로딩속도가 느려지게 될 것이다.

### 코드 스플리팅이란 하나의 큰 번들을 여러 개의 작은 번들로 쪼개주는 것을 의미한다. 코드 스플리팅을 함으로서, 필요한 번들만 필요할 때 로딩 시킴으로서 초기 로딩 시간을 줄일 수 있고, 애플리케이션의 성능도 향상 시킬 수 있다. 이런 방법을 코드 비동기 로딩이라고 한다.

#### 리액트에는 React.lazy 유틸함수와 Suspense 컴포넌트가 리액트에 내장되어 있다. 그러므로, 이것들을 이용해 리액트 애플리케이션에서 코드 스플리팅을 진행할 수 있다.

### React.lazy

```
// Split js file //
import React from 'react';

const Split = () => {
  return <div>this is Split</div>;
};

export default Split;

// another js file //
const Split = React.lazy(() => import("./Split)); // 미리 만들어 놓은 Split이라는 컴포넌트를 코드스플리팅 방식으로 불러온다.
```

#### React.lazy 유틸함수를 통해 컴포넌트를 렌더링하는 시점에 비동기적으로 로딩할 수 있다.

### Suspense

```
import React, { Suspense } from 'react';
<Suspense fallback={<div>loading...</div>}> // 로딩이 진행중일 때, <div>loading...</div>을 화면에 렌더링 해준다.
  <Split />
</Suspense>
```

#### Suspense는 리액트 내장 컴포넌트로서 코드 스플리팅된 컴포넌트를 로딩시켜주고, 로딩이 진행중일 때, 보여줄 UI 또한 설정할 수 있다. fallback props를 통해 로딩 중에 렌더링 시킬 것을 설정할 수 있다.

### 하지만, React.lazy와 Suspense는 아직 서버사이드 렌더링을 지원하지 않는다. 그러므로, 서버사이드 렌더링을 하는 애플리케이션에서는 loadable component라는 서드파티 라이브러리를 사용하면 된다.

### Loadable Component

#### 이번에 처음으로 Loadabl component 라이브러리를 사용하여 코드 스플리팅을 진행해보았다. 이 라이브러리는 위에서 설명한 React.lazy와 suspense와는 다르게 서버사이드 렌더링을 지원하지만, 아직까지 서버사이드 렌더링을 진행해본적은 없다. 조만간 서버사이드 렌더링을 공부해보아야 겠다. 말이 나온김에 서버사이드 렌더링에 대해서 간단하게 정리하고 가보자.

### Serverside Rendering

#### 서버 사이드 렌더링이란 웹 서비스의 초기 로딩 속도 개선, 캐싱 및 검색 엔진 최적화를 가능하게 해 주는 기술이다. 서버 사이드 렌더링을 사용하면 웹 서비스의 초기 렌더링을 사용자 브라우저가 아닌 서버 쪽에서 처리한다. 사용자는 서버에서 렌더링한 html결과물을 받아 와서 그대로 사용하기 때문에 초기 로딩 속도가 개선되고, 검색 엔진에서 크롤링할 때도 문제없다.

#### Loadable Component의 사용법은 아래와 같다.

```
// 설치 방법
npm install @loadable/component
```

```
// 사용 방법
import React, { useState } from 'react';
import loadable from '@loadable/component';

const SplitMe = loadable(() => import('./SplitMe'));

function App() {
  const [visible, setVisible] = useState(false);
  const onClick = () => {
    setVisible(true);
  };
  return (
    <div>
        <p onClick={onClick}>Hello React!</p>
          {visible && <SplitMe />}
    </div>
  );
}

export default App;
```

#### 위에서 설명한 fallback props와 마찬가지로 로딩 중에 보여주고 싶은 UI가 있다면, 아래와 같이 옵션을 설정해주면 된다.

```
const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>
});
```

### 아직까지 서버사이드 렌더링을 해본 적은 없지만, 개인적으로 React.lazy 또는 Suspense를 사용하는 것보단 loadable component를 사용하는 것이 더 편한 것 같다.
