# staleTime과 cacheTime // 2023년 7월 13일 (Thu)

## staleTime

1. stateTime(number | Infinity)

- staleTime은 데이터가 fresh에서 stale 상태로 변경되는데 소요되는 시간이다. 만약 staleTime을 1분으로 지정한다면, fresh 상태에서 1분이 지난 뒤 stale 상태로 변경이 된다.
- 상태가 fresh 상태일때는, 쿼리 인스턴스가 다시 mount 되어도 네트워크 요청을 하지 않는다. 즉, unmount 되었다가 mount 되어도 아직 fresh 상태라면 네트워크 요청을 다시 하지 않는다.
- 값을 따로 지정하지 않는다면, 기본 값은 0이므로 일반적으로 네트워크 요청 후 응답을 받고나면 바로 상태가 stale로 바뀌게 된다.

2. cacheTime(number | Infinity)

- 데이터가 inactive 상태인 경우 캐싱 된 상태로 남아 있는 시간이다. 쿼리 인스턴스가 unmount 되면 데이터는 inactive 상태로 변경 되며, 캐시는 지정한 cacheTime 만큼 유지가 된다.
- cacheTime이 지나고 나면, 가비지 콜렉터로 수집 된다. cacheTime이 지나기 전에 쿼리 인스턴스가 다시 mount 되면, 데이터를 fetch 하는 동안 캐시 데이터를 보여준다.
- cacheTime은 staleTime과 관계없이, 무조건 inactive 된 시점을 기준으로 cacheTime 만큼 시간이 지난다면 캐시 데이터 삭제를 한다.
- 값을 따로 지정하지 않는다면, cacheTime의 기본값은 5분이다.
  수

#### staleTime이 0인 이상, 데이터는 절대 fresh하다고 간주할 수 없다. 만약에 사용하는 데이터가 자주 변경되는 데아터라면, 기본 설정 값을 유지하는게 좋다. 하지만, 데이터의 변경이 자주 일어나지 않는다면 staleTime을 조정하여 사용하는게 불필요한 API 호출을 더 줄일 수 있음으로 더 효율적이다. 만약 staleTime을 길게 설정하더라도, cacheTime이 짧다면 이 또한 캐싱이 원활하게 진행되지 않을 것이다. 그러므로 staleTime, cacheTime 두 가지 옵션을 적절하게 설정해주어야 한다.

```
const { data: userInfo } = useQuery(
  ['userInfo'],
  getUserInfo,
  {
    cacheTime: 5 * 60 * 1000, // 5분
    staleTime: 1 * 60 * 1000. // 1분
  }
)
```
