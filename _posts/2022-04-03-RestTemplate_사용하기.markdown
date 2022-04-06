---
layout: post 
title: "RestTemplate 사용하기"
date: 2022-04-3 10:05:21 +0800 
tags: 스프링
color: rgb(98,170,255)
subtitle: openAPI를 적극 활용해보자
---
 
## RestTemplate 의 등장

`Spring 3` 부터 지원하기된 이 클래스는 스프링에서 제공하는 http 통신에 유용하게 쓸 수 있는 템플릿으로
HTTP 서버와의 통신을 단순화를 목적으로 등장했다.

- RESTful 원칙
- 멀티쓰레드 방식
- Blocking 방식

> Spring 5 부터는 WebFlux / WebClient 방식을 도입하여 RestTemplate을 대체할 수 있다. 


### RestTemplate 상세 설명

스프링 공식 문서를 확인하면 다음과 같이 쓰여있다.

HTTP 요구를 실행하는 HttpURL Connection, Apache HttpComponents 등의 기본 HTTP 클라이언트라이브러리 상에서 **심플한 템플릿 메서드 API**를 제공한다.

`RestTemplate`는 HTTP 메서드에 의한 일반적인 시나리오용 템플릿과 빈도가 낮은 케이스를 지원하는 일반적인 교환 및 실행 방식을 제공합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F99300D335A9400A52C16C1)

- 어플리케이션이 RestTemplate를 생성하고, URI, HTTP메소드 등의 헤더를 담아 요청
- RestTemplate 는 `HttpMessageConverter` 를 사용하여 requestEntity 를 요청메세지로 변환한다.
- RestTemplate 는 ClientHttpRequestFactory 로 부터 ClientHttpRequest 를 가져와서 요청을 보낸다.
- **ClientHttpRequest 는 요청메세지를 만들어 HTTP 프로토콜을 통해 서버와 통신한다.**
- RestTemplate 는 ResponseErrorHandler 로 오류를 확인하고 있다면 처리로직을 태운다.
- ResponseErrorHandler 는 오류가 있다면 ClientHttpResponse 에서 응답데이터를 가져와서 처리한다.
- RestTemplate 는 HttpMessageConverter 를 이용해서 응답메세지를 java object(Class responseType) 로 변환한다.
- 어플리케이션에 반환된다.


**즉, 개발자가 구성한 **URI**와 **HEADER** 등의 데이터를 조립하여 `RestTemplate`에 반환하고 싶은 메소드를 적용만 해준다면
자동으로 웹상에서 URI를 호출하여 데이터를 처리해주는 것이다.**


**<반환하고 싶은 메소드>**
![](https://media.vlpt.us/images/soosungp33/post/d02efacc-56e6-4abc-a390-12ecbb565c6b/image.png)

### RestTemplate 사용방법

```java
@RestController
public class Apicontroller {


    /// https://openapi.naver.com/v1/search/local.xml?query=%EC%A3%BC%EC%8B%9D&display=10&start=1&sort=random

    @GetMapping("/naver")
    public String naver() {

        String query = "중국집";

        // URI 구성하기
        URI uri = UriComponentsBuilder
                .fromUriString("https://openapi.naver.com")
                .path("v1/search/local.json")
                .queryParam("query", query)
                .queryParam("display", 10)
                .queryParam("start", 1)
                .queryParam("sort", "random")
                .encode(Charset.forName("UTF-8"))
                .build()
                .toUri();

        // 헤더 붙여주기
        RequestEntity<Void> req = RequestEntity
                .get(uri)
                .header("X-Naver-Client-Id", "You're X-Naver-Client-Id")
                .header("X-Naver-Client-Secret", "You're X-Naver-Client-Secret")
                .build();

        RestTemplate restTemplate = new RestTemplate();


        ResponseEntity<String> result = restTemplate.exchange(req, String.class);

        return result.getBody();

    }
}
```

## Reference
- [빨간색코딩](https://sjh836.tistory.com/141)
- [스프링 RestTemplate 정리](https://velog.io/@soosungp33/%EC%8A%A4%ED%94%84%EB%A7%81-RestTemplate-%EC%A0%95%EB%A6%AC%EC%9A%94%EC%B2%AD-%ED%95%A8)
- [한 번에 끝내는 Java/Spring 웹 개발 마스터 초격차 패키지 Online.]()
