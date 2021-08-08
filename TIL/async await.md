## async와 await 2021년 8월8일(일)

### async와 await는 자바스크립트의 비동기 처리 패턴 중 가장 최근에 나온 문법이다.

### async & await 기본 문법

```
async function 함수명() {
  await 비동기 처리 함수명();
}
```

### 먼저 함수의 앞에 async 라는 예약어를 붙인다. 그 뒤 함수 안에서 HTTP 통신을 하는 비동기 처리 코드 앞에 await을 붙인다. 주의 할 요소는 함수 내부의 비동기 처리 함수가 꼭 프로미스 객체를 반환해야 await이 의도한 대로 작동한다는 것이다.

#### 일반적으로, await이 붙는 비동기 처리 함수는 axios등 프로미스를 반환하는 API 호출 함수이다.

### 이러한 async와 await의 예외처리를 알아보자.

#### 예외를 처리하는 방법은 아래와 같이 try catch문을 사용하는 것이다.

```
async getMessages() => {
    try {
      // 아래와 같이 프로미스를 반환하는 비동기 처리 함수 앞에 await을 붙인다.
      const res = await axios.get("/api/messages");
      setMessages(res.data);
    } catch (err) { // try문 안에서 에러 발생 시 에러를 잡아낸다.
      console.log(err);
    }
  };
```

### async와 await을 이용하면 좀 더 깔끔하게 비동기 코드를 작성할 수 있다.
