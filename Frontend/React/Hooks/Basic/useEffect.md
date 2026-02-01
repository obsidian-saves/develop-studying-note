---
tags:
  - React
  - 훅
---
## useEffect란?
>**리액트에서 컴포넌트가 렌더링 될 때마다 특정 작업을 실행하는 훅**이다.
>주로 API 요청 후 **데이터를 저장**하거나, **외부 라이브러리 초기화**, **이벤트 리스너 등록 및 제거** 등에 사용된다.

---
## 왜 사용해야 할까?
리액트 공식 문서에 따르면 useEffect를 사용하여 외부 시스템과 컴포넌트를 동기화할 수 있게 만들 수 있다고 한다.

여기서 외부 함수는 react가 직접 제어하지 않는 요소들을 의미한다.
- **API 통신**
>서버와 데이터를 주고 받는 작업
- **DOM 제어**
>useRef를 이용해 특정 DOM 요소에 접근하고 조작하는 작업
- **전역 객체와의 상호작용**
>window와 같은 전역 객체에 이벤트 리스너를 등록하거나 해제하는 작업

이는 **JSON Place Holder API**와 연동할 때를 예시로 들어 이해할 수 있다.

**JSON Placeholder API**를 통해 요청을 했을 때, React에서는 [[Axios]]를 사용하여 데이터를 **요청**할 수 있다.
이때, useEffect를 쓰지 않으면 데이터 상태가 바뀔 때마다 **렌더링**이 일어나고,
그때마다 또 요청을 보내서 결국 **무한루프**에 빠지는 현상이 발생한다.
## 주요 용어
**useEffect**와 관련하여 나올 **중요한 용어들**이니 꼭 **기억**하도록 하자.

| `용어`                   | `설명`                                                                                                 |
| ---------------------- | ---------------------------------------------------------------------------------------------------- |
| **마운트(mount)**         | **컴포넌트가 최초 렌더링될 때 거치는 과정.** 이후 **props, state**가 **변경**될 때는 마운트 과정을 거치지 않는다.                         |
| **언마운트(unmount)**      | **컴포넌트가 삭제될 때 거치는 과정.**                                                                              |
| **부수 효과(Side Effect)** | **컴포넌트 렌더링(UI 그리기 행위 등) 외에 발생하는 모든 작업.**<br>**외부 시스템**과의 **상호작용**(데이터 가져오기, DOM 직접 조작, 구독 등)을 **의미** |


---
## 특징 살펴보기
useState의 특징은 대표적으로 아래와 같다.

- **Side Effect 관리**
>API 호출, 이벤트 리스너 등록/해제, 타이머 설정 등 렌더링 이후에 발생하는 비동기 작업이나 외부 세계와 상호작용하는 로직을 처리한다.
- **렌더링 후 실행**
>컴포넌트가 화면에 렌더링된 후에 실행되므로, 렌더링 성능에 영향을 주지 않으면서 부수 효과를 처리할 수 있다.
- **클린업(Cleanup) 기능**
>`useEffect` 안에서 `return 함수`를 통해 언마운트 시점에 타이머 해제, 구독 취소 등 리소스를 정리하는 로직을 실행하여 메모리 누수를 방지한다.

---
## 기본 사용법
- **1. 마운트 시 한 번만 실행**
```jsx
useEffect(() => {
  // 실행할 코드
}, [의존성 배열]);
```
의존성 배열을 빈 배열로 설정하면 컴포넌트가 처음 마운트될 때만 실행된다.

- **2. 특정 값이 바뀔 때만 사용**
```jsx
const [count, setCount] = useState(0);

useEffect(() => {
  console.log("count가 바뀔 때마다 실행:", count);
}, [count]);
```
count가 변경될 때마다 useEffect가 실행된다. 의존성 배열에 있는 값이 바뀔 때만 작동한다.

- **3. 클린업 함수 사용 (언마운트 또는 재실행 전에 실행됨)**
```jsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log("1초마다 실행");
  }, 1000);

  // 정리(clean-up) 함수
  return () => {
    clearInterval(interval);
    console.log("컴포넌트가 언마운트되거나 의존성 변경 시 정리");
  };
}, []);
```
코드에서 `setInterval`이 1초마다 로그를 찍게 되는데, 
이때 컴포넌트가 **사라지거나**(unmount) 다시 실행되기 전에 `clearInterval`로 타이머를 **정리**한다.
이를 **클린업 함수**라고 한다.
### 예시
아래는 JsonPlaceHolder에 요청을 보내 user 데이터의 모든 이름을 받는 코드이다.
만약 useEffect를 사용하지 않고 외부 API와 통신하게 된다면, 요청이 끝없이 반복될 것이다.
```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

export default function JsonFetcher() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        setUsers(res.data);
      })
      .catch((err) => {
        console.log("오류 발생 :", err);
      });
  }, []);

  return (
    <>
      {users.length > 0 ? (
        <ul>
          {users.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      ) : (
        <p>유저 정보를 불러오는 중...</p>
      )}
    </>
  );
}

```

