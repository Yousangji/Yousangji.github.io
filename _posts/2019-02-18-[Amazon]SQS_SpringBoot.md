---
title: "[Amazon] SQS-SpringBoot"
date: 2019-01-25 18:26:28 -0400
layout: home
categories:
  - Spring
---

#### Message Queuing 
이메일 송수신 관련 처리를 이메일 봇이 담당하고 있고 DB나 비지니스 로직 등은 모두 Api 에서 처리하고 있다. 
이메일 송수신은 이메일봇이 담당하지만 송수신 내용을 저장하는 것은 API 가 담당하다보니 한 트랜젝션에서 서로 엇갈린 요청을 하는 구조를 취하고 있는데, 이로 인해
각자 처리가 완료되기 전까지 서로를 물고 있는 세미 데드락 상황이 발생한다.

속도가 느린 문제가 있어 멀티 쓰레드로 비동기처리했지만 이도 컨텍스트 스위칭 비용이 발생할 것이다.  
현재는 요청이 많지 않아 문제가 없으나 이후 동시 요청이 많아졌을때 감당할 수 있는 구조가 아니라 개선이 필요하다고 생각했다.

서로 물고있는 요청이 사실 response를 기다릴 필요가 없는 단순 시그널 요청이기 때문에 메시징 브로커를 통해 병목현상이 없는 구조을 고려했고 메시지 큐 구조에 대해 알아봤다.

>"Loosely coupled systems"
the looser they are coupled,
the bigger they will scale,
the more fault tolerant they will be,
the less dependencies they will have,
the faster you will innovate.


#### AWS SQS란? 
Rabbit MQ, AWS MQ(Active MQ) 등을 고려했지만 현재 상황과 과금 방식으로 인해 request 당 비용이 발생하는 SQS를 적용하기로 했다. 

메시지 순서 보장없음
최소 