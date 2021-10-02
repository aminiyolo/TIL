# 2021년 10월2일 (토)

## URI(Uniform Resource Identifier)로 locator와 name으로 분류될 수 있다.

### URL(Uniform Resource Locator) 리소스의 위치를 의미한다.

### URN(Uniform Resource Name) 리소스의 이름을 의미한다.

#### URI의 uniform은 리소스를 식별하는 통일된 방식을 의미

#### resource는 자원, URI로 식별할 수 있는 모든 것을 의미

#### identifier는 다른 항목과 구분하는데 필요한 정보를 의미

### URL의 전체 문법

```
scheme://[userinfo@]host[:port][/path][?query][#fragment]
https://www.google.com:443/search?q=hello&hl=ko
```

#### scheme는 주로 프로토콜을 사용한다. 프로토콜은 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙이다. ex) http, https, ftp 등등

#### http는 80포트, https는 443포트를 주로 사용하며 생략가능하다. https는 http에 보안을 추가한 것이다.

#### userinfo는 URL에 사용자 정보를 포함해서 인증할 때 사용하는 것인데, 거의 사용하지 않는다.

#### path는 리소스 경로, 계층적 구조로 되어있다.

#### query는 key, value 형태로 ?로 시작, &로 추가가능하다. 쿼리 파라미터, 쿼리 스트링 등으로 불린다.

#### fragment는 html 내부 북마크 등에 사용되며, 서버에 전송하는 정보가 아니다.

## HTTP(HyperText Transfer Protocol)

### http 메세지에 모든 것을 담아 전송한다. ex) html, text, image, JSON 등 거의 모든 형태의 데이터 전송 가능. 서버간에 데이터를 주고 받을 때도 대부분 HTTP를 사용한다.

### HTTP의 특징

#### 클라이언트 서버 구조로 동작된다.

#### 무상태 프로토콜(stateless)을 지향한다. 비연결성 특징이 있다.

#### HTTP 메시지를 이용하여 통신한다.

#### HTTP는 단순하고 확장이 가능하다.
