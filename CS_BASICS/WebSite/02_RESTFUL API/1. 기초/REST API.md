---
tags:
  - Website
---

## 의미
> [[REST]] 원칙을 따르는 [[API]].

REST 원칙을 지켜 만들어진 API를 "RESTFUL하다."라고 한다. 하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아니고, REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있다.

---
## 지켜야할 원칙
 REST API를 올바르게 설계하기 위해서는 지켜야 하는 몇가지 규칙이 있으며 해당 규칙을 지켜야한다.

1. [[URI]]는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.
	- 잘못된 예시 : http://localhost:8080/Running/ 
	- 좋은 예시 : http://localhost:8080/run/

2. 마지막에 슬래시(/)를 포함하지 않는다.
	- 잘못된 예시 : http://localhost:8080/test/
	- 좋은 예시  : http://localhost:8080/test

3. 언더바( _ ) 대신 하이픈(-)을 사용하여야 한다.
	- 잘못된 예시 : http://localhost:8080/test_blog
	- 좋은 예시  : http://localhost:8080/test-blog

4. 파일 확장자는 [[URI]]에 포함하지 않는다.
	- 잘못된 예시 : http://localhost:8080/photo.jpg
	- 좋은 예시  : http://localhost:8080/photo

5. 행위를 포함하지 않는다.
	- 잘못된 예시 : http://localhost:8080/delete-post/1
	- 좋은 예시 : http://localhost:8080/post/1
---
