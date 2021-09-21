# GraphQL // 2021년 9월21일 (화)

## GraphQL이란?

### graphql은 facebook이 만든 API 표준이다. Graph와 Query Language가 합쳐진 말로 Structed Query Language(sql)와 마찬가지로 쿼리 언어이다. sql은 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것이 목적이고, gql은 웹 클라이언트가 데이터를 서버로 부터 효율적으로 가져오는 것이 목적이다.

### Rest API와 비교해보기

#### Rest API는 URL, Method 등을 조합하기 때문에 다양한 Endpoint가 존재한다. 하지만, gql은 단 하나의 Endpoint가 존재한다. gql API에서는 불러오는 데이터 종류를 쿼리 조합을 통해서 결정한다. Rest API에서는 각 Endpoint마다 데이터베이스 SQL 쿼리가 달라지는 반면, gql API에서는 gql 스키마의 타입마다 데이터베이스 SQL 쿼리가 달라진다.

## GraphQL의 구조

### query와 mutation

#### 쿼리는 데이터를 읽는 것에 사용하고, 뮤테이션은 데이터를 변경하는데 사용한다.

### schema와 type

#### schema에서는 시용할 데이터들에 대한 설명을 명시한다. ex) query, mutation, type

#### object 타입과 필드

```
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

#### 캐릭터 타입안에서 각각이 의미하는 바는 다음과 같다.

#### name, appearsIn : 필드 명

#### String, Episodde : 타입 명

#### !(느낌표) : 필수 값을 의미하며 [](대괄호)는 배열을 의미한다.

### Resolver

#### 데이터를 생성하고, 업데이트하고 삭제하는 등의 기능 구현 역할을 한다.

### Resolver 예시

```
import { getMovies, getMovie, getSuggestions } from "./db";

// Query의 프로퍼티에 들어오는 인수들은 순서대로 parent, args, context가 들어온다.
// parent: parent 객체, 거의 사용안함
// args: Query에 필요한 필드에 제공되는 인수
// context: 로그인한 사용자, DB access 등의 중요한 정보들

const resolvers = {
  Query: {
    movies: (_, { rating, limit }) => getMovies(limit, rating),
    movie: (_, { id }) => getMovie(id),
    suggestions: (_, { id }) => getSuggestions(id)
  }
};

export default resolvers;
```
