## Socket io 와 Websocket // 2021년 8월 4일 (수)

### 개인 프로젝트를 진행하던 도중 실시간 채팅기능을 구현해야 할 필요가 있었다. 처음 시도 하던것이다 보니 감이 오질 않아 구글링을 해보았고, 실시간 채팅을 구현하기 위해서는 Websocket 또는 socket.io를 사용해야 한다는 것을 알았고, 이 두 가지 중 socket.io를 사용하여 실시간 채팅 기능을 구현해야 한다는 것을 알았다. 이 두 가지에 대해 간략하게 정리해보려고 한다.

#### Websocket 과 socket.io 모두 웹 브라우저에서 양방향 통신을 가능하게 해준다. 우선 웹소켓과 소켓아이오를 먼저 정리하기 전에 polling, long polling, streaming에 대해서 먼저 알고가야 한다. 기존 웹페이지에서 사용하는 http 프로토콜은 클라이언트에서 요청을 보내야만 그에 대한 응답을 받을 수 있다. 하지만 http 프로토콜로 통신하는 경우 연결이 계속 유지되지 않기 때문에, 서버에서 먼저 요청을 보내는 것이 불가능하다. 그렇기 때문에 위에서 언급한 polling, long polling, streaming을 사용하여 비슷한 효과를 구현하고 있다.

##### Polling은 클라이언트에서 일정 주기마다 요청을 보내고, 서버는 현재 상태를 바로 응답하는 방식이다. 이러한 방식은 실시간으로 데이터를 주고 받아 반영을 해야하는 경우에는 효과적이지 않고, 서버의 변화가 없더라도 매 요청마다 응답을 하기 때문에 불필요한 트래픽이 발생한다.

##### Long Polling은 클라이언트에서 요청을 보내고 서버에서는 이벤트가 발생했을 경우, 응답을 주고 클라이언트가 응답을 받았을 경우 다음 응답을 기다리는 요청을 보내는 방식이다. Polling에 비해서는 불필요한 트래픽이 발생하지 않고 실시간 반응이 가능하지만, 이벤트가 자주 발생한다면 순간적으로 과부하가 걸리게 된다.

##### Streaming은 이벤트가 발생했을 때 응답을 주지만 응답을 완료시키지 않고 계속 연결을 유지하는 방식이다. Long Polling에 비해 효율적이지만, 연결 시간이 길어질수록 연결의 유효성 관리의 부담이 발생한다.

### 이제 웹소켓을 알아보자.

#### 웹소켓이란 웹 서버와 웹 브라우저간 실시간 양방향 통신환경을 제공해주는 실시간 통신 기술이다. 요청-응답 방식인 Polling과는 다르게 양방향으로 원할 때 요청을 보낼 수 있고 http에 비해 오버헤드가 적으므로 유용하게 사용할 수 있다. 웹소켓을 사용하여 메세지를 보낼 때는 메세지가 문자열로 전송이 되기 때문에 객체인 경우 stringify(서버) -> parse(클라이언트)를 해야 한다.

### Socket.io

#### socket.io는 WebSocket과 같이 클라이언트와 서버의 양방향 통신을 가능하게 해주는 모듈이다. socket.io는 통신을 시작할 때, 각 브라우저에 대해서 websocket, polling, streaming, flash socket 등에서 가장 적절한 방법을 사용하여 메세지를 전달한다. 그러므로, socket.io를 이용한다면 websocket이 지원 되지 않는 브라우저에서도 메세지를 주고 받을 수 있다.

##### 간단한 사용법

```
const http = require("http");
const SocketIO = require("socket.io");
const server = http.createServer(app);
const io = SocketIO(server, {
  cors: {
    origin: "*",
    methods: ["GET", "POST"],
    allowedHeaders: ["*"],
    credentials: true,
  },
});

io.on("connection", (socket) => {
  socket.on("msg", (msg) => {
    io.emit("msg", msg);
  };
});

server.listen(port, () => {
  console.log(`Server Listening on 3050`);
});
```

##### client에서는 socket.io-client를 사용하여 서버와 연동할 수 있다.

```
import React, { useEffect, useState } from "react";
import socketIOClient from "socket.io-client";

const Chat = () => {
  const [currentSocket, setCurrentSocket] = useState();
  const [value, setValue] = useState("");

  const onChange = (e) => {
    setValue(e.target.value);
  };

  useEffect(() => {
    setCurrentSocket(socketIOClient("localhost:3050"));
  }, []);

  const onSubmit = (e) => {
    e.preventDefault();
    currentSocket.emit("msg", { payload: value });
  };

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input onChange={onChange} />
      </form>
    </div>
  );
};

export default Chat;
```
