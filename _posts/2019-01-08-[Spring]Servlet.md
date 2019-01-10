---
title: "First Milestone"
date: 2019-01-05 13:26:28 -0400
layout: home
categories:
  - Spring
---

# Servlet

Java를 실행하기 위해서 JRE가 필요한 것 처럼, 서블릿을 실행하려면 웹 어플리케이션 컨테이너가 필요하다.
서블릿은 JVM 기반에서 웹 개발을 하기 위한 명세.  
 JAVA EE에 포함된 스펙 중 하나로 Http 요청과 응답을 처리하기 위한 내용을 담고 있다.


빌드 도구인 MAVEN 혹은  GRADLE을 이용해서 서블릿을 이용할 개발환경을 설정 할 수 있다.
G-Api는 Maven 사용.

####빌드 도구 Maven VS Gradle

Maven 과 Gradle은 설정에 있어서 같은 convention을 사용 하나, Maven은 좀 더 rigid한 모델을 사용한다.
멀티 프로젝트의 경우 특정 설정을 다른 모듈에서 사용하려면 상속받아야하나 gradle은 주입 방식으로 해결한다.
또한 Maven은 pom이라는 xml 파일이 필요하다. 

### Servlet 내부 동작

서블릿의 생명주기 3단계
1. 초기화 : 로드한 서블릿의 인스턴스 생성, 리소스 로드
2. 서비스 : 클라이언트 요청에 따라 호출할 메서드를 결정하는 단계
3. 소멸 : 서블릿 언로드, 런타임 오류가 발생하거나 컨테이너가 종료되었을 때 발생
 

1. Init 메서드 & 초기화
- 서블릿을 만드는 추상 객체는 HttpServlet으로 이를 extend해서 서블릿을 생성할 수 있음.
```
@WebServlet("/init")
public class Servlet extends HttpServlet

```

cf. 서블릿 3.0 이후로 어노테이션으로 url을 매핑할 수 있게됨.
web.xml 예시는 이전 버전에서 url을 매핑하던 방식

