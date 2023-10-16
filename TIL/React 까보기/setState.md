# setState의 state update 과정

### dispatchAction 함수

- update 객체 생성

  - expiration Time => Work가 진행될 때 할당 받는 값
  - action (setState()의 인자) => 우리가 호출하는 setState의 넣어주는 인자로 ex) setState(true) -> true = action
  - next: null => update 객체가 저장될때 어떤 형태로 저장되는지와 관련. 다음 노드의 레퍼런스 값을 저장
  - eagerReducer: null => 불필요한 렌더링이 발생하지 않도록 도와줌
  - eagerState: null => 불필요한 렌더링이 발생하지 않도록 도와줌

```
const last = queue.last;
if(last === null) {
  // this is the first update
  update.next = update;
} else {
  const first = last.next;
  if(first !== null) {
    // still circular
    update.next = first;
  }
  last.next = update;
}
queue.last = update;
```

- update 객체를 queue에 저장(circular linked list 구조)
- 불필요한 렌더링이 발생하지 않도록 최적화
  - 현재 컴포넌트의 업데이트로 인 해 발생하는 Work 스케쥴링이 없고,
  - action의 결과값(우리가 setState의 인자로 넣어준)과 현재 상태 값이 같다면,
  - 위의 2가지 조건이 만족하다면 함수를 return 하여 실행 중지
- 조건을 만족하지 않아 실행 중지가 되지 않는다면, update를 적용하기 위해 Work를 scheduler에 예약 -> scheduleWork(fiber, expirationTime)

### setState를 호출하는 2가지 상황 idle과 render phase

- idle update vs render phase update을 구분하는 조건문

  - render phase 진입점: idle 상태에서 update를 하면 scheduler에게 Work 전달
  - idle과 render phase 구분은 fiber와 currentlyRenderingFiber와 같은지를 비교한다.

    #### currentlyRenderingFiber

    - currentlyRenderingFiber는 workInProgress의 또 다른 이름으로 타입은 Fiber | null이다.
    - workInProgressHook에서 구분하기 위해 또 다른 이름으로 변수를 추가 생성하였다고 한다.
    - idle과 render phase를 구분하기 위해 fiber와 currentlyRen deringFiber를 비교하는데 두개의 값이 동일 하고 Null이 아니면, renderWithHooks()가 호출되었다는 의미로 현재 renderPhase라는 것을 알 수 있다.

    - currentlyRenderingFiber 해당 코드를 살펴보면, fiber와 alternate(current와 workInProgress)의 두개 모두를 비교하는데, 그러한 이유는 fiber가 current와 workInProgress 둘다를 가리키기 때문이다.
    - alternate는 fiber.alternate로 여기에는 current와 workInProgress가 서로를 참조하는 주소값을 할당한다.
    - 그런데, workInProgress(fiber)는 commit phase를 지나면서 current(fiber)로 바뀌고 current(fiber)를 참조 복사 해서 새로운 workInProgress(alternate)가 만들어짐. 즉, 현재 작업 중인 currentlyRenderingFiber가 fiber인지 alternate인지 둘다 확인해야 알 수 있음. 해당 코드는 아래와 같다.

    ```
    fiber === currentlyRenderingFiber || (alternate !== null && alternate === currentlyRenderingFiber)
    ```

  ### render phase update가 실제 리액트 코드에서 일어나는 과정

  ```
  function Test() {
    const [num, setNum] = useState(0);
    if(num === 1) setNum(prev => prev + 1);
    return <button onClick={() => setNum(prev => prev + 1)}> +1</button>
  }
  ```

  - num은 0이고 button tag를 return
  - button을 클릭하면 num의 값은 0 -> 1
  - num이 1이므로 setNum(prev => prev + 1) 호출
  - button tag를 return하기 전에 다시 Test 컴포넌트를 리렌더링. 이와 같이 button tag를 return 전에 다시 리렌더링 하는 상황이 render phase update

### render phase에서 setState

- 이미 idle에서 WORK를 scheduler에게 등록 했으므로, 다시 WORK를 등록할 필요가 없고 최적화할 필요가 없다.
- render phase에서 setState를 발생시키면 -> render phase update가 추가적으로 발생하지 않을때 까지 컴포넌트를 재호출하고 setState의 인자로 받아온 action 값을 소비하면 됨

### render phase에서의 update 저장

- didScheduleRenderPhaseUpdate의 값 유무로 이전에 추가된 업데이트가 있는지 판별 (boolean type)
- 계속적으로 추가 업데이트가 발생하는데 업데이트를 소비하기 전에 새로운 업데이트가 발생하므로, 새로 발생한 업데이트를 소비하기 위해서는 이전 추가된 업데이트 목록을 임시 저장소에 보관을 해놔야 한다. 리액트 코드를 확인해보면, renderPhaseUpdates라는 맵 객체에 저장한다.
- linked list 형태로 renderPhaseUpdates를 저장한다 (새로운 업데이트가 추가될 때마다 renderPhaseUpdates.next 값에 가장 최근 업데이트를 연결)

### render phase에서 update 소비

- renderWithHooks()에서 renderPhaseUpdates에서 저장하고 있는 update를 소비한다.
- hook 업데이트 구현체에서 render phase update를 소비하기 위해 필요한 변수들의 값을 할당 -> hook 업데이트 구현체를 dispatcher에 주입. ReactCurrentDispatcher.current = HooksDispatcherOnUpdate

  #### update 소비 중에 다시 update 발생하면

  - 컴포넌트 재호출 -> 여기서 다시 update가 발생한다면, didScheduleRenderPhaseUpdate의 값이 true로 됨 (컴포넌트 호출이 완료되면 마지막에 didScheduleRenderPhaseUpdate의 값을 false로 바꿔줌) didScheduleRenderPhaseUpdate의 값이 true이므로 다시 컴포넌트 호출 과정 진행 -> 반복되면 numberOfReRenderes의 변수 값을 +1 씩 해줌 -> 이러한 리렌더는 최대 25회까지만 반복됨 (리액트 코드 내에 최대 리렌더 값을 25로 지정되어 있음)
    즉, re-render는 최대 25번까지 가능하다.
