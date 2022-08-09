# 회사 업무에서의 TIL // 2022년 8월 9일 (Tue)

## 코호트 디스커버리 카탈로그 작업 중 이따금씩 카탈로그의 길이가 너무 길어 툴팁 박스 사이즈를 넘는 경우가 발생했다. 나는 그것을 해결하고자 CSS font-size를 줄여 카탈로그의 라벨이 툴팁 박스를 넘지 않도록 만들었다. 하지만, 옆에서 지켜보던 고은이가 "라벨의 길이가 더 긴 것들이 생긴다면 그때마다 font-size를 줄일거야?" 라고 물어봤다. 그런 고은이의 물음에 내가 잘못 만들고 있다는 것을 꺠달았고, 이러한 경우에는 ReactDOM.createPortal를 사용하는 것이 좋다는 고은이의 도움에 처음으로 포탈을 사용해 보았고 그것을 간략하게 정리 해보고자 한다.

### ReactDOM.createPortal

- 리액트 공식 문서에 따르면, 포탈은 부모 컴포넌트의 DOM 계층 구조 바깥에 있는 DOM 노드로 자식을 렌더링하는 최고의 방법을 제공한다고 한다.

- 형식은 다음과 같다.

```
  ReactDOM.createPortal(children, container);
  // 첫번째 인자는 엘리먼트, 문자열 등 "렌더링"할 수 있는 React 자식이다.
  // 두번째 인자는 DOM 엘리먼트를 의미한다.
```

- 우리는 가끔씩 부모 컴포넌트의 하위 자식 요소가 아닌 부모 컴포넌트의 상위 또는 최상위 요소에 컴포넌트를 렌더링 해야 하는 경우가 종종 발생한다. 보통 이러한 경우는 Modal, Popup, Popover 기능을 구현해야 하는 상황인 것 같다.

- 나의 경우에는 최상위 요소인 body에 Popover를 붙이기 위해 고은이가 만들어놨던 컴포넌트를 참고하여 다음과 같이 작성했다.

```
  export function TooltipPopover(children) {
    const $body = document.querySelector("body");
    const [container, setContainer] = useState(null);

    useEffect(() => {
      const target = document.createElement("div");
      $body.appendChild(target);
      setContainer(target);

      return () => {
        $body.removeChild(target);
      }
    }, [])

    return(
      container ? ReactDOM.createPortal(children, container) : container;
    )
  }
```

- 포탈의 전형적인 유스케이스는 부모 컴포넌트의 overflow: hidden 또는 z-index가 있는 경우이지만, 시각적으로 자식 요소를 튀어나와야 하는 경우에도 해당한다고 한다. ex) dialog, hoverCard, tooltip
