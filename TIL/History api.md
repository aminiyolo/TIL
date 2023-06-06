# History API // 2023년 6월 6일 (Tue)

### History API란 history 글로벌 객체를 이용해서, 브라우저의 세션 히스토리에 대한 접근과 조작 기능을 제공한다.

### 어떤 기능을 제공하는가 ? -

#### - history.length : 세션 히스토리에 들어있는 요소들의 개수 확인

#### - history.back(): 뒤로 이동

#### - history.forward(): 앞으로 이동

#### - history.go(number): 원하는 곳으로 이동 | 0을 넣으면 현재 페이지 새로고침, 음수를 넣으면 뒤로 양수를 넣으면 앞으로 이동

#### - history.pushState(state, unused, url)

#### - history.replaceState(state, unused, url)

#### state: 직렬화가 가능한 무언가, unused: 사용하진 않지만 무조건 있어야함 -> 빈 문자열 권장, url: 현재 url과 오리진(프로토콜, 호스트명, 포트번호)이 같은 절대 또는 상대경로, 주소창은 바뀌지만 실제 페이지 이동은 X

#### PopStateEvent - 사용자의 세션 기록 탐색으로 인해 같은 문서 내에서 히스토리 엔트리가 바뀔 때 마다 발생

### 이러한 History API를 이용해서 만들 수 있는 것이 대표적으로 Single Page Application이다.

#### 하나의 HTML

- 맨 처음에 모든 자원을 가져옴
- 서버와의 통신 최소화

#### 페이지 이동 시 바뀌는 부분만 렌더링

- 컴포넌트로 나누어 개발하기에 유리
- 좋은 사용자 경험
- 검색 엔진 최적화가 어려움

### URL Routing이란?

#### URL: Uniform Resource Locator, 자료가 어디에 존재하는지 나타내는 일종의 주소

#### Routing: 네트워크에서 경로를 고르는 과정, 자료에 어떻게 접근할 것인지를 정하는 것

#### URL Routing: URL을 특정 route로 매핑하는 과정

### SPA에서 URL Routing이 필요한 이유

- 실제 저장되어 있는 페이지는 하나
- 여러 페이지를 돌아다닌 것 같지만, 방문 기록이 없음
- 뒤로 가기, 앞으로 가기 사용 불가
- 본인이 보고 있는 페이지의 접근 방법을 알기 어려움
