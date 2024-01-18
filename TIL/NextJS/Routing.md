# Routing

### 폴더 이름을 따르는 URL Path

- Next.js의 파일 시스템 기반의 라우팅은 예를 들어, 다음과 같은 방식으로 형태를 이룬다. (ex. domain.com/dashboard/settings) App Routing의 경우 app이라는 폴더를 만들고 그 아래 dashboard, settings라는 폴더를 만들면 URL path가 폴더 이름과 동일한 순서를 따라 만들어진다.
- 다만, Next.js에서는 Route를 표현하는 폴더인 경우엔 라우트 세그먼트(Route Segment)라고 부른다. Root Route Segment 이름을 app이라고 지으면 App Router, pages라고 지으면 Pages Router이다.

### 파일 이름 (Next.js에서는 상황에 맞는 UI를 정의할 때 쓰는 파일명이 미리 정해져 있다.)

- layout: 세그먼트의 메인 컨텐츠와 하위 세그먼트의 공용 레이아웃 UI
- page: 세그먼트의 메인 컨텐츠 UI
- loading: 세그먼트의 메인 컨텐츠와 하위 세그먼트의 로딩 UI
- not-found: 세그먼트의 메인 컨텐츠와 하위 세그먼트의 Not Found UI
- error: 세그먼트의 메인 컨텐츠와 하위 세그먼트의 에러 UI
- global-error: 전역 에러 UI
- route: 서버 API 엔드포인트
- template: 특별하게 재사용될 수 있는 레이아웃 UI

### 동적 URL Path 만들기

- 블로그의 경우, 게시글마다 고유의 ID 값을 가지곤 한다. (ex. /post/:id) 이러한 경우를 위해 Next.js에서는 Dynamic Routes 기능을 제공한다.
- Dynamic Routes는 파일 이름을 대괄호로 묶어주는 걸 약속으로 한다. 대괄호 안에 지정한 이름을 Page 컴포넌트의 인자로 받는다.

```
(app/post/[id]/page.tsx)
export default function Page({ params }: { params: { id: string } }) {
  return <div>My Post: {params.id}</div>
}
```
