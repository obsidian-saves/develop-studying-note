---
tags:
  - React
  - 훅
---
# useContext란?
>컴포넌트 트리 깊숙한 하위 컴포넌트에서도 부모의 데이터를 props 전달 없이 사용할 수 있도록 도와주는 훅이다.
>**Context**는 **Redux**, **Jotai**, **Zustand**와 같은 **상태 관리 툴**이 절대 아니다. **Context**에 전달되는 **값**은 직접 **관리**해야 한다.
---
## 왜 사용해야 할까?
기본적으로 리액트는 **부모 컴포넌트**에서 **자식 컴포넌트**에게 값을 넘겨주는 **트리 구조**로 되어 있다.
부모에서 자식에게 **데이터**를 넘겨주기 위해선 `props`를 통해서 넘겨주는 것이 일반적이다.
그러나 여러 컴포넌트를 지나서 값을 넘겨주게 된다면 코드가 **매우 복잡해질 것**이다.
```tsx
<A props={something}>
  <B props={something}>
    <C props={something}>
      <D props={something}/>
    </C>
  </B>
</A>
```
위 코드처럼, A라는 부모 컴포넌트에서 D 자식 컴포넌트에게 데이터를 전달하려면 B와 C를 지날 수밖에 없다.
이러한 기법을 `props drilling`이라 한다.

리액트에서는 이러한 문제점을 해결하기 위해 내놓은 개념이 `Context`라는 것이다.
**Context**를 사용하면, **명시적인** props 전달 없이도 선언한 **하위 컴포넌트 모두**에게 자유롭게 원하는 값을 **제공**할 수 있다.

---
## 어디에서 사용할까?
Context는 주로 다음과 같은 두 가지 상황에서 사용된다.
### 1. 전역 데이터를 다룰 때
컴포넌트 트리 전체에서 사용되는 **데이터**가 **존재**한다면, 복잡하게 `props drilling` 현상을 일으킬 필요가 없다.
**전체 컴포넌트**를 `context provider`로 감싸주어 **provider**에서 **전체 컴포넌트 트리**에서 다루는 데이터 생성 후 **value 값**에 데이터를 전달하면 컴포넌트 어디서든 **useContext**를 사용하여 원하는 데이터를 가져올 수 있다.
### 2. 일부 컴포넌트 트리에서 자주 공유되는 데이터를 다룰 때
반드시 **전역 데이터**를 다룰 때만 사용하는 것은 아니다. **전체 컴포넌트 트리**에서 사용되는 것이 아닌, **일부 컴포넌트 트리**에서 자주 공유하는 **데이터**가 있다면 `context provider`을 사용하여 **데이터**를 편리하게 **사용**할 수있다.

---
## 기본 사용법
먼저 로그인한 **사용자 정보**를 **여러 컴포넌트**에서 사용해야 하는 경우가 있다고 가정해 보자.

**Context**를 사용하기 전 **구조**는 아래와 같다.
```
App
 └─ Layout
     └─ Header
         └─ Profile
```
로그인 한 사용자의 정보는 `App`에서 관리하지만, 실제로 필요한 곳은 `Profile`이다.

```tsx
// App.tsx
function App() {
  const user = { name: "rlaxogh76", role: "admin" };

  return <Layout user={user} />;
}
```

```tsx
// Layout.tsx
function Layout({ user }: { user: any }) {
  return <Header user={user} />;
}
```

```tsx
// Header.tsx
function Header({ user }: { user: any }) {
  return <Profile user={user} />;
}
```

```tsx
// Profile.tsx
function Profile({ user }: { user: any }) {
  return <div>{user.name}</div>;
}
```
`Layout.tsx`와 `Header.tsx`에서는 user `props`가 사용되지 않지만, 전달만 하는 코드는 계속 늘어가는 문제점이 **발생**하게 된다.
이를 어떻게 해결할 수 있을까?

먼저 **Context**를 생성해야 한다.

