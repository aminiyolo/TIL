# Axios 2021년 10월 26일(화)

## axios를 이용하며 개발을 하던 도중 axios에 대해 아직 잘 모르는 부분들이 있고 몇몇 기능들을 활용하고 있지 못한다는 생각이 들기에 정리해본다.

### axios란? -> 공식문서에 의하면 브라우저, Node.js를 위한 Promise API를 활용하는 HTTP 비동기 통신 라이브러리이다.

#### 자바스크립트에 내장되어 있는 fetct API와 비슷하지만 몇가지 차이점들이 존재한다.

### axios와 fetch의 몇 가지 주요 차이점들을 비교해보면 다음과 같다.

```
axios -> 써드파티 라이브러리로 설치 필요, data 속성을 사용, 자동으로 JSON 데이터 형식으로 변환, 요청 취소 가능, 많은 브라우저에 지원됨

fetch -> 브라우저 빌트인이라 별도 설치 필요x, body 속성을 사용, JSON 데이터 형식으로 변환하기 위해 json() 메서드를 사용해야함, 요청 취소 불가, 특정 브라우저 버전 이상에서만 사용 가능
```

## 두 개의 차이점들을 비교해본 결과 axios가 조금 더 많은 기능들을 제공해주는 것 같다는 생각이 든다.

### axios는 자주 사용되는 요청에 대해 인스턴스를 생성하여 사용할 수 있다.

```
import axios from 'axios';

const BASE_URL = 'http://localhost:8080/api'

export const userRequest = axios.create({
  baseURL: BASE_URL,
  headers: {Authorization: 'Bearer TOKEN' },
  timeout: 1000, // 만약 타임아웃이 필요하다면 설정가능하다.
})
```

### 위 코드를 이용하여 서버에 요청을 보낼 때 async await을 사용하면 가독성이 높아진다.

```
import { userRequest } from 'apiCalls.js';

const getUser = async () => {
  try {
    const res = await userRequest.get('/users/:id') // baseURL을 지정했기 때문에 모든 주소를 적어줄 필요가 없다.
  } catch (err) {
    console.log(err);
  }
}

```

### axios 요청에 있어서 config 옵션을 설정할 수 있다. 주로 사용되는 get 요청에는 두번째 params 자리에 설정해줘야 하고 post 요청에는 세번재 params 자리에 값을 설정해야 config 옵션이 설정된다.

```
axios.get(URL, confingOption);
axios.post(URL, data ,confingOption);
```

### axios를 요청을 하고 데이터를 받게되는 응답스키마에는 다음과 같은 6가지 종류가 있다.

```
data: {} // 서버가 응답한 데이터이다.
status: 200 // 서버 응답의 http 상태 코드이다. 200번대, 400번대, 500번대가 주로 사용됨
statusText: ok // 서버 응답의 http 상태 메세지이다.
headers: {} // 서버가 응답한 스키마의 헤더로서 모든 헤더 이름이 소문자로 제공된다.
config: {} // 요청에 대해 axios에 설정된 config이다. 즉, 요청할 때 설정한 config 값이다.
request: {} // 응답을 생성한 요청이다.
```

## 느낀점: axios 인스턴스를 유용하게 사용한다면 자주 반복되는 코드를 줄임으로 가독성을 높일 수 있고 또한, 유지보수적 측면에서도 더 효과적이라고 생각된다.
