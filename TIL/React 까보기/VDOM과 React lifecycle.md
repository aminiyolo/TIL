# VDOM과 React lifecycle

## VDOM

- VDOM의 프로그래밍 컨셉은 메모리 상에 UI 관련된 정보를 띄우고, react-dom과 같은 라이브러리를 통해 실제 DOM과 sync를 맞춘다. (Renderer 관여)
- 이 과정을 재조정(reconciliation)이라고 부름 (reconciler) 관여
- 가상으로 하는 이유는 실제로 mount -> paint 과정의 비용이 가상보다 더 크기 때문이다.

### VDOM -> fiber node로 구성된 tree 형태 (더블 버퍼링 구조 -> 똑같은 구조를 두개를 가지고 있음)

- current
  - DOM에 mount된 fiber
- workInProgress

  - render phase에서 작업 중인 fiber
  - commit phase를 지나면서 current tree가 됨

- 구현 상세
  - workInProgress tree는 current tree에서 자기 복제하여 만들어짐(서로 alternate로 참조)
  - fiber는 첫번째 자식만을 child로 참조, 나머지 자식들은 서로 sibling으로 참조, 모든 자식은 부모를 return으로 참조

## React lifecycle

### render phase

- VDOM을 재조정 하는 단계
  - element 추가, 수정, 삭제 -> WORK를 scheduler에 등록 (WORK: reconciler가 컴포넌트의 변경을 DOM에 적용하기 위해 수행하는 일)
  - reconciler가 담당 (React 16부터 reconciler 설계가 stack -> fiber로 바뀌어 렌더링을 abort, stop, restart가 가능해짐) 즉, 렌더링 우선순위 변경 가능

### commit phase

- 재조정한 VDOM을 DOM에 적용 및 lifecycle을 실행하는 단계
  - 일관성을 위해 sync 실행. 즉, DOM 조작 일괄 처리 후, 리액트가 콜스택을 비워준 다음 브라우저가 Paint 시작
