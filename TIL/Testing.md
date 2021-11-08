## Testing // 2021년 11월 8일 (월)

### 테스팅의 이유 -> 더 안정적인 어플리케이션을 만들 수 있다.

### 테스팅으로 얻는 이점

#### 1. 디버깅 시간을 단축, 만약 데이터가 잘못 나왔다면 그것이 UI의 문제인지 DB의 문제인지등 전부 테스트를 해봐서 찾아야 하는데 테스팅 환경이 구축 되어있다면 자동화 된 유닛 테스팅으로 특정 버그를 쉽게 찾아 낼 수 있다.

#### 2. 더욱 안정적인 어플리케이션, 많은 테스트 코드와 함께 작성된 코드의 어플리케이션이 되기 때문에 훨씬 안정적인 어플리케이션이 된다.

### React Testing Library

#### CRA로 리액트 앱을 생성하면 React-testing-library가 설치 되어 있다. 이것은 React 구성 요소 작업을 위한 API를 추가하여 DOM Testing Library 위에 구축된다. DOM Testing Library란 Dom 노드(Node)를 테스트하기 위한 매우 가벼운 솔루션이다.

#### Create React App으로 생성된 프로젝트는 즉시 React Testing Library를 지원한다. 그렇지 않은 경우 다음과 같이 npm을 통해 추가할 수 있다.

```
npm i --save-dev @testing-library/react
```

### Jest란 ?

#### FaceBook에 의해서 만들어진 테스팅 프레임 워크이다. 최소한의 설정으로 동작하며 Test Case 를 만들어서 어플리케이션 코드가 잘 돌아가는지 확인해준다. 단위 (Unit) 테스트를 위해서 이용한다.

```
npm i jest --save-dev // Jest 라이브러리 설치방법
```

#### Jest가 Test 파일을 찾는 방법에는 3가지가 있다. 1. {filename}.test.js / 2. {filename}.spec.js / 3. tests.folder

### Jest 간단 사용법

```
test('two plus two is four', () => { //앞에 테스트에 관한 설명을 적어준다.
  expect(2 + 2).toBe(4); // expect 함수는 값을 테스트 할때 마다 사용된다. 혼자서 사용되지 않고 matcher와 함께 사용되는데 여기서
  // toBe가 matcher이다. matcher에는 여러가지 함수가 존재한다.
})

// 특정 노드의 기능을 테스팅하는 경우
import { render, screen } from "@testing-library/react";

test('체크박스와 확인버튼 초기 값 세팅 확인', () => {
  render(<Home />) // 테스팅할 컴포넌트를 렌더링 해준다.
  const checkbox = screen.getByRole("checkbox");
  expect(checkbox.checked).toEqual(false); // 체크박스의 checkd가 되지 않았다는 것을 확인

  const confirmButton = screen.getByRole("button");
  expect(confirmButton.disabled).toBeTruthy(); // 확인 버튼의 disabled가 true인지를 확인

})
```

### TDD(Test Driven Development)

#### TDD란 Test Driven Development의 약자로 '테스트 주도 개발'이라고 한다. 반복 테스트를 이용한 소프트웨어 방법론으로, 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현한다.

### TDD 개발방식과 개발주기

#### TDD와 일반적인 개발 방식의 가장 큰 차이점은 테스트 코드를 작성한 뒤에 실제 코드를 작성한다는 점이다.

#### Red 단계 -> 실패하는 테스트 코드를 먼저 작성한다.

#### Green 단계 -> 테스트 코드를 성공시키기 위한 실제 코드를 작성한다.

#### Yello 단계 -> 중복 코드 제거, 일반화 등의 리팩토링을 수행한다.
