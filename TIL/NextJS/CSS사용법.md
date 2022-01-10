# Next에서 CSS 사용법 // 2022년 1월 10일(Mon)

## Next에서의 CSS 사용하기

---

### Next를 사용하여 웹페이지를 만들 때, CSS를 적용하기 위한 2가지 방법이 있다.

1. styled-jsx

#### styled-jsx는 넥스트 프로젝트에 기본적으로 포함되어 있다. 이를 활용하기 위해서는 jsx 라는 값을 속성으로 갖는 style 태그를 컴포넌트 본문에 위치시킨 후, 적용할 CSS 스타일을 문자열로 작성하면 된다.

```
<>
  <div className="greeting">
    Hello, World!
  <div/>
  <style jsx>{`
    .greeting {
      font-color: tomato;
    }
  `}<style>
</>

// Hello, World 문구 색깔이 토마토 색깔로 바뀌게 된다. styled-jsx로 작성된 style은 해당 컴포넌트에서만 적용이 된다.
```

2. CSS module

#### Create-next-app으로 Next를 설치하고 나면 루트 폴더에 styles라는 폴더가 생기는데, 이 안에서 각각의 컴포넌트에 해당하는 CSS 모듈을 만들어서 사용하는 것이다.

- styled-jsx를 적용한 코드를 CSS Module를 사용해서 적용한다면 코드는 다음과 같다.

```
// ./styles/greeting.module.css

.greeting {
  font-color: tomato;
}
```

```
import styles from "./greeting.module.css";
...
<>
  <div className={styles.greeting}>
    Hello, World!
  <div/>
</>
// styles를 import 한 뒤 CSS 모듈 안에서 적용한 클래스명을 객체처럼 사용하여 적용한다.
// styled-jsx와 CSS module를 사용 했을때의 CSS 결과는 정확히 동일하다.
```

---

## 위 두가지의 방법 말고도 React에서 자주 사용하는 styled-components를 사용하여 CSS를 적용하는 것도 가능하다. 하지만 Next애서는 첫번째 방법인 styled jsx 방식을 더 권장한다고 한다. 그러므로, Next를 사용할 때는 1번 방식으로 CSS를 꾸며보자.
