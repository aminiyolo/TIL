# useInfiniteQuery // 2023년 7월 11일 (Tue)

### useInfiniteQuery는 파라미터 값만 변경하여 동일한 useQuery를 무한정 호출할 때 사용된다. 사용 형태도 useQuery와 동일하다.

```
const response = useInfiniteQuery(queryKey, queryFn, options);
```

#### useInfiniteQuery의 queryFn에는 기존 useQuery와는 다르게, pageParam이라는 파라미터 값을 전달 받는다. pageParam의 기본 값(맨 처음 페이지 값)을 설정 해주어야 한다. 값을 넣어주지 않는다면 값은 undefined이다. 이러한 pageParam은 API를 호출한 뒤 getNextPageParam 옵션 설정을 통해 값을 변화 시킬 수 있다.

```
const getData = async ({pageParam = 1}) => {
  const { data } = await api.get(`api/test?page=${pageParam}&size=5`);
  return {
    data: data.result,
    isLast: data.isLast
  }
}

function useGetInifiniteData(params) {
  return useInfiniteQuery(['useGetInifiniteData', params], getData, {
    getNextPageParam: (lastPage, pages) => {
      // lastPage는 직전에 반환된 리턴 값, pages는 지금까지 받아온 전체 페이지 데이터
      // 마지막 페이지라면, undefined가 리턴이 되어 useInfiniteQuery의 리턴 값인 hasNextPage가 false 값이 된다.
      lastPage.isLast ? pages.length + 1 : undefined;
    },
    suspense: true,
    useErrorBoundary: true
  })
}

// hasNextPage 값을 가지고 가져올 페이지가 더 있다면 데이터를 요청하고 그렇지 않다면 요청하지 않는다.
const { data, fetchNextPage, hasNextPage } = useGetInifiniteData(params);

// infinite scroll 기능과 함께 사용
const intersecting = useInfiniteScroll(loadMore);

useEffect(() => {
  if(intersecting && hasNextPage) {
    // fetchNextPage 함수를 호출하여 다음 페이지 데이터 호출
    fetchNextPage();
  }
}, [intersecting])

// 생략...

<div ref={loadMore}>Load More...</div>

```

#### 만약, 페이지를 역순으로 보여주고 싶다면, select 옵션을 사용하여, 해당 데이터들을 reverse()를 사용하여 뒤집으면 된다.

```
// (...생략)
getNextPageParam: (lastPage, pages) => {
      lastPage.isLast ? pages.length + 1 : undefined;
},
select: (data) => {
  return [...data.pages].reverse();
}

```
