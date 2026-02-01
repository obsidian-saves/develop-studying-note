---
tags:
  - React
  - 성능
---
# React.memo란?
>리액트의 함수 컴포넌트 성능 최적화를 위한 **고차 컴포넌트**(HOC)로, 컴포넌트가 동일한 **props**를 받았을 때, 불필요한 리렌더링을 방지하고 이전 렌더링 결과를 재사용하게 해주는 **기술**이다.

쉽게 비유하자면, `같은 재료로 또 요리하면, 전에 만들어 둔 걸 그냥 다시 꺼내 쓰는 것과 같다.`

리액트는 props가 변경되면 다시 렌더링하게 된다.
하지만 props가 바뀌지 않았는데도 부모 컴포넌트에 의해 다시 렌더링되는 경우가 발생한다.
이때, **React.memo**를 사용하면 이전 렌더링 시점의 props와 현재 props를 비교하여,
값이 동일한 경우 컴포넌트의 리렌더링을 건너뛰고 기존 렌더링 결과를 그대로 재사용한다.

---
## 왜 사용해야 할까?
>React.memo는 사용자에게 “빠르게 반응하는 화면”을 제공하기 위한 성능 최적화 도구이다.

유저들은 **빠른 UI**를 선호한다. **응답 속도**가 **100ms~300ms** 사이라면 **대부분의 사용자들**이 떠날 것이다.
이러한 문제점을 해결하기 위해 **React.memo**는 **props**가 변경되지 않은 컴포넌트의 **리렌더링**을 방지함으로써,
**렌더링 비용**을 줄이고 **UI 반응 속도**를 **안정적으로 유지**할 수 있도록 도와준다.

---

## 언제 사용할까?
>**React.memo**를 사용하기 좋은 조건은 다음과 같다.
- 렌더링 비용이 작은 컴포넌트
- props가 자주 바뀌는 컴포넌트
- 비교 비용이 렌더링 비용보다 큰 경우

---
## 사용법
기본 사용법은 아래와 같다.

먼저 **memo**를 생성해야한다. 아래 코드와 같이 `Props` 타입을 가지는 `Greeting` 함수를 **memo**를 통해 생성해준다.
```tsx
import React from "react";

type Props = {
  name: string;
};

const Greeting = React.memo(({ name }: Props) => {
  console.log("렌더링!");
  return <h1>Hello {name}</h1>;
});

export default Greeting;
```

만든 **Greeting** 함수를 아래와 같은 코드와 함께 사용할 수 있다.
```tsx
export default function App() {
  const [count, setCount] = React.useState(0);

  return (
    <>
      <Greeting name="Alice" />
      <button onClick={() => setCount(count + 1)}>
        count: {count}
      </button>
    </>
  );
}
```
`count`가 변경되어 렌더링이 발생하더라도 **Greeting**의 `name` **props**는 변하지 않으므로 **Greeting**은 **리렌더링**되지 않는다.
### 예시
**React.memo**가 필요한 상황을 더 알아보자.

아래 코드는 `count`가 변경되지만 `Child`도 함께 리렌더링되는 현상이다.
```tsx
function Child({ value }: { value: number }) {
  console.log("Child 렌더링");
  return <div>{value}</div>;
}

function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Child value={10} />
      <button onClick={() => setCount(count + 1)}>+</button>
    </>
  );
}
```
이를 해결하기 위해선 **React.memo**를 생성하여 `Child`가 렌더링되지 않도록 만들어야한다.
```tsx
const Child = React.memo(({ value }: { value: number }) => {
  console.log("Child 렌더링");
  return <div>{value}</div>;
});
```

---

## 주의점
### 1. 모든 컴포넌트에 사용하면 안된다.
**React.memo**는 **props**를 비교하는 과정에서 **비교 비용**이 발생한다.
렌더링 비용이 **매우 작은 컴포넌트**에 무분별하게 적용하면 **props** 비교 비용이 렌더링 비용보다 더 비싸져 오히려 **성능**이 더 나빠질 수 있다.
### 2. 객체, 배열, 함수 props에 주의해야 한다.
**React.memo**는 기본적으로 [[얕은 비교(shallow Equality)]]를 수행한다.

아래와 같은 경우, 렌더링마다 새 객체가 생성되므로 **props**가 변경되었다고 판단하여 **React.memo**가 동작하지 않는다.
```tsx
<Child user={{ name: "Alice"}} />
```
이러한 경우에는 [useMemo](useMemo.md)와 [useCallback](useCallback.md)을 함께 사용해야 한다.
```tsx
const user = useMemo(() => ({ name: "Alice" }), []);
<Child user={user} />
```
### 3. props가 자주 변경되는 컴포넌트에는 효과가 적다.
**props**가 자주 바뀌는 컴포넌트는 매번 **리렌더링**이 발생하므로 **React.memo**를 사용해야할 이점이 거의 없다.
이런 경우에는 구조를 **분리**하거나, 상태 위치를 **조정**하는 것이 더 효과적일 수 있다.
### 4. 커스텀 비교 함수는 신중하게 사용해야 한다.
**React.memo**의 두 번째 인자로 커스텀 비교 함수를 전달할 수 있다.
```tsx
React.memo(Component, areEqual)
```
하지만 비교 로직이 복잡할수록 카드 가독성이 떨어지고, 비교 비용이 증가, 버그가 발생할 가능성도 커진다.
정말 필요한 경우에만 사용하도록 하자.

---

## 정리
React.memo는 **props**가 변경되지 않으면 컴포넌트의 리렌더링을 방지하는 **성능 최적화 도구**이다.
필요한 경우에만 사용하는 것이 좋다.

---

## 참고
[React 공식 문서](https://ko.react.dev/reference/react/memo)
