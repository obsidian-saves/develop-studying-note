---
tags:
  - WIP
  - React
---

> react는 `웹 프레임워크로`, 자바스크립트 라이브러리의 하나로서 사용자 `인터페이스(User Interface, UI)`를 만들기 위해 사용된다. Facebook(현재 Meta)에서 개발되었으며, `컴포넌트 기반` 아키텍처와 선언형 프로그래밍 방식이 특징이다.

## 특징
- **[[컴포넌트 ❌]] 기반(Component-Based)**  
  UI를 독립적인 컴포넌트 단위로 나누어 재사용성과 유지보수를 용이하게 한다.

- **[[JSX]](JavaScript XML)**
  HTML과 유사한 문법을 JavaScript 코드 내에서 사용할 수 있게 해주는 문법 확장이다.

- **[[Virtual DOM]]**
  가상 DOM을 사용하여 실제 DOM 조작을 최소화하고, 성능을 향상시킨다.

- **[[선언형 프로그래밍]]**
  상태가 변하면 자동으로 UI를 업데이트하므로, 개발자가 DOM 상태를 직접 관리할 필요가 없다.

- **[[단방향 데이터 흐름]](One-way Data Binding)**  
  데이터는 부모에서 자식 컴포넌트로 단방향으로 전달되어 애플리케이션의 상태를 예측 가능하게 한다.

## 간단한 사용 예제
```jsx
import React from "react";
import ReactDOM from "react-dom/client";

// Hello 컴포넌트 정의
function Hello() {
  return <h1>Hello, World!</h1>;
}

// 루트 요소에 컴포넌트 렌더링
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<Hello />);
```