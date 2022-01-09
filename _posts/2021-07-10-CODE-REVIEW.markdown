---
layout: post 
title: "2021-06-12-CODE-REVIEW"
date: 2021-07-17 14:05:21 +0800
tags: Self-CodeReview 
color: rgb(255,90,90)
author: KIM-JS-95 
subtitle: '2021-07-17-Self Code REVIEW'
---

# CODE-REVIEW

## What did you do?

1. Spring Security config 구성

### Security config 구성

Spring Security 에서 지원한 메소드는 Web, Http, Crypto(암호화) 등이 있다. 전체적인 구성의 목적은 페이지 접근권한, 사용자 정보에 대한 보호 및 암호화가 있다.

이번 코드 리뷰는 WebSecurity,HttpSecurity 메소드에서 사용되는 로그인 구성에 대해 학습하고 직접 제작해 보았다.

```java
public class main() {
  @Override
  public void configure(WebSecurity web) throws Exception{
    web.ignoring()
            .antMatchers("/css/**","/js/**","/images/**","/lib/**");
  }
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/", "/login", "/registration", "/h2/**").permitAll() --- 1
                .anyRequest().authenticated()

                .and()
                .csrf()
                .disable()
                
                .headers()
                .frameOptions().disable()

                .and()
                .formLogin() --- 2
                .loginPage("/login") 
                .defaultSuccessUrl("/home")
                .failureUrl("/login?error=true")
                .successHandler(successHandler())
                .failureHandler(failureHandler())
                .usernameParameter("username")
                .passwordParameter("password")

                .and()
                .logout() --- 3
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                .logoutSuccessUrl("/login")
                
                .and()
                .exceptionHandling()
                .accessDeniedPage("/access-denied");
    }
    
  @Bean
  public AuthenticationSuccessHandler successHandler() {
    return new CustomAuthSuccessHandler();
  }

  @Bean
  public AuthenticationFailureHandler failureHandler() {
    return new CustomAuthFailureHandler();
  }
}
```
### WebSecurity

Web 구성에 필요한 리소스 까지 인증 범위에 표함될 경우 원하는 모양의 페이지가 표시되지 않을 수 있다. 

리소스(js, css 등)는 인증할 필요성이 없는 경우는 web.ignore.antMatchers("파일 경로") 하여 인증 범위에서 제외 시킬 수 있다.


### HttpSecurity

1. 페이지 접근 허용

보안 구성을 할 경우 확인 해햐할 것은 페이지 접근 권한에 대해 번위를 선택하는 것이다.
모든 페이지를 보안으로 구성하게 되면 로그인 페이지 외에는 어느 서비스도 이용할 수 없게된다. 때문에 권한이 없어도 이용가능한 기능이
구성 되어 있는 경우 페이지 단위로 허용하여 서비스 이용할 할 수 있도록 해야한다.

.antMatchers() 는 지정한 페이지의 권한 범위를 지정할 수 있다. 
해당 코드는 로그인 페이지, 회원가입 페이지, h2-console 페이지의 접근에 대해서 허용한 상태이다.


2. 로그인 구성

로그인 구성은 Spring-Security 에서 제공되는 페이지가 제공되지만 .loginPage("해당 url") 메소드로 사용자가 제작한 로그인 페이지를 
구성 할 수 있다.

로그인은 성공과 실패로 구성하며 각 결과에 따라 handler를 구성하여 사용자가 원하는 기능을 구현할 수 있다.

로그아웃 또한 .logoutRequestMatcher(new AntPathRequestMatcher("url")) 로 연결하여 기능을 구현할 수 있다.
로그아웃 기능이 성공적으로 이루어진다면 .logoutSuccessUrl("url") 으로 페이지이동 또한 구성할 수 있다.


그중 CSRF(Cross Site Request Forgery : 사이트 간 요청 위조)는 개발중에는 매우 귀찮은 존재입니다
Spring Security의 CSRF 프로텍션은 Http 세션과 동일한 생명주기(Life Cycle)을 가지는 토큰을 발행한 후 
Http 요청(PATCH, POST, PUT, DELETE 메소드인 경우)마다 발행된 토큰이 요청에 포함되어(Http헤더 혹은 파라메터 둘중 하나)
있는지 검사하는 가장 일반적으로 알려진 방식의 구현이 설정되어 있습니다.

### WebSecurity vs HttpSecurity

두 구성의 차이를 보자면 WebSecurity 는 웹 소스에 대한 권한 구성으로 볼수 있다. 구성한 리소스 html ,css, js를 보호하거나 권한을 가진 경우에만 접근
할 경우는 보안을 구성하면 된다.

HttpSecurity 는 URL 관점에서의 보호라고 볼 수 있다. WebSecurity 에서 권한을 구성하여 리소스를 제한할 수 도 있겠지만 
HTTP 관점에서 URL을 차단하는 방식에 더욱 효율적이며 관리가 쉽기 때문이다.



### My GitHub link

[해당 프로젝트 링크](https://github.com/KIM-JS-95/FreeNote.git)