# CORS // 2021년 8월 17일 (Tue)

## CORS란?

### 교차 출처 리소스 공유(Cross-Origin-Resource Sharing) => CORS는 추가 HTTP 헤더를 사용하여 한 출처에서 실행중인 웹 애플리케이션이 다른 출처의 선택한 자원의 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제이다. 웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때, 교차 출처 HTTP 요청을 실행한다.

#### 보안 상의 이유로, 브라우저는 스크립트에서 시작한 교차 출처 HTTP 요청을 제한한다. 예를 들어, XMLHttpRequest와 Fetch API는 동일 출처 정책을 따른다. 즉, 이 API를 사용하는 웹 애플리케이션은 자신의 출처와 동일한 리소스만 불러올 수 있으며, 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해야 한다.

#### 즉, CORS는 서로 다른 출처간에 리소스를 전달하는 방식을 제어하는 체제이며, CORS 요청이 가능하게 하려면 서버에서 특정 헤더인 Access-Control-Allow-Origin과 함께 응답할 필요가 있다.

### ex) 클라이언트 포트 3000번, 서버 포트 5000번인 경우, 클라이언트 측에서 서버로 리소스 요청을 하게 되면 서로 다른 출처를 가진 상태에서 서버에 리소스를 요청하게 되기 때문에 CORS 에러가 발생한다.

## CORS 해결방법에는 무엇이 있을까?

### CORS의 해결 방법은 여러가지가 있지만, 몇 가지만 작성해보자.

### 서버에서 CORS 미들웨어를 사용하기

#### 서버를 express로 구축하는 경우 Node.js의 CORS 미들웨어를 사용하게 쉽게 문제를 해결할 수 있다.

```
const express = require("express");
const app = express();
const cors = require("cors");

const corsOptions = {
  origin: "http://localhost:3000", // client의 포트번호가 3000번이기 때문에
  credentials: true,
 };

app.use(cors(corsOptions));
```

#### 위와 같이 코드를 작성한 후 origin에 자신이 허용하고자 하는 도메인을 작성해주고, credentials을 true로 설정해주면 response header에 추가가 되어진다. 만약 위처럼 corsOptions를 작성해주지 않은채로 아래와 같이 실행하게 되면 모든 출처에서 오는 요청을 허락하게 된다.

```
app.use(cors());
```

### http-proxy-middleware 사용

### 클라이언트측에서 쉽게 CORS를 해결할 수 있는 방법은 http-proxy-middleware을 사용하는 것이다.

#### http-proxy-middleware 라이브러리를 아래와 같이 사용하게 되면 쉽게 CORS 에러가 해결된다.

```
const { createProxyMiddleware } = require("http-proxy-middleware");

module.exports = function (app) {
  app.use(
    "/api",                            // 요청주소가 "/api"로 시작
    createProxyMiddleware({
      target: "http://localhost:3050", // 서버포트가 3050이기 때문에
      changeOrigin: true,
    })
  );
};

```

#### 로컬환경에서 "http://localhost:3000/api" (클라이언트 포트: 3000번을 의미)로 시작되는 요청을 "http-proxy-middleware" 라이브러리가 "http://localhost:3050/api" 이곳으로 프록싱 해준다.

### 서버에서 Access-Control-Allow-Origin 헤더 세팅하기

#### 서버에서 헤더를 세팅해주는 것이 가장 기본적인 CORS 해결방법이라고 한다. 아직까지 이러한 방식으로 CORS 문제를 해결본적은 없지만, 이번에 CORS에 대해 공부하며 알게된 코드 작성법은 아래와 같다.

```
const express = require("express");
const app = express();

app.get("/api", (req, res) => {
  res.header("Access-Control-Allow-Origin", "http://localhost:3000");
  // 굳이 포트번호가 3000번이 아닌 자신이 허용 하고자하는 도메인을 작성해주면 된다.
  res.send(data);
})

```
