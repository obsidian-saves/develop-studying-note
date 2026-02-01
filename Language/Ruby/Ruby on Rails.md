> Ruby on Rails는 덴마크의 프로그래머 David Heinemeier Hansson이 2004년에 Ruby 언어로 개발한 오픈소스 웹 프레임워크로, 보통 Rails라고 불린다.

## **특징**
### `Convention Over Configuration(설정 보다는 규칙)`
- 개발자가 설정해야할 요소는 줄이고, 규칙을 제공하여 빠른 개발을 가능하게 한다. Rails에서 제공하는 규칙을 지키면서 개발하면, 개발자는 정말 필요한 개발 작업에만 집중할 수 있다는 특징이다.

### `Don't Repeat Yourself (DRY)
- 중복되는 코드를 피하고, 재사용 가능한 구조로 개발할 수 있게 도와준다.  
  같은 로직을 반복하지 않도록 도와주는 구조 덕분에 코드의 유지보수성과 확장성이 높아진다.

### `MVC`
![mvc 패턴](https://velog.velcdn.com/images%2Fy_dragonrise%2Fpost%2Fb1da821a-f44f-404b-b40d-9410c5f6cdcf%2Ff472d15e070e9ca2f3456f8a97b9b1a55ee5ce6b5f29025627c900bc69d958a2.jpeg)
- Rails는 Model, View, Controller의 구조로 작동한다.  
  데이터 처리(Model), 사용자 인터페이스(View), 비즈니스 로직과 흐름 제어(Controller)를 분리하여, 구조적인 개발이 가능하다.