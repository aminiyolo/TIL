# keepPreviousData // 2023년 7월 14일 (Fri)

```
const getUserData = async (pageNo: number) => {
  return await axios.get(
    `http://localhost:3000/users?page_size=5$page_no=${pageNo}`
  )
};

const { data, isPreviousData } = useQuery(["userData", pageNo], () => getUserData(pageNo), {
  keepPreviousData: true,
  useErrorBoundary: true,
});
```

- keepPreviousData를 true로 설정하면, 쿼리 키가 변경 되어서 새로운 데이터를 요청하는 동안에도 로더 대신 마지막 data 값을 유지하여 기존 값을 보여줄 수 있다.
- keepPreviousData를 이용하면, 페이지네이션 기능을 구현할 때 유용하다. 예를 들어, 1페이지에서 2페이지로 이동할 때 캐싱 되어 있는 데이터가 없다면 1페이지의 데이터를 보여주다 2페이지 데이터 목록을 다 받아오면 그 떄 데이터 목록을 바꿔줌으써 사용자 경험을 개선시킬 수 있다.
- 뿐만 아니라, isPreviousData 값을 이용하여 해당 데이터가 현재 쿼리 키에 해당하는 값인지도 확인할 수 있다. 예를 들어, 아직 새로운 데이터를 받아오지 않았다면 true를 반환하고 받아온 뒤에는 false를 반환한다.
