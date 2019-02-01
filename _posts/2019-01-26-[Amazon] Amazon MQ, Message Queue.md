

#### Message Queuing 
이메일 송수신 관련 처리를 이메일 봇이 담당하고 있고 DB나 비지니스 로직 등은 모두 Api 에서 처리함.
이메일 송수신은 이메일봇이 담당하지만 송수신 내용을 저장하는 것은 API 가 담당하다보니 한 트랜젝션에서 서로 엇갈린 요청을 하는 구조
각자 처리가 완료되기 전까지 서로를 물고 있는 세미 데드락 상황 발생.
현재는 요청이 많지 않아 문제가 없으나 이후 요청이 많아지면 문제가 발생할 수 있어 개선 필요.

서로 물고있는 요청이 response를 필요로 하지 않는 시그널 요청이기 때문에 메시징 브로커를 통해 EDA로 전환해 병목현상이 없는 구조을 고려.

메시지 큐잉
비동기 커뮤니케이션
느슨하게 연결된 모듈/ 프로세스

프로토콜 비교

###AMQP

표준 MQ protocol로 App 사이의 Message 를 전달할 때, MESSAGE를 어떻게 Queuing하고 Routing할지 정의한다.
다양한 Message 전달옵션을 정의하고 있어 언어 종속성, 환경 종속성이 없다. 
Tcp/ip

메시지는 Exchange로 전달되고 queue에서 소비된다.
1.1. Exchange
Message Routing & Filtering
4가지 모드를 지원한다.
- Fanout : 브로커에 존재하는 모든 큐에 메시지 전달  
- Direct : 지정된 routingkey를 가진 특정 큐에 메시지 전달
- Topic : 패턴 바인딩 형태에 일치하는 다수 큐에 메시지 전달
- Header : Message Header에 포함된 KEY 값에 따라서 다수의 queue에게 전달

1.2. Queue
전달될 Message를 임시로 저장하는 곳

*메시지 분배 (Round-robin dispatching)*
RabbitMQ는 Consumer가 병렬처리를 쉽게 할 수 있도록 같은 Queue를 바라보고 있는 Consumer에게 메시지를 균등 분배한다.

즉 첫번째 메시지는 Consumer1에게 두번째 메시지는 Consumer2에게 분배(중복 처리하지 않도록 1명에게만 줌)

MQ의 존재 이유를 보여주는 매우 중요한 개념이다. 이로 인해 메시지를 받아 처리하는 프로그램들은 수평 확장이 가능하다. 물론 Producer도 수평 확장이 가능하다. (같은 Queue에 메시지를 던져주기만 하면 됨)

*Fair dispatch(공평한 분배)*
여러 consumer에게 round robin할 때 번갈아가면서 메시지를 전달하지만 완전히 공평하진 않음 (매홀수는 데이터크기가 크고, 매짝수는 데이터크기가 작은 등)

때문에 busy한 서버에게 메시지를 계속 전달하지 않도록 prefetchCount라는 개념사용. prefetchCount가 1일때는 아직 ack를 받지 못한 메시지가 1개라도 있으면 다시 그 consumer에게 메시지 할당하지 않음. 즉 prefetchCount는 동시에 보내는 메시지 양임


*메시지 수신 통보 (Acknowledgment)*
많은 프로토콜들이 메시지 전달 보장을 위해 Acknowlegment(ACK)라는 개념을 사용하여 메시지에 대한 응답을 보내주도록 되어있다. MQTT의 경우는 QoS(Quality of Service) level이라고해서 0=안보냄, 1=전달완료확인, 2=최종목적지까지 처리완료 확인의 개념이 있는데 RabbitMQ의 경우는 Acknowledgment(Consumer전달확인)와 Confirm(Publish전달확인)을 이용한 level 1만 지원하는 듯 하다.(추정)
ACK가 중요한 이유는 Queue는 Consumer에게 데이터를 전달하고나면 Queue에서 메시지를 삭제하므로, Consumer가 전달받았지만 처리도중 오류가 발생하여 재처리해야하는 경우를 위해 보관 유예기간을 두는 용도로 이용된다. 즉 ACK가 온 메시지만 삭제처리하도록 하는 것이다.

※ 주의 basicQos라는 설정은 prefetchCount의 숫자 설정임(!! QoS level 0,1,2의 개념 아님!!)

여러 Client(Consumer) 중 일부가 죽었을시 대응방법
앞서 설명했듯이 Consumer가 ACK를 보내지 않으면 Queue에 쌓인 메시지는 지워지지 않고 남아있다. 메시지를 받은 Consumer가 느려서 아직 Processing 중일 수 있어서 다른 Consumer에게 Round Robin하여 재전송 하자니 중복처리 우려가 있고, 그대로 Unacknowledged 상태로 마냥 처리 안된채로 남겨 둘 수도 없고 곤란하다.

이 경우에 어떻게 동작하는지 확인해볼 필요가 있다.

- 다른 환경에서 함께 사용하기 좋은 프로토콜로 언어 종속성이 없는 것이 장점.
- RabbitMQ에서 사용하는 프로토콜, ActiveMQ에서도 NATIVE 지원

특징
- 비동기화 => 유연하게 사용가능
- 이벤트 기반
- 손쉽게 변경
- 확장성
- 탄력성

표준 MQ protocol로 APP 사이의 Message를 전달할 때 

###MQTT
IoT와 같이 부족한 Resource 환경에서 이용되는 PUB/sub 기반 messaging protocol.
Topic을 기준으로 동작한다. pulisher가 특정 topic으로 메시지를 브로커에 전달하면 브로커는 topic을 구독하는 모든 SUbscriber에게 메시지를 전달.
따라서 AMQP와 달리 Multicast 동작 수행

참조.
[RabbitMQ_Amqp-concept](https://www.rabbitmq.com/tutorials/amqp-concepts.html)
[AMQP_MQTT](https://ssup2.github.io/theory_analysis/AMQP_MQTT/)
[](https://www.slideshare.net/javierarilos/rabbitmq-intromsgingpatterns)
[rabbitMQ](http://gjchoi.github.io/rabbit/rabbit-mq-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/)
- 원격 측정 데이터를 전송하기 위해 설계대역폭과 배터리를 효율적으로 사용(성능이 낮은 기계에서도 사용가능하도록)

Apache ActiveMQ
- java 기반 오픈소스 메시지 브로커

기능
- 일시적 메시징 in-memory 속도 , 지속메시징 지원
- 큐 토픽 FIFO 지원
- 합성&가상 목적지


MOM 비교


