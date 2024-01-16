# Nextjs에서 localStorage를 사용하는 경우 이슈 발생

### Nextjs에서 localStorage를 사용하면, 'ReferenceError: localStorage is not defined' 에러가 발생한다. 이러한 에러가 발생하는 이유는 다음과 같다.

- next는 client-side를 렌더링 하기 전에 server-side 렌더를 수행하는데, next에서 제공하는 SSR에서는 window, document와 같은 브라우저 전역 객체를 사용할 수 없다.
- 그러므로, 페이지가 client에 로드가 되고 window 객체가 정의될때 까지는 localStorage에 접근할 수 없다.

### 이를 해결하기 위한 방법은 2가지가 있다.

- useEffect 사용

  ```
  useEffect(() => {
    const value = localStorage.getItem('KEY');
  }, [])
  ```

  - useEffect는 렌더링시 실행되므로, 초기 서버에서 빌드시에는 useEffect 내부 코드를 실행하지 않는다.
  - useEffect는 clinet-side에서만 실행되므로, 안전하게 localStorage에 접근이 가능하다.

- typeof window !== 'undefined' 사용
  ```
  if (typeof window !== 'undefined') {
  const values = localStorage.getItem('KEY');
  }
  ```
  - window 객체가 참조되지 않을 경우, window의 타입은 undefined이다.
  - window의 타입이 undefined의 경우 아직 페이지가 client에 마운트 되지 않았다는 것이기 때문에 localStorage의 접근을 막아 에러를 방지할 수 없다.
  -