---
## 주의점
### 1. useEffect간의 연쇄 작용
useEffect를 1개 추가하면 고려해야 할 side Effect도 1개가 된다.
하지만 useEffect가 늘어날 수록 고려해야 할 경우의 수는 선형적으로 늘어나지 않고,
연쇄반응 때문에 관리해야 할 side effect는 기하급수적으로 늘어날 수 있다.

아래 코드는 이 문제점을 그대로 보여주는 예시다.
```tsx
import { useEffect, useState } from "react";

export default function Example() {
  const [userId, setUserId] = useState<number | null>(null);
  const [user, setUser] = useState<any>(null);
  const [posts, setPosts] = useState<any[]>([]);
  const [filteredPosts, setFilteredPosts] = useState<any[]>([]);

  // 1. userId 변경 → user 조회
  useEffect(() => {
    if (userId === null) return;

    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => setUser(data));
  }, [userId]);

  // 2. user 변경 → posts 조회
  useEffect(() => {
    if (!user) return;

    fetch(`/api/posts?userId=${user.id}`)
      .then(res => res.json())
      .then(data => setPosts(data));
  }, [user]);

  // 3. posts 변경 → 필터링
  useEffect(() => {
    const filtered = posts.filter(post => post.likes > 10);
    setFilteredPosts(filtered);
  }, [posts]);

  // 4. filteredPosts 변경 → 로그 전송
  useEffect(() => {
    if (filteredPosts.length === 0) return;

    fetch("/api/log", {
      method: "POST",
      body: JSON.stringify({ count: filteredPosts.length }),
    });
  }, [filteredPosts]);

  return (
    <button onClick={() => setUserId(1)}>
      Load User
    </button>
  );
}
```
위 코드를 보면 단순히 "useEffect는 4개다"라고 생각할 수도 있다. 하지만 `setUserId(1)` 한 번의 이벤트로 인해 다음과 같은 흐름이 발생한다.
>1. userId 변경
>2. (1) -> user fetch
>3. (2) -> user 변경
>4. (3) -> posts fetch
>5. (4) -> posts 변경
>6. (5) -> filteredPosts 변경
>7. (6) -> 로그 API 호출

이처럼 side effect가 도미노처럼 연쇄적으로 발생하게 된다. 그렇다면 어떻게 해결할 수 있을까?
#### 해결 방법
**1. 연쇄되는 useEffect를 하나의 흐름으로 묶기**
>아래 코드처럼 하나의 **트리거(userId)**를 기준으로 명시적인 순서를 만든다.
```tsx
import { useEffect, useState } from "react";

export default function Example() {
  const [userId, setUserId] = useState<number | null>(null);
  const [user, setUser] = useState<any>(null);
  const [posts, setPosts] = useState<any[]>([]);
  const [filteredPosts, setFilteredPosts] = useState<any[]>([]);

  useEffect(() => {
    if (userId === null) return;

    const loadData = async () => {
      // 1. user 조회
      const user = await fetch(`/api/users/${userId}`).then(res => res.json());
      setUser(user);

      // 2. posts 조회
      const posts = await fetch(`/api/posts?userId=${user.id}`).then(res =>
        res.json()
      );
      setPosts(posts);

      // 3. 필터링
      const filtered = posts.filter(post => post.likes > 10);
      setFilteredPosts(filtered);

      // 4. 로그 전송
      if (filtered.length > 0) {
        await fetch("/api/log", {
          method: "POST",
          body: JSON.stringify({ count: filtered.length }),
        });
      }
    };

    loadData();
  }, [userId]);

  return (
    <button onClick={() => setUserId(1)}>
      Load User
    </button>
  );
}
```
**2. side effect가 아닌 로직은 useEffect에서 제거하기**
> 코드에서 `filteredPosts` 부분은 side Effect가 아니므로, 아래 코드처럼 만들면 된다.
```tsx
const filteredPosts = useMemo(
  () => posts.filter(post => post.likes > 10),
  [posts]
);
```

---
## 참고
[useEffect는 왜 useEffect 일까? | iamseon](https://iamseon.com/post/useEffect%EB%8A%94-%EC%99%9C-useEffect-%EC%9D%BC%EA%B9%8C#%EC%99%9C-useEffect)
[useEffect를 남용하지 말자 - react](https://gusrb3164.github.io/web/2021/12/29/less-use-useeffect/)
