# queryClient // 2023 3월 4일 (Sat)

```
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 0,
      useErrorBoundary: true,
    }
  },
});
```

### retry option은 API가 실패하면, 설정한 값만큼 재시도 하는 옵션이다. useErrorBoundary 옵션은 리액트 16 이상에서 제공하는 Fallback UI 설정에 대한 옵션이다. 예기치 못한 에러 케이스가 생길 수 있기 때문에 useErrorBoundary를 true로 설정해야 react의 errorBoudary를 사용할 수 있다. retry의 default 값은 3이다.
