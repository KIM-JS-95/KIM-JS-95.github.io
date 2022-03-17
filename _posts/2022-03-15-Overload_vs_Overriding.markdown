---
layout: post 
title:  "Overload VS Overriding"
date:   2022-03-16 12:05:21 +0800 
tags: 면접 자바 다형성
color: rgb(98,170,255)
subtitle: 오버로딩 ? 오버라이딩?
--- 

## 다형성에서 빠질 수 없는 개념

추상화는 **어떠한 객체에 대한 공통 요소를 추출하는 과정** 으로 배웠다.

그러나 구체적인 행위를 구현하는 것이 아닌 메소드의 이름만 명시하고 하위 클래스에서 그 행위를 구현하도록 `JAVA`는 약속했다.

이렇게 행위를 구현화 하는데 필요한 절차가 `오버로딩` , `오버라이딩` 이라고 하는 것이다.


### 🌠 오버라이딩

> Ride : (말, 자동차 등에) 타다

`오버라이딩`에 대해서는 상속과 추상화를 배웠다면 익숙한 어노테이션 일것이다.

상위 클라스의 추상메소드를 하위 클라스에서 `구체화`하는 과정으로 이해하면 충분하다 생각한다.

```java
abstract class Animal {
    void sound() {}
}

class Dog extends Animal {

    @Override
    public void sound() {
        System.out.println("이름을 알려주지 않아 공격적인 성향");
    }
    
}

class freeNode {
    public static void main(String[] args) {
        Dog dog = new Dog();
        
        dog.sound();
    }
}
```

### 🌠 오버로딩


만일 같은 이름의 추상 메소드라도 <u>입력되는 인자에 따라 행위의 결과가 다르다면 어떻게 해야할까?</u>


> Load : 물건을 싣다.

단어의 관점에서 바라본다면 **추상 메소드에 물건을 적제한다** 
라는 의미를 가지며 여기서 물건을 `메소드 인자` 라고 생각하면 쉬울 것이다.


- 오버로딩 예시

```java
abstract class Animal {
    void sound() {}
}

class Dog extends Animal {

    @Override
    public void sound() {
        System.out.println("이름을 알려주지 않아 공격적인 성향");
    }

    //TODO: 오버로딩 : guest 인자를 넣어주었다.
    public void sound(String guest) {
        System.out.println("강아지가 " + guest + " 를 " + "반가워한다.");
    }
}

class freeNode {
    public static void main(String[] args) {
        Dog dog = new Dog();

        dog.sound();
        
        //TODO: 이름을 알려주었다.
        dog.sound("Michle");
    }
}
```

추상 메소드의 이름을 한번 더 사용하고 싶지만 `오버라이딩` 할 경우 **중복된 메소드 사용으로 에러**가 발생하며

추상 메소드에 인자를 주입하여 좀더 다양한 결과를 반환하고 싶은 개발자를 위해 만들어진 개념이다.

추상 메소드는 변함이 없지만 하위 클래스에서 기존 추상 메소드에 인자를 주입하는 것으로 `오버로딩` 을 사용할 수 있는 것이다.



## 🧾 Reference
- [깃헙 - 스프링 입문 교재](https://github.com/expert0226/oopinspring)
- [책 - 스프링을 입문을 위한 자바 객체 지향의 원리와 이해](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=55641908)


