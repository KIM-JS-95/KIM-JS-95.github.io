---
layout: post
title:  "SpringBootServletInitializer 상속"
date:   2021-06-21 11:00:21 +0800
tags: SpringBoot 
color: rgb(154,133,255)
subtitle: ''
---

###  SpringBootServletInitializer

## 목적
SpringBoot 웹 애플리케이션을 배포할 때는 주로 embedded tomcat이 내장된 jar파일을 이용한다. 
하지만 특별한 경우에는 전통적인 배포 방식인 **war 파일**로 배포를 진행해야 하는 경우가 있을 수 있다. 
이럴 경우 **SpringBootServletInitializer**를 상속받으면 된다. 

```bash
@SpringBootApplication
public class SuperApplication extends SpringBootServletInitializer {
	public static void main(String[] args) {
		SpringApplication.run(SuperApplication.class, args);
	}
}
```

## war파일 배포할 때 SpringBootServletInitializer를 상속해야 하는 이유
Spring 웹 애플리케이션을 외부 Tomcat에서 동작하도록 하기 위해서는 web.xml (Deployment Descriptor, DD)에 애플리케이션 컨텍스트를 등록해야만 한다. 
이는, Apache Tomcat(Servlet Container)이 구동될 때 <u>/WEB-INF 디렉토리에 존재하는 web.xml을 읽어 웹 애플리케이션을 구성하기 때문이다.</u>
하지만 Servlet 3.0 스펙으로 업데이트되면서 web.xml이 없어도 동작이 가능해졌다. 이는, 
web.xml 설정을 WebApplicationInitializer 인터페이스를 구현하여 대신할 수 있게 됐고, 프로그래밍적으로 ServletContext에 
Spring IoC 컨테이너(AnnotationConfigWebApplicationContext)를 생성하여 추가할 수 있도록 변경됐기 때문이다.

이와 비슷한 맥락에서, web.xml이 없는 SpringBoot 웹 애플리케이션을 외부 Tomcat에서 동작하도록 하기 위해서는 WebApplicationInitializer 인터페이스를 구현한 
SpringBootServletInitializer를 상속을 받는 것이 필요했던 것이다.


### Reference
[!Spring Boot 웹 애플리케이션을 WAR로 배포할 때 왜 SpringBootServletInitializer를 상속해야 하는걸까?](https://medium.com/@SlackBeck/spring-boot-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%EC%9D%84-war%EB%A1%9C-%EB%B0%B0%ED%8F%AC%ED%95%A0-%EB%95%8C-%EC%99%9C-springbootservletinitializer%EB%A5%BC-%EC%83%81%EC%86%8D%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94%EA%B1%B8%EA%B9%8C-a07b6fdfbbde)