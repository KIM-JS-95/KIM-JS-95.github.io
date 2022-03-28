---
layout: post 
title:  "@Autowire vs @Resource"
date:   2022-03-28 12:05:21 +0800 
tags: 스프링
color: rgb(98,170,255)
subtitle: @Autowire / @Resource 차이는?
---

## @Autowire 사용과 @Resource의 사용

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd">

	<context:annotation-config />

	<bean id="tire" class="expert004.KoreaTire"></bean>

	<bean id="wheel" class="expert004.AmericaTire"></bean>

	<bean id="car" class="expert004.Car"></bean>
</beans>
```

```java
public class Car {
    // TODO: 속성 자동 주입
	@Autowired
	Tire tire;

	public String getTireBrand() {
		return "장착된 타이어: " + tire.getBrand();
	}
}

public class Car {
    // TODO: 속성 자동 주입
    @Resource
    Tire tire;

    public String getTireBrand() {
        return "장착된 타이어: " + tire.getBrand();
    }
}
```

위 코드를 본다면 차이가 보이지 않는다. **단, 어노테이션만 다를 뿐이다.**

||@Autowire|@Resource|
|:---:|:---:|:---:|
|출처|스프링 프레임워크|표준 자바|
|소속 패키지|org.springframework.beans.factory.annotation.Autowired|import javax.annotation.Resource|
|빈 검색|byType 먼저, 못찾으면 byName| byName 먼저, 못찾으면 byType|
|특이사항|@Qualifire("")|name 어트리뷰트|
|byName 강제하기|@Autowire <br> @Qualifire("tire1")|@Resource(name = "tire1")|



## 🧾 Reference
- [책 - 스프링을 입문을 위한 자바 객체 지향의 원리와 이해](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=55641908)
- [한 번에 끝내는 Java/Spring 웹 개발 마스터 초격차 패키지 Online.]()
