# useRef // 2021년 8월 21일(Sat)

## React hooks 중에서 useState 또는 useEffect는 항상 사용 되지만, 때때로 useRef를 사용해야 할 때가 있다. 그럴때 마다 사용 방법이 헷갈리곤 한다. useRef를 왜 사용하고 언제 사용하는지에 대해 정리해보자.

### javaScript를 사용할 때 특정 DOM을 선택하기 위해서는 querySelector, getElementById와 같은 DOM Selector 함수를 사용한다.

### 리액트를 사용하는 경우 어떻게 DOM을 선택할 수 있을까?

#### 리액트로 프로젝트를 만들다보면 스크롤바를 조작하거나 특정 이벤트 발생 시 포커싱을 준다거나 다양한 상황에서 우리는 특정 DOM을 선택해야 하는 상황이 발생한다. 이러한 경우 리액트에서는 ref를 사용해야 한다.

#### 함수형 컴포넌트에서는 ref를 사용하기 위해 useRef() 라는 Hook을 사용한다. 특정 이벤트 발생 시 input에 포커스가 잡힐 수 있도록 useRef를 사용하여 예시를 만들어보자.

```
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: ''
  });

  const nameInput = useRef(); // Ref 객체 만들기

  const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

  const onChange = e => {
    const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 비구조화 할당을 통해 추출
    setInputs({
      ...inputs, // 기존의 input 객체를 복사한 뒤 (spread operator syntax)
      [name]: value // name 키를 가진 값을 value 로 설정
    });
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: ''
    });
    nameInput.current.focus(); // inputs state 값들을 초기화 한 뒤 nameInput이 가리키는 DOM에 포커싱 주기
  };

  return (
    <div>
      <input
        name="name"
        placeholder="이름"
        onChange={onChange}
        value={name}
        ref={nameInput}
      />
      <input
        name="nickname"
        placeholder="닉네임"
        onChange={onChange}
        value={nickname}
      />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  );
}

export default InputSample;
```

#### const nameInput = useRef(); <- 이 코드처럼 useRef()를 사용하여 ref 객체를 만들고 이 ref 객체를 우리가 선택하고자 하는 DOM에 ref={nameInput} 처럼 ref 값으로 설정해주면 된다. 이렇게 ref 값을 설정해주고 나면 nameInput.current는 이름을 입력하는 input DOM을 가리키게 된다.

### useRef를 이용하여 컴포넌트 안의 변수 또한 관리할 수 있다.

#### useRef() 함수는 컴포넌트 안에서 조회 및 수정 가능한 변수를 관리할 수 있다. useRef를 사용하여 변수를 관리하게 된다면 그 변수가 업데이트 될지라도, 컴포넌트가 리렌더링 되지 않는다. 즉, 굳이 리렌더링이 필요 없는 변수라면 useRef를 사용하여 효율적으로 관리할 수 있다.

### useRef() 함수가 다른 React hooks에 비해 사용빈도가 떨어지는 이유는 아무래도 용도가 제한적이기 때문인것 같다. 하지만, 적재적소에 사용하면 좋은 hook이라고 생각한다. 나는 아직까지 특정 이벤트 발생 시 스크롤바를 움직이는 것에만 사용하고 있는 것 같다.
