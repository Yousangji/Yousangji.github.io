---
title: "Spring Boot Component"
date: 2019-01-25 18:26:28 -0400
layout: home
categories:
  - Spring
---
#### 스프링 부트란?
스프링 MVC로 웹서비스르 개발한다면 dependency 버전 관리, spring context 설정, Spring 기본 빈인 DispatcherServlet, view resolver, handler 등의 인을 구성해주어야 한다. 
즉, 스프링 부트는 스프링 개발에 필요한 서드파티 라이브러리, 빈구성 등을 관리해 빠르개 개발할수 있게 돕는 것이다.


####스프링 부트 모듈
스프링 부트는 크게 4가지 부분으로 구성되어있다. 


명칭 | 역할 | 비고
----|-----|----
AutoConfigurator | 설정 간소화 | 핵심컴포넌트
Starter | 스프링을 기반으로한 다양한 모듈을 총칭 | boot starter web
ClI | 스프링 부트로 만든 어플리케이션을 커맨드로 실행가능하게함 | spring run
Actuator | 스프링부트로 만든 어플리케이션을 모니터링 할 수 있게함 | 별도의 jar파일을 클래스 패스에 추가해야함.


#####@SpringBootApplication
spring-boot-autoconfigure.jar 에 포함되어있는 어노테이션으로 

> SpringBootApplication =  ComponentScan + configuration + EnableAutoConfiguration

~~~java
@Target({ElementType.TYPE})
       @Retention(RetentionPolicy.RUNTIME)
       @Documented
       @Inherited
       @SpringBootConfiguration
       @EnableAutoConfiguration
       @ComponentScan(
           excludeFilters = {@Filter(
           type = FilterType.CUSTOM,
           classes = {TypeExcludeFilter.class}
       ), @Filter(
           type = FilterType.CUSTOM,
           classes = {AutoConfigurationExcludeFilter.class}
       )}
       )
       public @interface SpringBootApplication {
           @AliasFor(
               annotation = EnableAutoConfiguration.class,
               attribute = "exclude"
           )
           Class<?>[] exclude() default {};
       
           @AliasFor(
               annotation = EnableAutoConfiguration.class,
               attribute = "excludeName"
           )
           String[] excludeName() default {};
       
           @AliasFor(
               annotation = ComponentScan.class,
               attribute = "basePackages"
           )
           String[] scanBasePackages() default {};
       
           @AliasFor(
               annotation = ComponentScan.class,
               attribute = "basePackageClasses"
           )
           Class<?>[] scanBasePackageClasses() default {};
       }
~~~

~~~java

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
~~~

#####@AutoConfigurationPackage

해당 어노테이션이 선언된 패키지 경로를 스프링 콘텍스트가 스캔가능하도록 하는 역할을 한다. 이 어노테이션은 @AutoConfiguration 을 임포트하는데 이 클래스가 실제로 패키지 정보를 등록하는 역할을 한다. 

