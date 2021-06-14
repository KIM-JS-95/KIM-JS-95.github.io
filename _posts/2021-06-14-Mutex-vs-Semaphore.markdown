---
layout: post
title: "Mutex VS Semaphore"
date: 2021-06-14
tags: CS
color: rgb(98,170,255)
author: KIM-JS-95
subtitle: 'what is different between Mutex and Semaphore'
---

## Mutex VS Semaphore

### Back - Ground
**Thread-safe**의 오류를 보완하기 위해 임예영역(Critical Section)을 동기화 기법으로 제어해주어야한다.
여기서 동기화 기법으로는 Mutex 와 Semaphore 기법이 있다.

> Thread-safe : 공유자원에 접근할때 의도한 대로 동작하는 것

### 동기화 객제의 종류
* Thread 동기화 방법
  * 실행 순서 동기화
  * 메모리 접근에 대한 동기화
  * Memory 접근에 있어 동시 접근을 방지
    * 실행 순서에 상관 없이, 한순간에 한가지의 스레드만 접근하도록 구성 


* 동기화 기법의 종류
  
  |  | 유저모드 | 커널모드 |
  |:---:|:---:|:---:|
  | 성능 | 향상 | 저하 |
  | 기능 | 저하 | 향상 |

### Common
Mutex 와 Semaphore 는 공유 자원에 접근하는 것을 제어하기 위한 병렬 처리를 위한 프로세스 동기화 기법이다.

---

### Mutex (상호배제)
**Critical Section**을 가진 <u>Thread</u> 들의 Running Time이 서로 겹치지 않게, 각각 단독으로 실행되게 하는 기술이다.
다중 프로세스들의 공유 리소스에 대한 접근을 튜닝하기위해 Synchronized 또는 lock 을 사용한다.

> Critical Section : 프로그램 상에서 동시에 실행될 경우 문제을 일으킬 수 있는 부분.

정리하면, Multi-Tread에서 공유자원에 동시에 접근하는 것을 방지하는 기술이다.

---

### Semaphore
공유된 자원의 데이터를 여러 <u>Process</u>가 접근하는 과정을 방지하는 기술이다.

동시에, 세마포어는 운영체제 또는 커널의 한 지정된 저장장치 내 값으로서, 각 프로세스는 이를 확인하고 변경할 수 있다.

공유 리소스에 접근할 수 있는 프로세스의 최대 허용치만큼 동시에 사용자가 접근하여 사용할 수 있다.

* 종류

    * 카운팅 세마포어(counting semaphore) : 세마포어의 초기값이 0 이상의 수이며 범위는 정해져 있지 않다.
  
    * 이진 세마포어((binary semaphore) : 초기 값이 0 또는 1만 가질 수 있는 세마포어, 카운팅 세마포어보다 구현이 간단


### Mutex VS Semaphore

|  | Mutex | Semaphore |
|:---:|:---:|:---:|
| 동기화 대상의 개수 | 동기화 대상이 <u>오직 하나</u> | 동기화 대상이 <u>하나 이상</u> |
| 포함 | Mutex 는 상태가 두개(0, 1) 인 Binary Semaphore 이다. |  |
| 책임 | 소유가 가능(책임을 가짐) | 소유 불가능 | 
| 해제 | 소유하고 있는 스레드가 Mutex 해제  | 소유하지 않는 스레드가 Semaphore 해제 |
| 범위 | 프로세스 범위를 가지며 프로세스가 종료될 때 자동 <u>Clean-up</u> |  시스템 범위에 걸쳐 있으며 파일 시스템상의 <u>파일 형태</u>로 존재 |

### 🧾 Reference
* [Mutex와 Semaphore 란?](https://artwook.tistory.com/17)
* [Semaphore](https://ko.wikipedia.org/wiki/%EC%84%B8%EB%A7%88%ED%8F%AC%EC%96%B4)