- **1. UserContext 생성하기**
```tsx
// UserContext.tsx
import { createContext } from "react";

type User = {
  name: string;
  role: string;
};

export const UserContext = createContext<User | null>(null);
```

- **2. Provider로 감싸기**
```tsx
// App.tsx
import { UserContext } from "./UserContext";

function App() {
  const user = { name: "HaeSang", role: "admin" };

  return (
    <UserContext.Provider value={user}>
      <Layout />
    </UserContext.Provider>
  );
}
```

- **3. 필요한 곳에서만 useContext 사용**
```tsx
// Profile.tsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {
  const user = useContext(UserContext);

  if (!user) return null;

  return <div>{user.name}</div>;
}
```

이 과정을 통해 기존의 `Layout.tsx`와 `Header.tsx`를 다음과 같이 **변경**할 수 있다.
```tsx
// Layout.tsx
function Layout() {
  return <Header />;
}
```

```tsx
function Header() {
  return <Profile />;
}
```

---
## 주의점
### 1. Context 값 변경은 하위 트리 전체 리렌더링을 유발한다.
**Context Provider**의 `value`가 변경되면, 별도의 최적화가 없는 한 **Provider** 하위의 **모든 컴포넌트**가 다시 **리렌더링**된다.
이는 **리액트**의 **렌더링 규칙**인 `“부모가 리렌더링되면 자식도 리렌더링된다”` 라는 특성 때문이다.
따라서 **Context**를 **전체 컴포넌트 트리**를 감싸거나 구조가 크고 복잡한 경우 **불필요한 리렌더링**을 통해 성능 문제가 **발생**할 수 있다.
#### Provider 자체의 리렌더링을 막는 방법
위 **문제점**을 해결하려면 다음 중 하나의 방법을 통해 **하위 트리 전체 리렌더링**을 **방지**할 수 있다.

- **Provider** 바로 아래 **컴포넌트**에 [React.memo](React.memo.md) **적용**
- **Provider**의 `children`으로 하위 컴포넌트를 **전달**

이를 통해 **Context** 값 변경 시 실제로 **필요한 컴포넌트**만 리렌더링되도록 **제어**할 수 있다.
### 2. Context Provider는 하나의 value만 전달한다.
**Context Provider**는 단 **하나의** `value` 객체만 하위 트리에 **전달**한다.
```tsx
const value = { a: 1, b: 2, c: () => {} };

<Context.Provider value={value}>
  {children}
</Context.Provider>
```
위 구조에서 `a` 값만 변경되더라도 `value` 객체 자체가 새로 **생성**된다.
따라서 `b`, `c`만 사용하는 컴포넌트도 모두 **리렌더링**된다.
#### Context를 역할별로 분리하여 리렌더링을 막는 방법
**불필요한 리렌더링**을 **방지**하면 **상태와 액션**이 서로 다른 **Context**로 분리하는 것이 **효과적**이다.
```tsx
const ToggleStateContext = React.createContext(null);
const ToggleActionContext = React.createContext(null);

function Toggle({ children }) {
  const [on, setOn] = React.useState(false);

  const stateValue = React.useMemo(() => ({ on }), [on]);
  const actionValue = React.useMemo(() => ({ setOn }), []);

  return (
    <ToggleStateContext.Provider value={stateValue}>
      <ToggleActionContext.Provider value={actionValue}>
        {children}
      </ToggleActionContext.Provider>
    </ToggleStateContext.Provider>
  );
}
```
코드와 같이 분리하면 `on` 값은 사용하는 컴포넌트만 상태가 변경될 시 **리렌더링**하고,
`setOn`만 사용하는 컴포넌트는 **리렌더링**되지 않는다.

---
## 참고
[[React] useContext 사용법 및 예제](https://itprogramming119.tistory.com/entry/React-useContext-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C)
[useContext – React](https://ko.react.dev/reference/react/useContext)