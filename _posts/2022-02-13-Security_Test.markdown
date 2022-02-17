---
layout: post
title:  "Spring Security 단위 테스트"
date:   2022-02-13 12:05:21 +0800
tags:  Security
color: rgb(154,133,255)
subtitle: Spring Security 단위 테스트 방법
--- 
## 🚀 Security 테스트
스프링 환경에서 Security를 적용할 경우 인증된 토큰을 가진 사람만 접근이 가능하다

이 경우 단위 테스트를 할 때에 어떻게 접근해야하는지 알아야 한다.


기존에 다음과 같이 유저 정보가 입력되어 있어야한다.
```java 

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
                .withUser(User.withDefaultPasswordEncoder()
                        .username("user1")
                        .password("1111")
                        .roles("USER")
                        .build())
        ;
    }
    

}

```

### 🌠 헤더를 직접 입력하는 방법

`Base64` 토큰을 생성하여 헤너에 넣어주는 방법으로 가장 정석의 방법이라고 할 수 있다.

토큰은 유저의 정보(ID / PW)를 입력하여 생성된 토큰은 헤더에 넣어주게 된다.
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class BasicTokenTest {

    @LocalServerPort
    int port;

    private RestTemplate restTemplate = new RestTemplate();

    @DisplayName("1. Basic Token Test")
    @Test
    void test_1(){

        String url = format("http://localhost:%d%s", port, "/greeting");
        HttpHeaders headers = new HttpHeaders();
        headers.add(HttpHeaders.AUTHORIZATION, "basic "+ Base64.getEncoder().encodeToString("user1:1111".getBytes()));
        HttpEntity entity = new HttpEntity("", headers);

        ResponseEntity<String> response = restTemplate.exchange(url, HttpMethod.GET, entity, String.class);

        assertEquals("Hello jongwon", response.getBody());
    }

}

```
### 🌠 TestRestTemplate

`TestRestTemplate`메소드는 이미 `Base64`토큰에 생성과 사용이 구현되어 있어서 유저 정보만 입력해줘도 자동으로 보안 인증이 가능하다.


```java

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class BasicTokenTest {

    @LocalServerPort
    int port;

    private RestTemplate restTemplate = new RestTemplate();

    @DisplayName("2. 템플릿 테스트")
    @Test
    void test_2(){
        String url = format("http://localhost:%d%s", port, "/greeting");
        TestRestTemplate testClient = new TestRestTemplate("user1", "1234");
        String resp = testClient.getForObject(url, String.class);

        assertEquals("Hello jongwon", resp);
    }

}

```

### 🧾 Reference


