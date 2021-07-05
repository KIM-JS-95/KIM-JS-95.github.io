---
layout: post
title:  "Session_and_Token"
date:   2021-06-8 14:05:21 +0800
tags: Security Token Session
color: rgb(154,133,255)
cover: '../assets/profile.jpeg'
subtitle: '보안에서의 Session 과 Token'
---

## Session vs Token

### Session

Session 과 Cookie 인증 방식은 클라이언트의 세션 저장소를 요구하며 로그인시 사용자 정보를 저장하고 Key로 
사용되는 세션 ID를 생성한다. 사용은 HTTP Header에 세션 ID가 포함된 Cookie와 함께 클라이언트에
전송하는 방식으로 클라이언트는 이를 세션 저장소에서 비교하여 인증이 완료되면 Request에 따라 응답을 수행한다.

- 유저가 로그인을 하고 세션이 서버 메모리 상에 저장된다. 이 때 세션을 식별하기 위한 Session Id 를 기준으로 정보를 저장한다.
- 브라우저에 쿠키로 Session Id 가 저장된다.
- 쿠키에 정보가 담겨있기 때문에 브라우저는 해당 사이트에 대한 모든 Request 에 Session Id 를 쿠키에 담아 전송한다.
- 서버는 클라이언트가 보낸 Session Id 와 서버 메모리로 관리하고 있는 Session Id 를 비교하여 Verification 을 수행한다.

#### 장접

1.  쿠키는 세션 저장소에 담긴 유저 정보를 얻기 위한 열쇠이지만 만일의 HTTP 요청
    도중에 Cookie 가 노출되더라도 쿠키에 포함된 세션 ID는 의미없는 값으로 본다.
    때문에 초기 인증방식의 계정정보 인증 방식 보다는 안전하다고 본다.

2. 모든 사용자가 서로다른 Session ID를 발급받게 되며 클라이언트는 별도의 사용자 조회 없이도 Session ID를 통해 사용자
   정보를 확인할 수 있어 서버의 자원에 접근성이 빠르다.

#### 단점

1. 세션 하이재킹 공격
  HTTP의 Cookie가 노출되어도 Session ID 자체적 의미는 없지만 해커가 SessionID를 악용하여 사용자의 권한으로
   클라이언트에 접근 하여 정보를 탈취할 위험성이 있다. 즉 타인의 권한(SessionID)으로 악용가능성이 있다.
   

![Session / Cookie 방식](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbe5HFu%2FbtqAsR8iEdh%2Frk9Xno6XlQAwbTWFiGIXIk%2Fimg.png)

### Token (JWT)

인증받은 사용자들에게 Token을 발급하고, 서버에 요청을 할 때 Header 에 Token을 함께 보내도록 하여 유효성 검사를 한다. 
이러한 시스템에서는 더이상 사용자의 인증 정보를 서버나 세션에 유지하지 않고 클라이언트 측에서 들어오는 요청만으로 작업을 처리한다.
즉, Session / Cookie 방식과 달리 상태를 유지하지 않는 구조이다. 
이러한 토큰 기반의 인증 방식을 통해 수많은 문제점들을 해결할 수 있는데, 대표적으로 사용자가 로그인이 되어있는지 안되어있는지 신경쓰지 않고 손쉽게 
시스템을 확장할 수 있다.

- 유저가 로그인을 하고 서버에 세션을 이용해서 정보를 기록하는대신, Token 을 발급한다.
- 클라이언트는 발급된 Token 을 저장한다 (일반적으로 local storage 에 저장한다.)
- 클라이언트는 요청 시 저장된 Token 을 Header 에 포함시켜 보낸다.
- 서버는 매 요청시 클라이언트로부터 전달받은 Header 의 Token 정보를 Verification 한 뒤, 해당 유저에 권한을 인가한다.

### 장점
1. 세션/쿠키는 별도의 저장소의 관리가 필요하지만 JWT는 발급한 후 검증만 하면 되기 때문에 추가 저장소가 필요 없습니다.
   이는 비상태유지 서버의 특징으로 볼 수 있다. 여기서 Stateless는 어떠한 별도의 저장소도 사용하지 않는, 즉 상태를 저장하지 않는 것을 의미합니다.

2. 확장성이 우수하여 Token 기반으로 하는 다른 인증 시스템에 접근이 가능합니다. 
   예를 들어 Facebook 로그인, Google 로그인 Kakao 로그인(**OAuth 2.0**) 등은 모두 토큰을 기반으로 인증을 합니다.
   
> OAut??
> 
> 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 
> 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준이다.

여기까지의 글만 봤을 때는 JWT가 세션/쿠키 방식보다 더 효율적으로 보입니다. 하지만 JWT도 단점들이 존재합니다.

#### 단점

1. 이미 발급된 JWT에 대해서는 돌이킬 수 없습니다. 세션/쿠키의 경우 만일 쿠키가 악의적으로 이용된다면, 해당하는 세션을 지워버리면 됩니다. 하지만 JWT는 한 번 발급되면 유효기간이 완료될 때 까지는 계속 사용이 가능합니다. 따라서 악의적인 사용자는 유효기간이 지나기 전까지 신나게 정보들을 털어갈 수 있습니다.
   -> 해결책
   기존의 Access Token의 유효기간을 짧게 하고 Refresh Token이라는 새로운 토큰을 발급합니다. 그렇게 되면 Access Token을 탈취당해도 상대적으로 피해를 줄일 수 있습니다. 이는 다음 포스팅에 나올 Oauth2에 더 자세히 다루도록 하겠습니다.

2. Payload 정보가 제한적입니다. 위에서 언급했다시피 Payload는 따로 암호화되지 않기 때문에 디코딩하면 누구나 정보를 확인할 수 있습니다. (세션/쿠키 방식에서는 유저의 정보가 전부 서버의 저장소에 안전하게 보관됩니다) 따라서 유저의 중요한 정보들은 Payload에 넣을 수 없습니다.

3. JWT의 길이입니다. 세션/쿠키 방식에 비해 JWT의 길이는 깁니다. 따라서 인증이 필요한 요청이 많아질 수록 서버의 자원낭비가 발생하게 됩니다.


![Token 방식](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FogoAg%2FbtqAriyT5sY%2FYYt2wkEz50kKN47mLwRDXK%2Fimg.png)

### 🧾Reference
1. [Session 기반 인증과 Token 기반 인증](https://jins-dev.tistory.com/entry/Session-%EA%B8%B0%EB%B0%98-%EC%9D%B8%EC%A6%9D%EA%B3%BC-Token-%EA%B8%B0%EB%B0%98-%EC%9D%B8%EC%A6%9D)
2. [Session / Cookie 방식](https://tansfil.tistory.com/58)
3. [토큰 기반 인증 VS 서버 기반 인증](https://mangkyu.tistory.com/55)
4. [OAuth](https://ko.wikipedia.org/wiki/OAuth)
