# Redirect vs history.push vs Link // 2021년 8월 12일(목)

### 리액트로 SPA을 만들다보면 특정 이벤트 발생 시 특정 페이지로 이동시켜 주어야 하는 일이 발생하는데, 이럴 경우 Redirect, history.push, Link 중 한가지를 사용하게 된다. 세 가지 모두 특정 페이지로 이동시켜준다는 공통점이 있으므로 각각의 차이점에는 어떠한 것이 있는지 알아보자.

### history.push

```
const Login = ({ history }) => (
  ....
  const onClick = () => {
    history.push("/");
  }
  ....
)
```

#### 위와 같은 방식으로 코드를 작성한다. 하지만, 자식 컴포넌트에서 history 객체를 사용하려면 props를 통해 부모 컴포넌트로부터 받아와야 한다. 또한 react-router-dom의 withRouter를 이용하여 컴포넌트를 감싸야 한다.

```
import { withRouter } from "react-router-dom";
  ....
export default withRouter(Login)

```

#### react-router-dom의 BrowserRouter 와 Route를 이용하여 라우팅된 페이지의 최상위 컴포넌트에는 자동으로 props를 통해 histort 객체가 전돨된다.

### Redirect

```
import { Redirect } from "react-router-dom";

<Redirect to="/" />

```

#### Redirect는 react-router-dom에서 import 하여 사용한다. Redirect는 작동법이 history.replace 유사하다. history 객체는 이동 전의 URL을 남기는데, history.replace를 사용하면 이동 후에도 남기지 않는다. Redirect도 이처럼 이동 전 경로를 남기지 않는다.

#### Redirect는 history와는 다르게 history 객체를 props로 넘겨 받아오지 않아도 되고 바로 자식 컴포넌트에서 다른 페이지로 이동시켜 줄수 있다.

### Link

```
import { Link } from "react-router-dom";

  <Link to="/" />

```

#### Link도 Redirect와 마찬가지로 react-router-dom에서 import 하여 사용한다. 내부적으로 <a></a> tag를 통해 작동되고 사용법도 비슷하지만, 실제 동작은 일반적인 <a></a> tag와 다르게, 페이지 전체를 리로드하지 않고 필요한 부분만 리로드하게 된다.

#### history.push처럼 이동 전 경로가 history 객체에 남는다.
