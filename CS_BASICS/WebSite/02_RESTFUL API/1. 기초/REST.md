---
tags:
  - Website
---

# REST (Representational State Transfer)

## 1. 의미
> Representational State Transfer의 약자로, **자원을 이름으로 구분**하여  
> 해당 자원의 **상태를 주고받는 모든 행위**를 의미한다.

즉, REST란  
1. HTTP [[URI]](Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,  
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해  
   해당 자원([[URI]])에 대한 **CRUD Operation**을 적용하는 것을 말한다.

> **CRUD Operation이란?**  
> 컴퓨터 소프트웨어의 기본 데이터 처리 능력인 **Create, Read, Update, Delete**를 일컫는다.

| CRUD | 의미 | HTTP Method |
|------|------|-------------|
| Create | 데이터 생성 | POST |
| Read | 데이터 조회 | GET |
| Update | 데이터 수정 | PUT / PATCH |
| Delete | 데이터 삭제 | DELETE |

---

## 2. 핵심 구성요소
REST는 다음 세 가지 요소로 구성된다.

1. **자원(Resource)** : HTTP [[URI]]  
2. **자원에 대한 행위(Verb)** : HTTP Method  
3. **자원에 대한 표현(Representation)** : HTTP Message [[Payload]]

---

## 3. 특징
1. **Server-Client** : 서버와 클라이언트의 명확한 역할 분리  
2. **Stateless** : 서버는 클라이언트 상태를 유지하지 않음  
3. **Cacheable** : HTTP 캐시를 이용해 응답 성능 향상 가능  
4. **Layered System** : 계층적 구조로 구성 가능  
5. **Uniform Interface** : 일관된 인터페이스 제공  

---

## 4. 장단점

| 구분    | 내용                                                                                                                  |
| ----- | ------------------------------------------------------------------------------------------------------------------- |
| ✅ 장점  | - HTTP 인프라를 그대로 활용 (별도 환경 필요 없음)<br>- 표준 프로토콜 활용으로 범용성 높음<br>- REST API 메시지가 의도를 명확히 표현<br>- 서버와 클라이언트 역할 분리로 설계 명확 |
| ⚠️ 단점 | - 공식 표준 부재로 정의 필요<br>- HTTP Method 제한 존재<br>- Header 중심 처리로 초심자 진입장벽 높음<br>- 구형 브라우저 호환성 문제 (특히 IE)                 |

---

## 5. 관련 개념
- [[HTTP]]  
- [[URI]]  
- [[CRUD]]  
- [[Hypermedia]]  
- [[RESTful API]]  

---

## 6. 참고 자료
- [REST 위키백과](https://ko.wikipedia.org/wiki/REST)
- [MDN REST 개요](https://developer.mozilla.org/ko/docs/Glossary/REST)
- [Roy Fielding 논문](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
