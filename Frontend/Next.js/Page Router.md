---
tags:
  - WIP
  - NextJS
---
# Page Router

## 1. 의미
> Page Router란?
> [[React Router]]처럼 pages 폴더를 기반으로 페이지 라우팅을 해주는 기술이다.

![[Pasted image 20251018095750.png]]
사진과 같이, pages 폴더 내 파일명을 기준으로 페이지 이동이 가능하다.

![[Pasted image 20251018095902.png]]
파일명 외에도 폴더구조를 통해 이동이 가능하도록 기능을 제공한다.

## 2. 동적 경로(Dynamic Routes)
![[Pasted image 20251018100048.png]]
블로그나 쇼핑 웹사이트를 방문하게 되면 url 주소 뒤에 번호가 있는 것을 몇 번 보았을 것이다. 이를 `동적 경로`라고 부르는데, Next에선 위 사진과 같이 대괄호 안에 원하는 가변적인 값을 넣어 페이지를 라우팅할 수 있다.