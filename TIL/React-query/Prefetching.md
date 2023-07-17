# Prefetching // 2023년 7월 17일 (Mon)

```
const prefetchNextData = async (nextPage: number) => {
  const queryClient = useQueryClient();
  await queryClient.prefetchQuery(
    ["nextUserData", nextPage], () => getUserData(nextPage),
    {
      ...options
    }
  )
};

// page가 변할때 마다 데이터 prefetch
useEffect(() => {
  const nextPage = page + 1;
  if(nextPage < lastPage) {
    prefetchNextData(nextPage)
  }
}, [page])
```

- prefetch는 미리 데이터를 fetch 해오겠다는 의미이다.
- react-query에서는 queryClient.prefetchQuery를 통해서 prefetch 기능을 제공하는데 해당 기능을 사용하여 사용자 경험을 상승 시킬 수 있다.
- 비동기 요청은 데이터 양이 클수록 응답 속도가 느려 소요시간이 길다. 그러므로, prefetch 기능을 이용하여 미리 데이터를 캐싱해 놓는다면 사용자는 기다림 없이 바로 캐싱된 데이터를 확인할 수 있기에 좋은 경험을 느낄 수 있다.
- 예를 들어, 1페이지에 진입 했을 때 2페이지의 데이터를 미리 받아 놓구, 2페이지를 진입 했을 때 3페이지 데이터를 미리 받아놓는 것이다.
- prefetchQuery를 통해 가져오는 쿼리에 대한 데이터가 이미 캐싱된 채로 데이터가 있다면 새로운 데이터를 가져오지 않는다.
