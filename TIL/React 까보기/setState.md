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
  - 현재 컴포넌트의 업데이트로 인해 발생하는 Work 스케쥴링이 없고,
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
