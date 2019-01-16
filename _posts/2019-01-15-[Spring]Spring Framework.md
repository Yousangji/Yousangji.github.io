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

스프링 핵심 모듈인 DataAccess/Web/Core을 포함해 SNS 연동에 유용한 Spring social, 메시징과 관련된 Spring AMQP, 빅데이터 처리를 위한 Spring XD 등
다양한 분야에 맞는 모듈이 있으며, 이렇게 많은 모듈들을 지속적으로 확장해 나갈 수 있다. 
스프링 프레임워크의 확장성은 Core Container를 기반으로 모듈간의 의존도를 최소화 했기 때문에 가능하다. 
의존도를 낮춘 방법 중 하나는 IOC 패턴을 활용한 것이다. 

[EJB VS POJO VS JAVABEAN](https://www.quora.com/What-is-the-difference-between-JavaBean-EJB-and-POJO)

####IOC 패턴

#####인터페이스

#####스프링 설정
스프링 컨테이너에 클래스를 등록하면 스프링이 클래스의 인스턴스를 관리한다. 이렇게 빈을 등록하고 설정하는 방법은 
XML/Annotation 두 가지가 있다. 

######XML 설정
######Annotation 설정


