# renderWithHooks

### reconciler의 renderWithHooks 함수에서는 무슨 일이 일어날까?

- 리액트의 renderWithHooks 함수 코드를 보면, ReactCurrentDispatcher.current에 전역 변수로 선언한 nextCurrentHook이 값이 null이냐 아니냐에 따라 값이 다르게 할당된다.
- nextCurrentHook -> null이면, mount로 HooksDispatcherOnMount 할당 (첫 마운트로 어떠한 state도 가지고 있지 않다.)
- nextCurrentHook -> null이 아니면, update로 HooksDispatcherOnUpdate 할당 (업데이트로 기존의 어떠한 state 값을 가지고 있다.)
- renderWithHooks() -> hook과 함께 render 즉, hook을 주입하는 역할
  - 컴포넌트 상태가 mount, update일때 다음 컴포넌트가 사용할 수 있게 전역변수 초기화 로직도 포함(nextCurrentHook, firstWorkInProgressHook 등은 전역변수 즉, 작업중인 컴포넌트에만 접근 가능한 값. hook주입과 렌더링이 끝나면 값을 null로 초기화하여 다음 컴포넌트가 활용할 수 있도록 준비하는 로직)
  - nextCurrentHook, firstWorkInProgressHook 등을 전역변수로 사용하는 이유는 renderWithHooks 함수를 호출하는 내부에 다른 함수를 호출하는데 해당 함수에서도 해당 변수를 사용 해야하기 때문이다.

### renderWithHooks() -> Component(fiber)와 Hook 연결

- currentlyRenderingFiber 전역변수에 workInProgress 할당
- nextCurrentHook 변수가 null이면, null 할당 그렇지 않으면, current.memoizedState 할당
- current는 VDOM 중에서 DOM에 mount된 정보를 가진 fiber (작업중인 fiber = workInProgress)
- current.memoizedState에는 hook이 들어가 있다.(fiber의 memoizedState -> hook 할당)
  .

### useState()시 얻는 [state, setState] 증에서 state에 대해 구현한 코드 살펴보기

- state는 hook 객체 안에 memoizedState라는 키값에 할당되어 있다.

### hook 객체

- hook.memoizedState에 마지막에 얻은 state 값이 저장되어 있다.
- hook.next라는 값이 있는데, 이 값은 다음 hook을 가리키는 pointer이다. (hook은 linked list에 저장)
- hook.queue hook을 호출할때 마다 update 객체를 linked list로 구현한 queue에 저장
- hook은 배열이라기 보다는 실제 코드에서는 linked-list로 구현되어 있다.
