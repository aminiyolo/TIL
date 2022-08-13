# TIL // 2022년 8월 13일 (SAT)

## 회사 프로젝트 중 드래그 앤 드랍을 구현하기 위해 'react-drag-drop-container'라는 라이브러리를 사용하고 있는데, 한 가지 문제점은 생성되는 drag container div 엘리먼트의 style을 inline-block으로 지정 한다는 것이다. 각각 한줄에 하나의 drag container를 만들어야 하는 상황이었기 때문에, 이를 해결하고자 useEffect를 사용하여 마운트가 된 이후에 그것들을 전부 display: block으로 변경 해주었다. 어떻게 보면, 기능상 또는 성능상 문제가 되는 부분은 없었지만 inline-block으로 생성된 container를 paint가 다 되고 난 후, 다시 block으로 paint를 해주는 것이 불 필요 했다. 그러던 와중, 종명이형이 useLayoutEffect라는 hook이 있다는 것을 알려 주었고 그것을 공부하게 되었다.

### useLayoutEffect

- 기본 형태는 다음과 같다.

```
  useLayoutEffect(() => {
    something function

    return () => {
      cleanup function
    }
  }, [])
```

- 보기와 같이, useEffect와의 형태가 동일하다. 공식 문서에 따르면, useEffect는 DOM의 레이아웃 배치와 페인트가 끝난 후 이펙트 함수를 호출한다. 그러므로, 상태값이 이펙트에 의존하는 경우 사용자 입장에서 화면의 깜박임을 보기 때문에 불편한 사용자 경험을 발생 시킬 수 있다.

- 이를 해결하고자, 나온 훅이 바로 useLayoutEffect이다. 이 훅의 이벤트는 브라우저가 화면에 DOM을 그리기 전에 이펙트를 수행한다. useEffect가 DOM을 그리고 나서 발생하는 것과는 다르게, DOM을 그리기 전에 이펙트가 발생하는 것이다. paint가 되기 전에 실행되므로, dom을 조작하는 코드가 있더라도 사용자는 깜박임을 경험하지 않는다.

```
// 기존 코드
  useEffect(() => {
    Array.from(document.querySelectorAll('.ddcontainer')).forEach((elem) => elem.setAttribute('style', 'display: block'));
  }, [])
```

```
// 수정 코드
    useLayoutEffect(() => {
    Array.from(document.querySelectorAll('.ddcontainer')).forEach((elem) => elem.setAttribute('style', 'display: block'));
  }, [])
```

### drag container가 tree 구조에서 가장 하위 뎁스에 존재하기 때문에 사용자 입장에서는 DOM 그려지고 난 뒤 style 변화가 일어나는 것을 눈으로 볼 수는 없어 체감상 느껴지는 변화는 없지만, 개발 하는 입장 에서는 inline-block으로 그려진 container를 DOM 그려지고 난 뒤 다시 block으로 바꾸기 보단, 처음부터 DOM을 그리기 전에, container를 block으로 바꾼 뒤 그리는 것이 불필요한 작업을 제거할 수 있기 때문에 좋은 방법이라고 생각한다.

### 결론 -> useEffect는 비동기적으로 실행, useLayoutEffect는 동기적으로 실행된다. 그렇기 때문에 useLayoutEffect는 내부의 코드가 모두 실행된 후 painting 작업을 거친다. 만약 이펙트 내부의 로직이 복잡한 경우에는 사용자가 레이아웃을 보는데 까지 시간이 오래 걸릴 수 있다는 단점이 있기 때문에 기본적으로는 useEffect를 사용하는 것을 권장 한다고 한다.
