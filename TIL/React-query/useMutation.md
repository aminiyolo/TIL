# Optimistic Update // 2023년 7월 18일 (Tue)

- react-query에서는 기본적으로 서버에서 데이터를 응답 받아올 때, useQuery를 사용한다.
- 만약, Get 요청이 아닌 Post, Patch, Put, Delete 요청을 하는 경우에는 useMutation을 사용해야 한다.
- 즉, R(read)는 useQuery / CUD(Create, Update, Delete)는 useMutation을 사용해야 한다.

```
const useUpdateUserInfo = () => {
  return useMutation(
    (params) => updateApiCall(params), {
        onMutate: () => {
        console.log("Sending Request...")
      },

      onSuccess: (data) => {
        console.log("Response has arrived")
      },

      onError: (error) => {
        console.log("Error! Error!")
      },

      onSettled: () => {
        console.log("")
      }
    }
  )
}
const { mutate } = useUpdateUserInfo();
const onClickUpdata = useCallback(() => {
  // useMutation 함수가 리턴해주는 mutate 함수를 이용
  // 클릭하여 서버에 요청
  mutate(params);
}, [])
```

- useMutation의 반환 값인 mutation 객체의 mutate 메서드를 이용해서 요청 함수를 호출할 수 있다.
- useMutation은 onSuccess, onError 메서드를 통해 요청이 성공 했을 때와 실패 했을 때 핸들링 할 수 있다.
- onMutate 메서드는 useMutation에 첫번째 인자로 넣은 콜백 함수가 실행되기 이전에 실행이 되어진다.
- onSettled 메서드는 우리가 아는 try, catch, finally 문에서 finally 처럼 요청의 성공 실패와 상관 없이 최종적으로 실행된다.
