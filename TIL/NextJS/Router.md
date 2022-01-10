# Next에서 Router 사용법 // 2022년 1월 10일(Mon)

## Next에서는 기존 React에서 사용하는 방식과는 다르게 Next 자체의 Router를 사용한다.

---

### create-next-app으로 next를 설치한 뒤 폴더를 확인해보면, pages 폴더를 발견할 수 있다. Next에서는 pages 폴더를 사용하여 페이지를 나눈다.

- 로그인 페이지를 만들기 위해서는 pages 폴더 안에 login.js 파일을 만들어 주면 된다.
- 홈 화면 페이지는 기존에 만들어져 있는 index.js를 의미한다.

#### login, index 파일에서 만든 컴포넌트들은 각각 해당 페이지로 라우팅 되었을때 렌더링 된다. 이 점이 리액트에서의 라우팅과는 많이 달라서 신기한다.

### 다이내믹 라우팅

- 다이내믹 라우팅을 하기 위해서는 pages 폴더 안에서 파일을 만들어 줄때, [id].js로 만들어 주면 된다. 만약 pages안의 about.js에서 [id].js를 사용하기 위해서는 about이라는 폴더를 생성하고 그 안에 [id].js를 생성하면 http://localhost:3000/about/{id} 로 다이내믹 라우팅이 가능하게 된다. id 값에는 어떠한 값이 와도 상관 없기 때문에 동적으로 값을 받아 라우팅을 할 수 있게 만들어 준다.

---

---

### Navbar를 사용하여 Routing

```
import Link from "next/link"; // 별도의 라이브러리 설치 필요 없이 next에 내장된 기능이다.

export default function Navbar() {
  return(
    <nav>
      <Link href="/login">
        <a>Login</a>
      </Link>
      <Link href="/">
        <a>Home</a>
      </Link>
    </nav>
  )
}
```

## CSS 적용하기

### 아래 코드와 같이 네비게이션 바의 CSS를 적용하기 위해 Link 컴포넌트에 className을 사용하면 될줄 알았지만, 작동을 하지 않았다.

```
import Link from "next/link"; // 별도의 라이브러리 설치 필요 없이 next에 내장된 기능이다.

export default function Navbar() {
  return(
    <nav>
      <Link className="login" href="/login">
        <a>Login</a>
      </Link>
      <Link className="home" href="/">
        <a>Home</a>
      </Link>
      <style jsx>{`
        .login {
          background-color: red;
        }
        .home {
          background-color: blue;
        }
      `}</style>
    </nav>
  )
}
```

### Link 컴포넌트에 style을 적용하기 위해서는 Link가 아닌 a태그의 style을 적용해줘야만 CSS가 정상적으로 작동한다.

```
import Link from "next/link"; // 별도의 라이브러리 설치 필요 없이 next에 내장된 기능이다.

export default function Navbar() {
  return(
    <nav>
      <Link href="/login">
        <a className="login">Login</a>
      </Link>
      <Link href="/">
        <a className="home">Home</a>
      </Link>
      <style jsx>{`
        .login {
          background-color: red;
        }
        .home {
          background-color: blue;
        }
      `}</style>
    </nav>
  )
}
```

---
