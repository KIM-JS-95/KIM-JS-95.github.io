---
layout: post
title:  "드디어 도착했다! Spring Cloud"
date:   2022-01-31 12:05:21 +0800
tags: Spring-Cloud
color: rgb(154,133,255)
subtitle: 서비스 기업의 기본 소양
--- 

## 🚀 Spring Cloud 서론

```text

Spring Cloud is a framework for building robust cloud applications. The framework facilitates the development of applications by providing solutions to many of the common problems faced when moving to a distributed environment.

Applications that run with microservices architecture aim to simplify development, deployment, and maintenance. The decomposed nature of the application allows developers to focus on one problem at a time. Improvements can be introduced without impacting other parts of a system.

On the other hand, different challenges arise when we take on a microservice approach:

Externalizing configuration so that is flexible and does not require rebuild of the service on change
Service discovery
Hiding complexity of services deployed on different hosts
In this article, we'll build five microservices: a configuration server, a discovery server, a gateway server, a book service, and finally a rating service. These five microservices form a solid base application to begin cloud development and address the aforementioned challenges.


 - Baeldung 발췌 -
```
정리하자면 Spring Cloud는 클라우드 어플리케이션 구축 프레임 워크이며 
서비스 개발의 규모가 거대해지면서 각 서비스를 분산적으로 구축 및 개발 관리하는데 이를 `MSA(Micro Service Architecture)`라고한다.

서비스 역할 단위로 구분하여 개발하지만 서비스 이용시 하나의 서비스처럼 묶어 줄 수 있는 도구가 필요한데 여기서 등장하는 것이 `Spring Cloud`이다. 

#### Spring Cloud Gateway (SCG)




### 🧾Reference
[Spring Cloud](https://www.baeldung.com/spring-cloud-configuration)

[Swegger 기본 사용법](https://leeys.tistory.com/29)

[Swagger Editor](https://editor.swagger.io/)