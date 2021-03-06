---
layout: post
title:  "Servlet and JSP"
date:   2022-02-09 12:05:21 +0800
tags: Spring Web
color: rgb(154,133,255)
subtitle: Servlet JSP
--- 
### ๐ JSP(JavaServer Pages)

JSP๋ HTML๋ด์ ์๋ฐ ์ฝ๋๋ฅผ ์ฝ์ํ์ฌ ์น ์๋ฒ์์ ๋์ ์ผ๋ก ์น ํ์ด์ง๋ฅผ ์์ฑํ์ฌ ์น ๋ธ๋ผ์ฐ์ ์ ๋๋ ค์ฃผ๋ 
์๋ฒ ์ฌ์ด๋ ์คํฌ๋ฆฝํธ ์ธ์ด์ด๋ค. 
Java EE ์คํ ์ค ์ผ๋ถ๋ก ์น ์ ํ๋ฆฌ์ผ์ด์ ์๋ฒ์์ ๋์ํ๋ค.

### ๐ Servlet
ํด๋ผ์ด์ธํธ์ ์์ฒญ์ ์ฒ๋ฆฌํ๊ณ , ๊ทธ ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํ๋
Servlet ํด๋์ค์ ๊ตฌํ ๊ท์น์ ์งํจ `์๋ฐ ์น ํ๋ก๊ทธ๋๋ฐ ๊ธฐ์ `
๋ค์๋งํด, ์๋ฐ ์ธ์ด๋ก ๊ตฌํํ ์น ์ดํ๋ฆฌ์ผ์ด์์ด๋ผ๋ ๋ง์ธ๋ฐ ์ด ๋๋ฌธ์ `JSP`์ ๋ง์ด ๋น๊ตํ๋ค.

### ๐ Servlet Container (์๋ธ๋ฆฟ ์ปจํ์ด๋)

