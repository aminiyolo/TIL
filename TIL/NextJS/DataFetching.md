# DataFetching

- Next.js 13 이전 버전에서는, getStaticProps이나 getServerSideProps라는 메서드를 사용하면 Data Fetching 및 사전에 각 페이지의 HTML을 생성해 두는 Pre-rendering이 가능했다. 13 버전이 나온 후, Next.js에서 Data Fetching은 새롭게 변화하였다.
- 13 버전부터는 React 18 버전을 사용하기 시작 했는데, 18 이전 버전에서는 모든 컴포넌트를 클라이언트 컴포넌트로 처리했다. 하지만 18 버전을 도입하며 서버 컴포넌트와 클라이언트 컴포넌트로 구분하여, 서버 관련 로직을 내포하고 있는 컴포넌트는 서버 컴포넌트, 커스텀 훅이나 이벤트 리스너, 브라우저 API 등 클라이언트 로직을 사용할 수 있는 컴포넌트는 클라이언트 컴포넌트로 분리하였다.
- Next.js 13에서 새롭게 추가된 App 디렉터리에 있는 모든 컴포넌트의 디폴트값은 앞서 설명한 서버 컴포넌트로 세팅되어 있다. 즉, 서버에서 데이터를 불러오는 작업을 별도의 추가적인 세팅 없이 바로 할 수 있다는 것이다.
- 13 버전 부터는, Fetch API 옵션을 통해 13 이전 버전의 쓰이던 getServerSideProps, getStaticProps 메소드를 구현할 수 있다.

### SSR의 경우

```
const fetchPosts = async () => {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    cache: "no-store",
  });
  return response.json();
};

const Posts = async () => {
  const data = await fetchPosts();
  return data.map((item, index) => <div key={item.id}>{item?.title}</div>);
};

export default Posts;
```

- getServerSideProps와 유사하다.
- 기존의 getServerSideProps 같은 다소 복잡한 이름 대신 직관적으로 사용자가 원하는 함수(fetchPosts)를 정의하고 해당 함수로 Post데이터를 불러올 수 있다.
- 캐시 옵션의 "no-store” 옵션은 말 그대로 해당 쿼리에 대해서는 자동으로 캐싱 작업을 하지 않고, 매번 새로운 요청이 있을 때마다 새롭게 데이터를 불러오도록 설정해준다.

### SSG의 경우

```
const fetchPosts = async () => {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts"); // { cache: "force-cache" } default 값으로 생략이 가능하다.
  return response.json();
};

const Posts = async () => {
  const data = await fetchPosts();
  return data.map((item, index) => <div key={item.id}>{item?.title}</div>);
};

export default Posts;
```

- getStaticProps와 유사하다.

#### Server Component

- fetch data
- 백엔드 리소스에 접근 시
- 중요한 정보 서버에 보관 시(API key, access token ..)
- 클라이언트 측 JS 코드 줄여야 할 때

#### Client Component

- fetch data - SWR, React Query 등 라이브러리 사용시
- 이벤트 리스너 추가시(onClick, onChange ..)
- State, Lifecycle Effects 사용시(useState, useEffect, useReducer ..)
- 브라우저 Web API 사용시(localStorage, sessionStorage, Cookie)
- State, Effect, 브라우저 API에 의존하는 커스텀 Hook 사용시
- React Class Component 사용시
