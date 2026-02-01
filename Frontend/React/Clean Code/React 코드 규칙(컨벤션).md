`아래는 협업에서 유지보수 및 가독성, 코드 이해를 돕기 위해 개발자들 사이에 코드를 작성할 때 지켜야할 코드 규칙(컨벤션)이다.`
# 1. NAMING CONVENTIONS

1. **Components의 이름은 `Pascal Case`로 작성하라.**
	- `Header.js Footer.jsx MainBanner.js BlogList.jsx`

2. **Non-Components의 이름은 `Camel Case`로 작성하라.**
	- `myUtilityFile.js cookieHelper.js fetchApi.js`

3. **`Unit test` 파일명은 대상 파일명과 동일하게 작성하라.**
	- `MainBanner.js MainBanner.test.js BlogList.js BlogList.test.js`

4. **속성명은 `Camel Case`로 작성하라.**
```jsx
className
<div className=""></div>

onClick, onSubmit
<div onClick="{}" onSubmit="{}"></div>
```

5. **inline 스타일은 `Camel Case`로 작성하라.**
	- `<div style={{ fontSize: "1rem", fontWeight: "700" }}></div>`


```embed
title: "[react] react 코딩 컨벤션"
image: "https://phrygia.github.io/og-image.png"
description: "최근 면접을 보면서 나의 코딩 스타일에 대해 생각하게 되었다.  나는 많은 인강을 보면서 react를 독학한 케이스인데 아무래도 인강 강사님의 코드 스타일을 따라가게 되었고 무의식적으로 그렇게 코딩을 했었다.  면접관님이 코딩스타일은 다 다르지만 내가 사용한 코딩스타일은 값과 코드를 확인하기 위해 여러군데를 살펴봐야 하기때문에 본인은 선호하는 방식이 아니…"
url: "https://phrygia.github.io/2022-04-05-react/"
favicon: ""
aspectRatio: "50"
```

# 2. TSX으로 작성할 경우

1. NPM 라이브러리 설치시 https://www.npmjs.com/ 으로 접속 후 TS를 지원하는 지 확인해야함. 만약 TS 아이콘이 없다면 사용 불가