`Spring Boot` ๊ฐ๋์ด ๋ฑ์ฅํ๊ธฐ ์  ์น ์ดํ๋ฆฌ์ผ์ด์์ผ๋ก 
![](https://gitlab.com/jongwons.choi/spring-boot-security-lecture/-/raw/master/images/fig-1-servlet-container.png)

- ํฐ์ผ๊ณผ ๊ฐ์ ์น ์ ํ๋ฆฌ์ผ์ด์์ ์๋ธ๋ฆฟ ์ปจํ์ด๋๋ผ๊ณ  ๋ถ๋ฅด๋๋ฐ, ์ด๋ฐ ์น ์ ํ๋ฆฌ์ผ์ด์(J2EE Application)์ ๊ธฐ๋ณธ์ ์ผ๋ก ํํฐ์ ์๋ธ๋ฆฟ์ผ๋ก ๊ตฌ์ฑ๋์ด ์์ต๋๋ค.
- ํํฐ๋ ์ฒด์ธ์ฒ๋ผ ์ฎ์ฌ์๊ธฐ ๋๋ฌธ์ `ํํฐ ์ฒด์ธ`์ด๋ผ๊ณ ๋ ๋ถ๋ฆฌ๋๋ฐ, ๋ชจ๋  request ๋ ์ด ํํฐ ์ฒด์ธ์ ๋ฐ๋์ ๊ฑฐ์ณ์ผ๋ง ์๋ธ๋ฆฟ ์๋น์ค์ ๋์ฐฉํ๊ฒ ๋ฉ๋๋ค.



#### ๐  ์๋ธ๋ฆฟ ์น ํ์ด์ง
```java
@WebServlet(name = "memberSaveServlet", urlPatterns = "/servlet/members/save")
public class MemberSaveServlet extends HttpServlet {

    private final MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        final String username = request.getParameter("username");
        final int age = Integer.parseInt(request.getParameter("age"));
        final Member member = new Member(username, age);
        memberRepository.save(member);

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        final PrintWriter printWriter = response.getWriter();
        printWriter.write("<html>\n" +
            "<head>\n" +
            " <meta charset=\"UTF-8\">\n" +
            "</head>\n" +
            "<body>\n" +
            "์ฑ๊ณต\n" +
            "<ul>\n" +
            " <li>id=" + member.getId() + "</li>\n" +
            " <li>username=" + member.getUsername() + "</li>\n" +
            " <li>age=" + member.getAge() + "</li>\n" +
            "</ul>\n" +
            "<a href=\"/index.html\">๋ฉ์ธ</a>\n" +
            "</body>\n" +
            "</html>");
    }
}
```

์ฒ์ SpringBoot๋ก ๊ฐ๋ฐ์ ์์ํ ๋์๊ฒ ์์ด์ Client ์ Server์ ๊ตฌ๋ถ์ด ์์ด ๊ฐ๋์ฑ์ด ๋จ์ด์ง๊ณ 
์ ์ง๋ณด์์์๋ ๋ฌธ์ ๊ฐ ๋ง์์ง ๊ฒ์ ๋๊ผ๋ค.

๊ทธ๋์ ์ฌ๊ธฐ์ ๋ฐ์ ํ ๊ฒ์ด `JSP`์ด๋ค

#### ๐  JSP ์น ํ์ด์ง
```java
<%@ page import="hello.servlet.basic.domain.member.Member" %>
<%@ page import="hello.servlet.basic.domain.member.MemberRepository" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    final MemberRepository memberRepository = MemberRepository.getInstance();
    final String username = request.getParameter("username");
    final int age = Integer.parseInt(request.getParameter("age"));
    final Member member = new Member(username, age);
    memberRepository.save(member);
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
์ฑ๊ณต
<ul>
    <li>id=<%=member.getId()%>
    </li>
    <li>username=<%=member.getUsername()%>
    </li>
    <li>age=<%=member.getAge()%>
    </li>
</ul>
<a href="/index.html">๋ฉ์ธ</a>
</body>
</html>
```

๊ธฐ์กด ์๋ธ๋ฆฟ์ด JAVA์ HTML์ฝ๋๋ฅผ ๋ฃ๋ ๋ฐฉ์์ด๋ผ๋ฉด JSP๋ ๊ทธ ๋ฐ๋์ ๋ฐฉ์์ ์ ์ฉํ๋ค.
๊ทธ๋๋ ์์ฃผ ๋ณ๋ก๋ค ๐คฎ๐คฎ๐คฎ๐คฎ๐คฎ

### ์ฐจ์ด์ 
- Servlet
  - ์๋ฐ์ฝ๋๋ก ๊ตฌํํ๊ณ  ์ปดํ์ผํ๊ณ  ๋ฐฐํฌํด์ผ ํ๋ค.
  - HTMLํ๊ทธ๋ก ๋ฌธ์์ด("")์คํฌ๋ฆผ์ผ๋ก ์ฒ๋ฆฌํด์ผ ํ๋ค.
  - ์ฝ๋๊ฐ ์์ ๋๋ฉด ๋ค์ ์ปดํ์ผํ๊ณ  ๋ฐฐํฌํด์ผ ํ๋ค


- JSP
   - ํค์๋๊ฐ ํ๊ทธํ ๋์ด ์๋ธ๋ฆฟ์ ๋นํด ๋ฐฐ์ฐ๊ธฐ ์ฝ๋ค.
   - ์๋ฐ์ฝ๋๋ฅผ <%%>ํ๊ทธ ์์ ์ฒ๋ฆฌํด์ฃผ์ด์ผ ํ๋ค.
   - HTML์ฒ๋ผ ํ๊ทธ๋ฅผ ์ฌ์ฉํ์ฌ ์๋ฐ์ฝ๋๋ ์ฌ์ฉ์ด ๊ฐ๋ฅํ๋ค.


### ๐  Servlet ํน์ง

- ํด๋ผ์ด์ธํธ์ ์์ฒญ์ ๋ํด ๋์ ์ผ๋ก ์๋ํ๋ ์น ์ดํ๋ฆฌ์ผ์ด์ ์ปดํฌ๋ํธ
- html์ ์ฌ์ฉํ์ฌ ์์ฒญ์ ์๋ตํ๋ค.
- Java Thread๋ฅผ ์ด์ฉํ์ฌ ๋์ํ๋ค.
- **MVC ํจํด์์ Controller๋ก ์ด์ฉ๋๋ค.**
- HTTP ํ๋กํ ์ฝ ์๋น์ค๋ฅผ ์ง์ํ๋ javax.servlet.http.HttpServlet ํด๋์ค๋ฅผ ์์๋ฐ๋๋ค.
- UDP๋ณด๋ค ์ฒ๋ฆฌ ์๋๊ฐ ๋๋ฆฌ๋ค.
- HTML ๋ณ๊ฒฝ ์ Servlet์ ์ฌ์ปดํ์ผํด์ผ ํ๋ ๋จ์ ์ด ์๋ค.


### ๐  JSP ํน์ง

- ์ต์ด ์๋ธ๋ฆฟ์ผ๋ก ์ปดํ์ผ ๋ ํ์๋ ๋ฉ๋ชจ๋ฆฌ์์ ์ฒ๋ฆฌ๋๊ธฐ ๋๋ฌธ์ ๋ง์ ์ฌ์ฉ์ ์ ์๋ ์ํ ํ ์ฒ๋ฆฌ๋๋ค.
- JSP ๋ํ ๋ค๋ฅธ Servlet ๊ฐ ๋ฐ์ดํฐ ๊ณต์ ๊ฐ ์ฉ์ดํ๋ค.
- ์๋ฐ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ํ๊ณ  ์์ผ๋ฏ๋ก ํ๋ซํผ์ ๋๋ฆฝ์ ์ด๋ค.
- (์๋ฐ)๋น์ฆ๋ผ๋ ์๋ฐ ์ปดํฌ๋ํธ๋ฅผ ์ฌ์ฉํ  ์ ์๋ค.
- ๋๊ท๋ชจ ์ดํ๋ฆฌ์ผ์ด์์ ๊ตฌํํ  ๋ ์ฌ์ฉ๋๋ EJB(Enterprise Java Bean) ๊ธฐ์ ๊ณผ ์๋ฒฝํ๊ฒ ํธํ๋๋ค.
- ์ฌ์ฉ์ ์ ์ ํ๊ทธ๋ฅผ ๋ง๋ค์ด ์ฌ์ฉํ  ์ ์์ผ๋ฉฐ JSTL๊ณผ ๊ฐ์ ํ๊ทธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ด์ฉํ  ์ ์๋ค.
- HTTP์ ๊ฐ์ ํ๋กํ ์ฝ์ ๋ฐ๋ผ ํด๋ผ์ด์ธํธ์ ์์ฒญ์ ์ฒ๋ฆฌํ๊ณ  ์๋ตํ๋ค.
- HTML, XML ๋ฑ ์น ์๋น์ค์ ๊ด๋ จ๋ ๋ฌธ์๋ฅผ ์์ฑํ๋ ๋ฐ ์ฃผ๋ก ์ฌ์ฉ๋๋ค.
- JDBC(Java DataBase Connect) API๋ฅผ ์ฌ์ฉํ์ฌ ์น ์ดํ๋ฆฌ์ผ์ด์์์ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ฐ๊ฒฐํ  ์ ์๋ค.
- ํํ ์ธ์ด, ํํ์, ์คํฌ๋ฆฝํธ๋ฆฟ ๋ฑ์ ๋ค์ํ ์คํฌ๋ฆฝํธ ์์์ ์ก์ ํ๊ทธ ๋ฑ์ ์ ๊ณตํจ์ผ๋ก์จ ๋์ฑ ์ฌ์ด        ํ๋ก๊ทธ๋๋ฐ์ ๊ตฌํํ  ์ ์๋ค.
- ์๋ธ๋ฆฟ ์ปจํ์ด๋(ex. Tomcat)๊ฐ ํ์ํ๋ค.



### ๐งพ Reference

[Servlet JSP ์ฐจ์ด](https://steady-coding.tistory.com/463)

[๊ธฐ๋ก๊ณต๊ฐ](https://lipcoder.tistory.com/459)
