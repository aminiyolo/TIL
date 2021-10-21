# Redux toolkit(Rtk) // 2021년 10월 21일(목)

## Redux toolkit (이하 RTK) 이유

### 리덕스의 문제로 언급되는 대표적인 3가지는 다음과 같다.

#### 리덕스 스토어 환경 설정은 너무 복잡하다.

#### 리덕스를 유용하게 사용하려면 많은 패키지를 추가해야 한다.

#### 리덕스는 보일러플레이트, 즉 어떤 일을 하기 위해 꼭 작성해야 하는 코드를 너무 많이 요구한다.

### 이러한 이슈를 해결하기 위해 툴킷이 등장하였고 공식 문서에 따르면 툴킷은 리덕스 로직을 작성하는 표준 방식이 되기 위한 의도로 만들어졌다고 한다. -> 기존 리덕스의 복잡함을 낮추고 사용성을 높이는 것이 목적이다.

## 몇 가지 주요 RTK의 API를 살펴보자.

### configureStore()

#### 스토어를 구성하는 함수로서, 리덕스 라이브러리의 표준 함수인 createStore를 추상화한 것이다. 더 좋은 개발환경을 위해 기존 리덕스의 번거로운 기본 설정 과정을 자동화해준다.

```
import { configureStore} from '@reduxjs/toolkit';
import rootReducer from "./reducer";
const store = configureStore({ reducer: rootReducer})
```

#### 위와 같이 코드를 작성하면 기본 미들웨어로 redux-thunk를 추가하고 개발 환경에서 리덕스 개발자 도구를 활성화해준다.

### createReducer()

#### 상태에 변화를 일으키는 리듀서 함수를 생성하는 유틸함수이다.

### createAction()

#### 기존 리덕스 라이브러리에서 액션을 정의하는 일반적인 접근 방법은 액션 타입 상수와 액션 생성자 함수를 분리하여 선언하는 것이었다. 하지만, RTK에서는 이 두개의 과정을 createAction 함수를 사용하여 하나로 결합하여 추상화한다.

### createSlice()

#### 리덕스 로직을 작성하는 표준 접근법은 createSlice를 사용하는 것에서 출발한다. RTK의 다른 API들인 createAction, createReducer 함수가 내부적으로 사용되며 createSlice에 선언된 슬라이스 이름을 따라서 리듀서와 그리고 그것에 상응하는 액션 생성자와 액션 타입을 자동으로 생성한다. 따라서 createSlice를 사용하면 createAction, createReducer를 따로 작성할 필요가 없다.

```
import { createSlice } from "@reduxjs/toolkit";

export const userSlice = createSlice({
  name: "user",
   initialState: {
     name: "john",
     email: "john@email.com",
   },
   reducers: {
     update: (state, action) => {
       state.name = action.payload.name;
       state.email = action.payload.email;
     },
     remove: (state) => {
       state = null;
     },
   },
 });

export const { update, remove } = userSlice.actions;
export default userSlice.reducer;
```

### createSelector

#### 리덕스 스토어 상태에서 데이터를 추출할 수 있도록 도와주는 유틸리티 함수이다. 리덕스의 useSelector 함수의 결점을 보완하기 위한 좋은 솔루션이다.
