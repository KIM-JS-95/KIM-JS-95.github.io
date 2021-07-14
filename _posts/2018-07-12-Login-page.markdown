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

### HttpSecurity 메소드
A HttpSecurity는 네임스페이스 구성에서 Spring Security의 XML <http> 요소와 유사합니다. 
특정 http 요청에 대해 웹 기반 보안을 구성할 수 있습니다. 기본적으로 모든 요청에 적용되지만 requestMatcher(RequestMatcher)또는 기타 유사한 방법을 사용하여 제한할 수 있습니다 .

```java
   protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/", "/login", "/registration", "/h2/**").permitAll()
                .antMatchers("/home/admin").hasAuthority(ERole.ADMIN.getValue())
                .antMatchers("/home/user").hasAuthority(ERole.MANAGER.getValue())
                .antMatchers("/home/guest").hasAuthority(ERole.GUEST.getValue())
                .anyRequest().authenticated()
                
        .and()
                .csrf()
                .disable()
                .headers()
                .frameOptions().disable()
                
        .and()
                .formLogin()
                .loginPage("/login")
                .defaultSuccessUrl("/home")
                .failureUrl("/login?error=true")
                .successHandler(successHandler())
                .failureHandler(failureHandler())
                .usernameParameter("username")
                .passwordParameter("password")
                
        .and()
                .logout()
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                .logoutSuccessUrl("/login")
                .and()
                .exceptionHandling()
                .accessDeniedPage("/access-denied");
    }
```

#### antMatchers()
ant경로를 기반으로 URL 경로를 선택하여 이어지는 명령어에 따라 *.permitAll()* / *hasAutority()* etc... 접근 권한을 부여한다.
   

> ant 경로 패턴
>
> 3가지(?, * , **) 특수문자로 경로를 표시하는 패턴 
>
> 1. * : 0개 또는 그 이상의 문자와 매치
> 2. ** : 0개 또는 그 이상의 디렉토리와 매치
> 3. ? : 1개 이상의 글자
> 
> 예시)
>
> 1. "/member/?*.jsp" -> /member/로 시작하고 확장자가 .jsp 로 끝나는 모든 경로
> 
> 2. "/abc/d? fg.hi" -> /abc/d 로 시작하고 1글자가 사이에 위치하고 fg.hi로 끝나는 모든 경로
> 
> 3. "/folder/**/files" -> /folder/로 시작하고 중간에 0개 이상의 중간 경로가 존재하고 /files 로 끝나는 모든 경로

#### .formLogin()
비설정 *.disable()* 시 스프링에서 기본 제공하는 페이지를 사용하게 되며 *.loginPage()* 를 설정하게 되면 사용자가 제작하거나 
원하는 페이지의 로그인 form을 사용 할 수 있게 된다.
   
동시에 성공 / 실패 에 따라 페이지를 구분하거나 *successHandler() / failureHandler()* 를 설정하여 이동할 수 있도록 가능하며 원하는 추가적인 기능을 구현이 
가능하다.

*.usernameParameter("email")* 의 경우는 html 파일에서 설정한 파라메터 name="email" 을 사용함을 명시해주는 역할이다.


#### csrf(Cross site request forgery)
웹 사이트의 취약점을 이용하여 이용자가 의도하지 하지 않은 요청을 통한 공격을 의미하며
http 통신의 Stateless(비상태저장) 특성을 이용하여 쿠키 정보만 이용해서 사용자가 의도하지 않은 다양한 공격들을 시도할 수 있다.

이러한 특성을 차단하기위해  CSRF Token 정보를 Header 정보에 포함하여 서버 요청을 시도하는 방법을 사용할 수 있다.

> 자세한 사용법은 다음 블로그에서 확인할 것
> [Cross site request forgery](https://cheese10yun.github.io/spring-csrf/)


### 정리

### 🧾Reference
> 로그인 구성과 보안 학습 자료 제공에 정말 큰 도움이 되었습니다. 감사합니다.

1. [Blog by "u2ful"](https://u2ful.tistory.com/)
2. [클래스 HttpSecurity](https://docs.spring.io/spring-security/site/docs/4.2.x/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html)



