---
layout: post
title: "2021-06-12-CODE-REVIEW"
date: 2021-06-12
tags: Self-CodeReview
color: rgb(255,90,90)
author: KIM-JS-95
subtitle: '2021-06-12-Self Code REVIEW'
---

# 2021-06-12-CODE-REVIEW

## What did you do?

1. Room func ๊ตฌํ(Add/ inquire / Modify)
2. AcceptControllerTest Modify
3. AdminController.checkOut() func Modify (Now)


### AcceptControllerTest Mvc Error

*  ๐คฆโโCause

mvc ๋๋ฉ์ธ ์ํ์ ๋ํด ์ง์์ ์ผ๋ก 400์๋ฌ(BadRequest)๊ฐ ๋ฐ์
<b> @RequestBody Annotation </b> ์ ์์ธ์ผ๋ก ํ์ธ 

 <b>Code</b>
```bash
  mvc.perform(post("/accept/100")
    .contentType(MediaType.APPLICATION_JSON))
      .andExpect(status().isOk());
```


<b>Return</b>
```bash
 Required request body is missing: public com.HotelService.entity.Admin com.HotelService.controller.
             AcceptController.checkIn(java.lang.String,com.HotelService.entity.AcceptDTO

MockHttpServletRequest:
      HTTP Method = POST
      Request URI = /accept/100
       Parameters = {}
          Headers = [Content-Type:"application/json"]
             Body = <no character encoding set>
    Session Attrs = {}

```

---
* ๐โโDebuging
  
  @RequestBody Annotation์ ์ฌ์ฉํ  ๊ฒฝ์ฐ ์์ฒญ ๋ฐ์ดํฐ์ ์ ํฉํ json Data๋ฅผ ์ ์กํด์ค์ผํจ
  

  <b>Code</b>
```bash
  mvc.perform(post("/accept/100")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\n" +
                        "  \"email\" : \"baugh248730@gmail.com\",\n" +
                        "  \"name\": \"kim\",\n" +
                        "  \"phonenum\": \"010-1234-5677\",\n" +
                        "  \"people\": \"10\"\n" +
                        "}\n"))
                .andExpect(status().isOk());
```


### ์๋ฐฉํฅ Mapping ๊ณผ N+1 Error
์๋ฐฉํฅ Mapping์ผ๋ก Room Entity๋ก๋ถํฐ Admin Entity์ Id์ปฌ๋ผ์ ์กฐํํ์ฌ Delete ๊ธฐ๋ฅ์ ๊ตฌํํ๋ ๊ณผ์ ์์

*  ๐คฆโโCause

<b> N+1 Error</b> ๊ฐ ๋ฐ์ํ์ฌ ๋ Entity ์ฌ์ด์ ๋ฌดํ ์กฐํ๊ฐ ๋ฐ์  

```bash
public class Admin {

    @Id
    @GeneratedValue
    private Long id;
    private String email;
    private String name;
    private String phonenum;
    private String people;
    @OneToOne
    @JoinColumn(name="ROOM_roomnum")
    private Room room;

public class Room {

    @Id
    private String roomnum;
    private String bedtype;
    private String st;
    @OneToOne
    private Admin admin;
}
```

```bash
public class Room {

    @Id
    private String roomnum;
    private String bedtype;
    private String st;
    @OneToOne
    private Admin admin;
}

```
* ๐โโDebuging
์๋ ๋ธ๋ก๊ทธ๋ฅผ ๋ฐ๋ผ N+1 Mapping Error ์ ํด๊ฒฐํ๋ ๋ฐฉ๋ฒ์ 2๊ฐ์ง ๋ฐฉ๋ฒ์ ์ ์ํ๋ค.
  * @OnetoOne ๋จ๋ฐฉํฅ ์ผ๋ก Mapping ํ  ๊ฒ 
  * @OnettoMany ์๋ฐฉํฅ์ผ๋ก Mapping ํ  ๊ฒ
  

https://ckdgus.tistory.com/75
### My GitHub link
[ํด๋น ํ๋ก์ ํธ ๋งํฌ](https://github.com/KIM-JS-95/AbstractCnS.git)