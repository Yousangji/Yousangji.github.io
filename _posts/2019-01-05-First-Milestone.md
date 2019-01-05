---
title: "First Milestone"
date: 2019-01-05 13:26:28 -0400
categories: jekyll update
---
####BackEnd 공부 방향 

 - 웹 생태계의 스펙
   - HTML, HTTP(1.1, HTTP/2)  
 - 기본 SDK, 라이브러리/프레임워크 이해와 활용
 - 클라이언트 API 설계
 - 서버/컴퍼넌트/객체 간의 역할 분담/의존성/통신 방법 설계
 - 저장소 활용
    - DBMS 설계
    - Cache 적용
      - Global/Local cache 적용범위, 라이프 싸이클, 솔루션 
    - 파일 저장 정책/솔루션 선택 활용
 - 검색엔진 연동 방식 결정
 - 빌드 도구
 - 배포 전략
 - 성능 테스트/프로파일링/튜닝
    - JVM 레벨 튜닝(GC 옵션)
        - 웹 서버 설정/튜닝
        - OS 설정 주요값확인
 
 #####DB 쿼리 스타일의 변화
 : 데이터를 조회하는 SQL이 단순할수록 데이터를 다른 저장소에 캐시하거나 분산해서 저장하기가 쉬워집니다.
   대용량을 저장하는 UGC 서비스에서는 RDB 테이블 사이의 JOIN은 최대한 제약을 하고 어플리케이션 레벨에서 여러 저장소의 연관된 데이터를 조합하기도 합니다.
   
 #####병렬처리
Servlet기반의 Java웹서버들은 기본적으로 사용자의 요청을 병렬적으로 처리합니다. 그래서 객체가 멀티스레드에서 공유되는 것인지, 아닌지를 의식하는 일은 중요합니다. 클래스의 멤버변수에는 항상 멀티스레드에서 접근해도 안전한(Thread-safe)한 변수만 두면 된다는 단순한 규칙만으로도 많은 문제를 예방할 수 있습니다. 그런데 Local cache를 적용할 때는 멀티스레드에서 공유된 객체가 쉽게 눈에 안 띌 수도 있습니다. 그래서 Cache대상의 객체는 Immutable하게 유지하는 것이 안전합니다. 직접 Cache 모듈을 만드는 경우 위험한 버그가 생기는 경우가 많습니다. 하나의 메모리 누수를 잡기까지 도 그런 사례입니다.

사용자 요청을 처리하는 스레드 외에도 별도의 스레드 풀로 실행해야 할 작업이 종종 생기기도 합니다. 예를 들면 사용자에게 주는 응답에 영향을 주지는 않지만 실행시간이 길어질 수도 있는 update 구문을 DB에 실행하는 작업같은 것입니다. 그런 경우 Java에서는 Executors, ThreadPoolExecutor에 있는 많은 옵션들이 정교한 제어를 하는데 도움이 됩니다. new Thread()로 직접 스레드를 생성하는 방식은 JDK5 이후로는 권장하지 않습니다.
  [병렬처리](https://d2.naver.com/helloworld/1326256)
  
##### 보안

XSS, CSRF, SQL Injection

##### 추가 소스
[serverless 관련 글](https://martinfowler.com/bliki/Serverless.html)
[빌드 서버 가상화 사례](https://martinfowler.com/bliki/Serverless.html)
[HashMap](https://d2.naver.com/helloworld/831311)
[Line Architecture](https://d2.naver.com/helloworld/809802)





