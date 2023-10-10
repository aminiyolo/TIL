# useState

### Hook은 어디서 오는 걸까?

- react 코어

  - React -> ReactHooks -> ReactCurrentDispatcher
  - react 코어는 react element에 대한 정보만 알고 있음
  - react element가 VDOM에 올라가기 위해서는, fiber로 확장이 되어야 하는데 확장이 될 때 hook에 대한 정보를 포함하게 됨
  - 이러한 확장을 reconciler가 함. reconciler가 hook에 대한 정보를 알고 있는 것으로 추측.
  - react 코어는 hook을 사용하기 위해서 외부에서 주입 받음(여러곳에서 사용하기 위해서)

    - React -> ReactHooks -> ReactCurrentDispatcher -> ReactSharedInternals
    - react/ReactSharedInternals.js는 객체에 property로 외부 모듈을 할당 받음
    - shared는 모든 패키지가 공유하는 공통 패키지
      - shared/ReactSharedInternals.js는 react 코어 패키지에 연결된 출입구

  - reconciler는 shared에 hook을 주입(객체에 property로 할당)
    - react
      - React -> ReactHooks -> ReactCurrentDispatcher -> ReactSharedInternals
    - shared
      - ReactSharedInternals
    - reconciler

  #### hook(useState)의 출처를 한줄로 정리하면, reconciler -> shared/ReactSharedInternal -> react/ReactSharedInternal -> react/ReactCurrentDispatcher -> react/ReactHooks -> react -> 개발자
