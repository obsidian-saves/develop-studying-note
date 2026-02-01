---
tags:
  - SpringBoot
---

# build.gradle이란?
build.gradle은 파일 자체가 Project 오브젝트(객체)로, Project 오브젝트는 Project 인터페이스를 구현하는 구현체를 의미한다. Project 오브젝트는 Project 단위에서 필요한 작업을 수행하기 위해 모든 메서드와 프로퍼티를 모아놓은 슈퍼 객체이다.

```
public interface Project extends Comparable<Project>, ExtensionAware, PluginAware {

// 코드

}
```

위 코드와 같이 우리가 build.gradle 안에 적는 모든 코드는 Project 오브젝트의 `프로퍼티`와 `메서드`가 되며, Project 오브젝트는 `프로젝트 이름`, `변수`, `메서드`를 포함하는 `객체`가 된다.

Project 오브젝트 내부에는 수많은 메서드(Method)와 속성(Properties)를 가진다. 메서드 중 대표적인 모든 java application용 build.gradle을 가진 `plugins`, `repositories`, `dependencies`, `application` 메서드가 있다.

![[2132edqw.png]]
### 의미

- **plugins란**
	빌드 스크립트에 많은 작업을 작성하고, 구성 블록의 중복이 많아지면 프로젝트가 복잡해질 수 있는데, 이때 플러그인을 사용해서 여러가지 기능을 제공해준다.
    
    ### **이점**
    - `재사용성 증진`
        
        **여러 프로젝트에서 유사한 논리를 복제할 필요성이 줄어든다.**
        
    - `모듈성 향상`
        
        **모듈화되고 체계화된 빌드 스크립트가 가능하다.**
        
    - `논리 캡슐화`
        
        **명령형 논리를 분리하여 선언적인 빌드 스크립트가 사용 가능하다.**
        
- **repositories란**
    의존성 라이브러리를 어디서 다운로드 받을지 지정하는 공간이다.
    
    ### 이점
    - `자동 의존성 다운로드`
        
        필요한 라이브러리를 빌드할 때 자동으로 다운로드할 수 있다.
        
    - `네트워크 위치 지정`
        
        어떤 서버에서 라이브러리를 받을지 지정이 가능하다.
        
    - `빌드 속도 최적화`
        
        필요한 저장소만 검색하여 불필요한 검색 감소가 가능하다.
        
    - `보안 및 버전 관리`
        
        검증된 repository만 사용이 가능하다. ( 예 : 회사에서 Nexus 사용 시 )
        
    - `오픈소스 활용`
        
        Spring Boot, Lombok 등 다양한 오픈소스 쉽게 활용이 가능하다.
        
    
    ### 주의할 점
    - 꼭 필요한 repository만 등록 → 중복 검색 줄임
    - 사설 저장소 사용 시 인증 필요 가능
    - 오래된 저장소 (jcenter)는 되도록 사용 지양
    
- **dependencies란 //** 나중에 다시 재정리 필요
    
    의존성을 관리하고 설정하는 프로퍼티이다. 프로젝트에 필요한 라이브러리의 정보를 기술하면 그 라이브러리를 참조할 수 있게 된다. Gradle에서는 `compile` ,`implementation` ,`testlmplementation` 등의 다양한 옵션을 제공한다.
    
    - **compile**
        
        모듈을 수정하게 되면 이 모듈을 직 간접적으로 의존하는 모든 모듈이 recompile 된다.
        
    - **implementation**
        
        컴파일 + 런타임
        
        모듈을 수정하게 되면 모듈을 직접 의존하는 모듈만 recompile하게 된다.
        
    - **compileOnly**
        - 컴파일 시에만 빌드하고 빌드 결과물에는 포함되지 않는다.
        - runtime시 필요 없는 라이브러리인 경우를 말하는데, 이 말은 즉 runtime 환경에 이미 라이브러리가 제공되고 있는 경우를 말한다.
    - **annotationProcessor**
        
        어노테이션 프로세서 사용하는 라이브러리인 경우 사용, 컴파일에만 적용 ( 예시 : lombok )
        
        옛날 버전에서는 compileOnly를 썼었는데 최신 버전에서는 사용이 불가능하다.
        
- **application란**