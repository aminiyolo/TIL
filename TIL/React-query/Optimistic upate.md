# Optimistic Update // 2023년 7월 10일 (Mon)

## Optimistic updates

#### 회사에서 개발 및 유지보수를 진행하고 있는 정밀의료플랫폼 CDW에 연구과제, 데이터셋의 즐겨찾기를 체크하는 경우, 서버에서 응답이 오기 전까지는 체크 표시가 되지 않아 조금 버벅이는 느낌을 받았다. 사용하는 의료진들도 분명히 똑같은 사용자 경험을 느낄 것이라고 생각이 되었고, 낙관적 업데이트를 하여 서버에서 응답이 오기전에 우선 사용자에게 UI update를 보여주어야 겠다고 생각했다. 현재 프로젝트에서 react-query를 이용하여 서버 상태를 관리하고 있기 때문에 react-query에 여러 기능들을 살펴보다, Optimistic updates 기능이 있는 것을 발견하였고, 해당 기능을 이용하여 원하던 개선 방향으로 개발을 완료할 수 있었다. 해당 기능에 대하여 요약 정리를 해보자.

### 리액트 쿼리는 optimistic updates를 지원하며, 이는 데이터를 서버로부터 성공적으로 업데이트 하기 전에도 UI를 업데이트 할 수 있도록 해준다. 즉, 요청을 보내기 전에 UI를 업데이트 하는 기능이다.

### 예를 들어, 사용자가 게시물에 좋아요 또는 즐겨찾기를 눌렀을 때, 즉시, 좋아요 표시나 즐겨찾기 표시가 바뀔 수 있도록 도와준다. optimistic updates는 사용자 경험을 개선하고, 서버에서 응답을 받을때 까지 기디리지 않고 사용자에게 빠른 피드백을 제공할 수 있다.하지만 요청이 실패하면 업데이트 하기 전으로 돌려야 하기 때문에, 이를 처리하는 롤백 로직을 함께 구현해야 한다.

### react-query에서 optimistic updates를 수행하는 방법은 useMutation 훅에서 onMutate 옵션을 사용하는 것이다. onMutate 함수는 useMutation 훅 안에서, API 호출 전에 실행되는 함수이다. 이 함수는 최적화된 업데이트를 위해 현재 데이터 캐시를 업데이트 하거나, UI를 변경하는 등의 작업을 수행할 수 있다. onMutate 함수는 업데이트가 성공적으로 이루어지지 않았을 때 사용할 수 있는 rollback 메커니즘도 제공한다.

#### 아래 공식 문서의 예시 코드를 살펴보자.

```
const queryClient = useQueryClient()

useMutation({
  mutationFn: updateTodo,
  // When mutate is called:
  onMutate: async (newTodo) => {
    // 영향을 줄 가능성이 있는 쿼리를 캔슬 시켜야 한다.
    await queryClient.cancelQueries({ queryKey: ['todos'] })

    // Snapshot the previous value
    // API 요청 오류가 발생한 경우 이전 데이터 값으로 다시 원복 시켜야 하기 때문에, 다음과 같이 이전 값을 변수에 저장한다.
    const previousTodos = queryClient.getQueryData(['todos'])

    // Optimistically update to the new value
    // 쿼리 데이터를 새로운 내용으로 업데이트 시킨다.
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo])

    // Return a context object with the snapshotted value
    // 이전 값을 onError 함수에서 사용하기 위하여 리턴 시킨다.
    return { previousTodos }
  },
  // If the mutation fails,
  // use the context returned from onMutate to roll back
  // 에러가 발생하는 경우, 우리가 onMutate에서 리턴한 previousTodos는 context 값 안에 담겨서 온다.
  onError: (err, newTodo, context) => {
    // 이전 값으로 데이터 원복
    queryClient.setQueryData(['todos'], context.previousTodos)
  },
  // Always refetch after error or success:
  onSettled: () => {
    // 캐시를 무효화 하여 다시 데이터 리패치
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
})
```
