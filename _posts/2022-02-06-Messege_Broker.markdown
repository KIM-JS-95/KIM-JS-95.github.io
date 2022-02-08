---
layout: post
title:  "메시지 브로커?"
date:   2022-01-31 12:05:21 +0800
tags: Spring-Cloud
color: rgb(154,133,255)
subtitle: Rabbit MQ 와 Kafka
--- 

## 🚀 메세지 큐

사용자의 입력을 메시지로 전달하는 시스템에서 어떤 프로세스에 대한 메지시를 저장하기 위해 할당된 큐

### 장점

- 비동기(Asynchronous): Queue에 넣기 때문에 나중에 처리할 수 있습니다.
- 비동조(Decoupling): Appliction 과 분리할 수 있습니다.
- 탄력성(Resilience): 일부가 실패 시 전체에 영향을 받지 않습니다.
- 과잉(Redundancy): 실패할 경우 재실행 가능합니다.
- 보증(Guarantees): 작업이 처리된걸 확인할 수 있습니다.
- 확장성(Scalable): 다수의 프로세스들이 큐에 메시지를 보낼 수 있습니다.

## 🚀 메세지 브로커

생산자에 의한 요청(메세지)를 Queue에 넣어 순차적 처리하는 방식으로 데이터 통신에서 발생하는 `병목현상`을 줄일 수 있다.

### 🍕 AMQP(Advenced Message Queing Protocol)

![](https://media.vlpt.us/images/gjrjr4545/post/65d9f647-a9b5-4d51-9906-a15020e53b72/amqp.png)

인스턴스가 데이터를 서로 교환할 때 사용하는 방법으로
메세지 지향 미드웨어를 위한 개방형 표준 응용계층 프로토콜이다.

즉, 이기종간 메세지 교환의 문제점을 해결하기 위해 등장한 프로토콜이다.

#### 🍔특징

* 모든 broker들은 똑같은 방식으로 동작할것
* 모든 client들은 같은 방식으로 동작할것
* 네트워크상으로 전송되는 명령어들의 표준화
* 프로그래밍 언어 중립적

#### 🍔 AMQP Standard Exchange Type

Client어플리케이션과 middleware broker와의 메시지를 주고받기 위한 프로토콜

- Direct Exchange

메세지의 라우팅 키를 큐에 1:1으로 매칭 시키는 방법
라운드 로빈 방으로 여러 Consumer간 task 분리

- Topic Exchange

  - 와일드카드를 이용해서 메세지를 큐에 매칭시키는 방법
  - MultiCast 방식에 적합
  - 라우팅 키는 ","으로 구분된 0개 이상의 단어의 집합으로 간주 되고 와일드카드 문자들을 이용해 특정 큐에 바인딩
  - '*' : 하나의 단어
  - '#' : 0개 이상의 단어


- Fanout Exchange

  - 모든 메세지를 모든 큐로 라우팅 하는 유형
  - 1:N 방식으로 메세지를 브로드캐스트 하는 용도로 사용


- Headers Exchange

  - key-value로 정의된 헤더에 의해 라우팅을 결정
  - 큐를 바인딩 할때 x-match라는 특별한 argument로 헤더를 어떤식으로 해석하고 바인딩 할지를 결정
    - all : 모두 충족 시켜야 함 (and)
    - any : 하나만 충족시키면 된다. (OR)
    

### 🍕 AMQP 종류
 현재 가장 유명한 두가지 `Apache Kafka` 과 `Rabbit MQ`을 이야기 한다.

#### 🍕 Rabbit MQ

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtzNQ6%2FbtqE32Ew22M%2FIoTTgECRYbDpUZZJFq0oAk%2Fimg.png)

> “open source distributed message broker”

비동기식 메세지 관리로 사용자로부터 수집한 데이터를 `Queue`에 넣고 필요시 꺼내 쓰는 방식

- 높은 신뢰성 / 안정성
- 유연한 라우팅
- 클러스터링(로컬 네트워크에 있는 여러 `Rabbit MQ`서버를 논리적으로 클러스터링 가능)
- UI 모니터링 편리성

#### Apache Kafka

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd2nDtD%2FbtqE4pzsyPZ%2FTcw9I6z3d7eHjeQ7uaTqa0%2Fimg.png)

> “distributed event streaming platform”

아파치 재단이 스칼라로 개발한 `오픈 소스 메세지 브로커`이며 실시간 데이터 피드를 관리하기 위해 통일된, 높은 처리량, 
낮은 지연시간을 지닌 플랫폼을 제공하는 것이 목표이다.

- 대용량 실시간 로그 처리 특성화

- AMQP 프로토콜이나 JSM API를 사용하지 않고 단순한 메세지 헤더를 지닌 TCP 기반 프로토콜을 사용하므로서 
오버헤드가 비교적 작다.

- 노드 장애에 대한 대응성
- 메세지를 기본적으로 파일 시스템에 저장하여 별도의 설정을 하지 않아도 오류 발생시 **오류 지점부터 복구가 가능**
- 메시지를 파일 시스템에 저장하기 떄문에 메세지가 많이 쌓여도 기존 메세징 **시스템에 비해 성능이 크게 감소하지 않는다.**

### 🍕 Rabbit MQ vs Kafka 차이

- RabbitMQ는 메시지 브로커 방식이고 Kafka는 pub/sub 방식입니다.
- Kafka는 TCP_IP 기반으로 이벤트 발생시 파일로 저장됨
- Rabbit MQ는 이벤트 로그가 메모리로 남기 때문에 저장되어 지지않는다.


#### Kafka가 적절한 곳

Kafka는 복잡한 라우팅에 의존하지 않고 최대 처리량으로 스트리밍하는 데 가장 적합하며
또한 이벤트 소싱, 스트림 처리 및 일련의 이벤트로 시스템에 대한 모델링 변경을 수행하는 데 이상적이다.

<u>즉, 스트리밍 데이터를 저장, 읽기, 다시 읽기 및 분석하는 프레임워크가 필요한 경우 Kafka가 적합하다.</u>

또한 모니터링이 가능하기 때문에 정기적으로 감사하는 시스템이나 메시지를 영구적으로 저장하는 데 이상적이다.

#### RabbitMQ가 적절한 곳

복잡한 라우팅 서비스를 구축할 경우 RabbitMQ를 사용한다. 
RabbitMQ는 신속한 요청-응답이 필요한 웹 서버에 적합하며 또한 부하가 높은 작업자 간에 부하를 공유합니다. 
<u>즉, 장시간 실행되는 작업, 안정적인 백그라운드 작업 실행, 애플리케이션 간 내부 통신 및 통합이 필요할때 적합하다.</u>

### 🧾Reference
[Rabbit MQ vs Kafka 차이](https://coding-nyan.tistory.com/129)

[Kafka, RabbitMQ 개념 및 비교](https://armful-log.tistory.com/61)