# Event bubblig and Event capture // 2021년 8월9일 (월)

## Event bubbling이란?

### 이벤트 버블링은 특정 화면요소에서 이벤트가 발생했을 경우, 해당 이벤트가 상위의 화면 요소들로 전달되는 특성이다.

```
<div class="top" >
  <div class="middle" >
    <div class="target">target</div>
  </div>
</div>

const onClick = () => {
  console.log(event.currentTarget.className);
}

const divs = document.querySelectorAll('div');
divs.forEach((div) => div.addEventListener('click', onClick));
```

#### 위에 코드를 작성한 후 target div를 클릭하면 콘솔에 target -> middle -> top 순서대로 한줄씩 찍힌다.

#### 가장 하위 div를 클릭했지만 그 위의 상위 요소들에도 전부 이벤트가 발생한다.

### 브라우저는 이벤트가 발생했을 때, 그 이벤트를 최상위 요소까지 이벤트를 전파시키는 방식을 취한다. 예를 들어, class가 middle인 div를 클릭하면 -> 콘솔에 middle -> top 순서대로 찍힌다.

#### 하지만, 이벤트가 특정 div 태그에만 달려있다면 이벤트 버블링은 발생하지 않는다.

#### 상위 요소의 이벤트가 등록된 태그가 있을 경우, 이벤트가 전달되는 이벤트 버블링 현상이 발생한다.

## Event capture란?

### 이벤트 캡쳐는 이벤트 버블링과 반대로 진행되는 이벤트 전파 방식이다.

```
<div class="top" >
  <div class="middle" >
    <div class="target">target</div>
  </div>
</div>

const onClick = () => {
  console.log(event.currentTarget.className);
}

const divs = document.querySelectorAll('div');
divs.forEach((div) => div.addEventListener('click', onClick, { capture: true }));
```

### 위와 같이 addEventListener의 옵션에 객체 { capture: true} 값을 전달해주면, target div를 클릭하여도 콘솔에 top -> middle -> target 순서대로 이벤트 버블링과는 반대로 이벤트가 전파되는 것을 확인할 수 있다.

## event.stopPropagation()

### 이러한 이벤트 버블링과 이벤트 캡쳐를 막기 위해서는 웹 api인 event.stopPropagation()를 사용하면 된다.

#### event.stopPropagation()를 사용하면, 이벤트가 전파되는 것을 막을 수 있다. 이벤트 버블링의 경우, 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트가 전파되는 것을 막는다. 이벤트 캡쳐의 경우, 하위 요소로 이벤트가 전파되는 것을 막는다.
