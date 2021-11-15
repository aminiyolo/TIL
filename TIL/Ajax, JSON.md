# Ajax, JSON // 11월 14일 (Sun)

## Ajax

### Ajax란 Asynchronous javascript and XML으로, 자바스크립를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

#### Ajax 사용 이전에는 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생했다. 또한, 변경할 필요가 없는 부분까지 처음부터 다시 렌더링을 함으로 화면 전환으로 인한 화면 깜박임 현상이 발생했다.

#### 하지만, Ajax의 사용으로 인하여 변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않고, 화면이 순간적으로 깜박이는 현상도 막을수 있게 되었다. 또한, 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## JSON

### JSON은 Javascript Object Notation으로 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그레밍 언어에서 사용할 수 있다.

### JSON 표기방식

```
{
  "lastname":"park",
  "age":"20,
  "alive":true,
  "firstname":"aminiyolo"
}
// JSON의 키는 반드시 큰 따옴표로 묶어야 한다.
```

### JSON.stringify

#### JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화라 한다.

```
const obj = {
  lastname:"park",
  age:"20,
  alive:true,
  firstname:"aminiyolo"
}

const json = JSON.stringify(obj);
console.log(typeof json, json); // string {"lastname":"park","age":"20,"alive":true,"firstname":"aminiyolo"}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기를 하기 위해서는 아래와 같이 작성해야 한다.
const prettyJson = JSON.stringify(obj, null, 2);


```

### JSON.parse

#### JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화라 한다. 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

```
const obj = {
  lastname:"park",
  age:"20,
  alive:true,
  firstname:"aminiyolo"
}

const json = JSON.stringify(obj);
console.log(typeof json, json); // string {"lastname":"park","age":"20,"alive":true,"firstname":"aminiyolo"}

const parsed = JSON.parse(json);
console.log(typeof parsed, parsed) // object {lastname:"park",age:"20,alive:true,firstname:"aminiyolo"}
```
