# 회사 업무에서의 TIL // 2022년 8월 8일 (Mon)

## 코호트 디스커버리 카탈로그 툴팁 작업 중 고은이 덕분애 parent 컴포넌트에서 children 컴포넌트에 ref를 전달하여 글자수가 길어져 말 줄임이 된 경우를 찾아내어 툴팁을 적용하는 좋은 방법을 배웠다. 처음으로 children 컴포넌트에 React.cloneElement 함수를 사용하여 ref를 전달할 수 있다는 것을 알게 되었고 그것에 대해 간략하게 기록하고자 한다.

### React.cloneElement

- 기본 형태는 다음과 같다.
  - React.cloneElement(element, [config], [...children])
  - 첫번째 인자인 element를 기준으로 element를 복사하고 새로운 element를 반환한다.
  - 두번째 인자인 config는 새로운 props와 key, ref를 의미한다. 기존 element가 가지고 있던 props와 config에 전달한 props 값이 합쳐진다.
  - config에 새로운 ref와 key를 전달하지 않았다면, 기존 element의 key와 ref는 그대로 유지된다.

### 실제 사용 코드

```
  // InnerTreeItem -> 각각의 카탈로그 아이템 컴포넌트
  export function InnerTreeItem(label) {
    return(
      ...
        <CatalogTooltip>
          <div className="ellipsis">{label}</div>
          // 카탈로그 툴팁 컵포넌트로부터 ref를 전달 받는다.
        </CatalogTooltip>
      ...
    )
  }

  // CatalogTooltip -> 카탈로그 툴팁 컴포넌트
  export function CatalogTooltip(children) {
    ...
    return(
      <div className="tooltip">
        React.cloneElement(children, {ref: targetRef})
        // cloneElement를 사용하여 자식 컴포넌트에게 ref를 전달한다.
        {isShow &&
          <div>
            <Tooltip>{label}<ToolTip>
          </div>}
      </div>
      ...
    )
  }
```
