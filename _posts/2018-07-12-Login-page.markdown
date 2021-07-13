---
layout: post
title:  "spring 보안 로그인 폼"
date:   2021-07-12 14:05:21 +0800
tags: SpringBoot Security
color:  rgb(154,133,255)
subtitle: '스프링 시큐리티 보안 형태 분석하기'
---

## 서론
홈페이지 제작과정에서 가장 중요한 것은 보안이라고 생각한다.
사용자의 정보를 *크립토그래피* 과정을 통해 해쉬처리하고 JWT를 통해 로그인 유지와 인증을 보장하는 과정은 쉬운 과정은 아니었다.

그중에서도 *SecurityConfig* 를 구성하는 과정은 보기만 해서는 이해할 수 없었다. 

때문에 직접 로그인 화면을 제작하고 *스프링 Security* 에서 제공하는 HttpSecurity 메소드를 직접 사용해보며 
스프링 보안에 한발 더 접근해 보도록 한다.

### 어떤 로그인 화면을 가져올 것인가?

스프링에서는 기본적으로 제공되는 보안 화면이 있다.
일방적으로 로그인만 가능하고 회원가입 구성이 없어 개발자 이외에는 로그인이 불가능하다.

![1](https://blog.kakaocdn.net/dn/bhsWG1/btqB2uqtACf/v266RwlsUQZYGGLGnXcPm0/img.png)

반대로 개발자가 직접 제작하는 페이지를 사용하고자 한다면 SecurityConfig 에서 별도로 설정해 주어야 한다.

![image](https://user-images.githubusercontent.com/65659478/125460615-f27d71cb-ff40-4dcd-85cd-5256831b8c54.png)

추가적으로 로그인 페이지 외에도 파일 접근권한 설정 , 사용자 접근권한 등을 설정할 수 있다.

### SecurityConfig 구성

전체적인 웹의 사용자 설정이 가능하고 그 밖에도 보안 요소까지도 설정이 가능하다.
*WebSecurityConfigurerAdapter* 상속을 받는것으로 Web / Http 보안 설정 외에도 
인증와 관련한 AuthenticationManagerBuilder 까지도 관리할 수 있다.


```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    
    }

    @Override
    public void configure(WebSecurity web) throws Exception{

    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
 
    }
}
```

주제에 따라 로그인 페이지에 관한 HttpSecurity 메소드에 관해서만 다룬다.

### HttpSecurity

### 정리

### 🧾Reference
> 로그인 구성과 보안 학습 자료 제공에 정말 큰 도움이 되었습니다. 감사합니다.

1. [로그인 페이지 만들기](https://u2ful.tistory.com/)



