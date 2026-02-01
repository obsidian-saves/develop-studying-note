---
tags:
  - React
---
## Virtual DOM이란?
> **Virtual DOM** (**VDOM**)은 UI의 이상적인 또는 “가상”적인 표현을 메모리에 저장하고 **React DOM**과 같은 라이브러리에 의해 “실제” [[DOM]]과 동기화하는 **프로그래밍 개념**이다. -React 공식 문서-

Virtual DOM은 실제 DOM에 접근하여 조작하는 대신, 추상화한 **자바스크립트 객체**를 구성하여 사용한다.
DOM의 상태를 **메모리**에 저장하고, 변경 전과 변경 후의 상태를 비교한 뒤, 최소한의 내용만 반영하여 **성능 향상**을 이끌어낸다.

쉽게 비유하자면 **Virtual DOM**은 실제 건물을 바로 고치는 작업이 아닌 **설계도**에서 수정한 뒤, 바뀐 부분만 실제 건물에 **반영**하는 것이다.
## Virtual DOM은 왜 사용하는걸까?
그렇다면 이미 존재하는 **DOM**을 사용해도 될 것을, **React**에서는 왜 **Virtual DOM**을 사용하는 것일까? 

만약 규모가 작고 **정적인 웹 애플리케이션**이라면, 일반 **DOM**의 성능이 더 좋을 것이다.
그러나, 지금의 **React**는 **동적인 웹 애플리케이션**에 **중점**을 두고 있다.

[[SPA]]에서는 **DOM 조작**이 빈번하게 발생한다. 변화를 적용하기 위해서는 브라우저가 많은 연산을 하게 되고,
결국 전체적인 **프로세스**가 **비효율적이게** 된다.
## Virtual DOM은 어떤 식으로 동작할까?
이제 Virtual DOM을 사용해야하는 이유를 알게 되었으니, Virtual DOM의 동작 방식을 알아보자.

Virtual DOM을 사용하면 실제 DOM에 접근하며 조작하는 대신, 이를 추상화한 자바스크립트 객체를 구성하여 사용한다.
DOM의 상태를 메모리에 저장하고, 변경 전과 변경 후의 상태를 비교 한뒤 최소한의 내용만 반영하여 성능 향상을 이끌어낸다.
DOM의 상태를 메모리 위에 계속 올려두고, DOM에 변경 있을 경우 해당 변경 사항만 반영하는 것이다.

![](Pasted%20image%2020251223004953.png)

## 참고한 글
```embed
title: "[React] DOM이란? Virtual DOM을 사용하는 이유?"
image: "https://velog.velcdn.com/images/ctdlog/post/7e16ac09-40bd-42d1-8d07-7e1562fb8b43/image.png"
description: "React의 장점 중에는 Virtual DOM이 있다. 근데 대체 Virtual DOM이 무엇일까?Virtual은 말 그대로 가상이라는 뜻이고 DOM은 Document Object Model의 약자로 그대로 해석하면 문서 객체 모델을 의미한다. 문서 객체한 HTML, "
url: "https://velog.io/@ctdlog/React-DOM%EC%9D%B4%EB%9E%80-Virtual-DOM%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0"
favicon: ""
aspectRatio: "54.6875"
```