# setState의 state update 과정

### dispatchAction 함수

- update 객체 생성

  - expiration Time => Work가 진행될 때 할당 받는 값
  - action (setState()의 인자) => 우리가 호출하는 setState의 넣어주는 인자로 ex) setState(true) -> true = action
  - next: null => update 객체가 저장될때 어떤 형태로 저장되는지와 관련. 다음 노드의 레퍼런스 값을 저장
  - eagerReducer: null => 불필요한 렌더링이 발생하지 않도록 도와줌
  - eagerState: null => 불필요한 렌더링이 발생하지 않도록 도와줌

- update 객체를 queue에 저장(circular linked list 구조)
- 불필요한 렌더링이 발생하지 않도록 최적화
  - 현재 컴포넌트의 업데이트로 인해 발생하는 Work 스케쥴링이 없고,
  - action의 결과값(우리가 setState의 인자로 넣어준)과 현재 상태 값이 같다면,
  - 위의 2가지 조건이 만족하다면 함수를 return 하여 실행 중지
- 조건을 만족하지 않아 실행 중지가 되지 않는다면, update를 적용하기 위해 Work를 scheduler에 예약 -> scheduleWork(fiber, expirationTime)
