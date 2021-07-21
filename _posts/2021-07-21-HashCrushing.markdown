---
layout: post
title:  "spring 보안 로그인 폼"
date:   2021-07-21 14:05:21 +0800
tags: SpringBoot Security
color:  rgb(154,133,255)
subtitle: '암호화 충돌'
---

## 서론

컴퓨터는 데이터를 복잡하게 암호화 하여 값을 저장하고 암호화된 데이터를 가져오는 과정에서 키(Key) 값을 통해 회수하게 된다. 
암호화된 복잡한 데이터를 호출하기 위해 키값을 생성하여 테이블에 저장하게 되는데 이를 해싱(Hashing) 과정 이라고 하며
Hasing 는 크게 Hash-function , Hash-Table 요소로 구분할 수 있다.


### Hash-function

해시 함수에서 중요한 것은 고유한 인덱스 값을 설정하는 것이다.

 해시함수 방식

* 제산법

제산(Division)법은 레코드키로 해시표의 크기보다 큰 수 중에서 가장 작은소수로 나눈 나머지를 홈 주소로 삼는 방식이다.

H(Key) = Key % Prime_Number

* 제곱법

제곱법은 레코드키 값을 제곱한 후 그 중간 부분의 값을 홈주소로 삼는 방식이다. 

(Key^2)

* 폴딩법

폴딩(Folding)법은 레코드키값을 여러부분으로 나눈 후 각 부분의 값을 **더하거나 XOR(배타적 논리합)**한 값을 홈주소로 삼는 방식이다.

* 기수변환법

기수변환법은 키 숫자의 진수를 다른 진수로 변환시켜 주소 크기를 초과한 높은 자릿수를 절단하고, 이를 다시 주소 범위에 맞게 조정하는 방법이다.

* 대수적 코딩법

대수적코딩법은 키 값을 이루고 있는 각 자리의 비트 수를 한 다항식의 계수로 간주하고, 이 다항식을 해시표의 크기에 의해 정의된 다항식으로 나누어 얻은 나머지 다항식의 계수를 홈 주소로 삼는 방식이다.

* 계수 분석법(숫자 분석법)

계수 분석법은 키 값을 이루는 **숫자의 분포를 분석**하여 비교적 고른 자리를 필요한 만큼 택해서 홈 주소로 삼는 방식이다.

* 무작위법

무작위(Random)법은 난수(Random Number)를 발생시켜 나온 값을 홈 주소로 삼는 방식이다.



### Hash-Table

Hash-Table은 Key값과 Value 값을 1대1로 저장하여 관리하는 자료구조의 방식으로 Value 값은 중복 저장이 가능하지만 <u>Key 값은 중복이 허용되지 않는다.</u>


### 해시 충돌(Hash Collision)

* 충돌 원인
두 요소의 Hash-function의 결과가 동일한 Key 값을 가지는 경우가 발생하는 것이 원인.
  
* 해결 방안

* Open Addressing(Open Hashing)
  
  개방 주소법의 경우에는 다르다. 해시 충돌이 일어나면 다른 버켓에 데이터를 삽입하는 방식을 개방 주소법이라고 한다.

    * 선형 조사 : 기존 주소에 데이터가 존재하면 다음 위치에 저장
    * 이차원 조사 : 충돌 발생시 다음 함수로 결정
      > H(x) = (h(x) + C1*i^2 + C2*i) mod (m) 
    * 더블 해싱
    * 무작위 처리


* Seperate Chaning(Closing Hashing)
  
  버켓 내에 연결리스트(Linked List)를 할당하여, 버켓에 데이터를 삽입하다가 해시 충돌이 발생하면 연결리스트로 데이터들을 연결하는 방식이다.
  체이닝의 경우 버켓이 꽉 차더라도 연결리스트로 계속 늘려가기에, <u>데이터의 주소값은 바뀌지 않는다.</u>

  Sandra Dee라는 사람의 연락처를 삽입할 때, 충돌이 일어나니 버켓 내에서 연결리스트로 데이터를 연결하는 것을 확인할 수 있다.

![!](https://t1.daumcdn.net/cfile/tistory/2525963E580F616926)
  

### Open Addressing(Open Hashing) vs Seperate-Chaning(Closing Hashing)

◎ 체이닝(Chaining)의 장점

→ 연결 리스트만 사용하면 된다. 즉, 복잡한 계산식을 사용할 필요가 개방주소법에 비해 적다.

→ 해시테이블이 채워질수록, Lookup 성능저하가 Linear하게 발생한다.

---

◎ 개방주소법(Open Addressing)의 장점

→ 체이닝처럼 포인터가 필요없고, 지정한 메모리 외 추가적인 저장공간도 필요없다.

→ 삽입,삭제시 오버헤드가 적다.

→ 저장할 데이터가 적을 때 더 유리하다.

![비교](https://t1.daumcdn.net/cfile/tistory/25484F43581421980E)


### 🧾Reference
1. [Hash-function](https://mangkyu.tistory.com/102)
2. [Open Addressing vs Seperate-Chaning](https://preamtree.tistory.com/20)

