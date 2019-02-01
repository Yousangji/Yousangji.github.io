---
title: "Spring Framework"
date: 2019-01-15 18:26:28 -0400
layout: home
categories:
  - Spring
---

###스프링
>스프링은 객체 관리를 해주는 빈 컨테이너 프레임워크다.

서버에서 각자 자바 객체를 관리하기 어려우므로 웹프로젝트에서 자바 빈(객체)들을 관리해줄 컨테이너가 필요했고 그 시작은 EJB이다.
이후 개발 요구사항이 많아지면서 경량화되고 간소한 컨테이너가 필요하게 되었다.
마침내 컨테이너와 상관없이 독립적으로 빈의 생명주기를 관리할 수 있는 프레임워크를 만드는데 그것이 스프링 프레임워크다.

####스프링 모듈
![스프링 모듈](../_image/spring-overview.png)

스프링 코어 켄테이너는 스프링 프레임워크  의존성 주입, IOC 컨테이너 등 어플리케이션 컨텍스트의 핵심기능을 제공한다. 

모듈 | 사용
---|---|
`spring-core` | 다른 스프링 모듈들이 사용하는 코어 유틸리티
`spring-beans` | 스프링 빈지원. 스프링- 코어와 함께 스프링 프레임워크의 핵심기능인 의존성 주입 제공, 빈팩토리 구현을 포함하고 있음
`spring-context` | 빈 팩토리를 상속하는 어플리케이션 콘텍스트를 구현하고 리소스 로드 및 국제화 지원을 제공
`spring-expression` | EL을 확장하고 빈 속성 및 접근 처리를 위한 언어를 제공

횡단 관심
로깅과 보안과 같은 모들 어플리케이션 레이어에 적용할 수 있다. 단위테스트, 통합테스트는 모든 레이어에 적용할 수 있으므로 이 카테고리에 적합하다. 

모듈 | 사용
---| ---|
`spring-aop` | 메서드 인터셉터와 포인트 컷을 사용해 관점 지향 프로그래밍에 대한 기본적인 지원 제공
`spring-aspect` | 가장 인기있는 AOP 프레임워크인 AspectJ와의 통합을 제공
`spring-instrument` | 기본적인 인스트루멘테이션을 제공
`spring-test` | 단위 및 통합 테스팅에 대한 기본 지원제공


스프링 핵심 모듈 DataAccess/Web/Core을 포함해 SNS 연동에 유용한 Spring social, 메시징과 관련된 Spring AMQP, 빅데이터 처리를 위한 Spring XD 등
다양한 분야에 맞는 모듈이 있으며, 이렇게 많은 모듈들을 지속적으로 확장해 나갈 수 있다. 
스프링 프레임워크의 확장성은 Core Container를 기반으로 모듈간의 의존도를 최소화 했기 때문에 가능하다. 
의존도를 낮춘 방법 중 하나는 IOC 패턴을 활용한 것이다. 

[EJB VS POJO VS JAVABEAN](https://www.quora.com/What-is-the-difference-between-JavaBean-EJB-and-POJO)

####IOC 패턴
의존성 주입

스프링에서 객체 생성 및 연결의 책임이 Ioc Container라는 새로운 구성요소로 인계되었다. 클래스는 의존성을 정의하고, Ioc Container는 객체를 만들고 의존성을 연결한다. 
의존성 생성 및 와이어링 제어가 컨테이너에의해 수행되는 기능을 의존성 주입이라한다. 
이런 의존성 주입의 중요한 이점은 쉬운 유지 관리성, 결합력 감소 및 테스트 가능성 개선이 있다.

#####인터페이스

~~~java
public class UserServiceImpl{
    private DataService dataService;
    
    public long sumValueOfUserData(User user){
        long sum = 0;
        for(Data data : dataService.retrieveDataList(user){
            sum += data.getValue();
        }
    }
}
~~~
UserServiceImpl이 작동하기 위해선 DataService가 필요하고 의존성이 있다고 명명한다.

UserServiceImpl은 DataService를 생성해야하는데 인터페이스 없이 인스턴스 자체를 생성하는 방법은 단단한 결합이다.
~~~java
DataServiceImpl dataService = new DataServiceImpl();
~~~

 다른 방법으로, 클래스 인터페이스를 사용하는 느슨하게 결합된 방법도 있다.
  인터페이스를 구현하는 모든 와이어링을 잘 정의된 의존성으로 교체할 수 있다. 즉 변경이 필요할 때 코드 자체를 변경하지 않고 와이어링만을 변경할 수 있다.  
~~~
public interface DataService{
   List<Data> retrieveDataList(User user);
}
~~~

#####Ioc Container
의존성 주입에서 언급했듯이, Ioc Container는 객체를 만들고 와이어링을 담당한다. 
여러빈 중에서 어떤 빈을 생성해야하는지 어노테이션을 통해 판단하며,
 UserServiceImpl에 DataServiceImpl를 연결시키위해선 @Autowired 를 통해 연결할 것을 정의해주어야한다.
 

#####스프링 설정
스프링 컨테이너에 클래스를 등록하면 스프링이 클래스의 인스턴스를 관리한다. 이렇게 빈을 등록하고 설정하는 방법은 
XML/Annotation 두 가지가 있다. 

######XML 설정
######Annotation 설정

#### 스프링 MVC
Front Controller 패턴에 Spring의 의존성 주입을 이용해 컴포넌트의 생명주기를 관리하는 웹 MVC 프레임워

