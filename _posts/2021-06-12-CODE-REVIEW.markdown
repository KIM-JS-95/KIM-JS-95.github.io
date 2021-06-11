---
layout: post
title: "2021-06-12-CODE-REVIEW"
date: 2021-06-12
tags: Self-CodeReview MVC @RequestBody
color: rgb(255,90,90)
author: KIM-JS-95
subtitle: '2021-06-12-Self Code REVIEW'
---

# 2021-06-12-CODE-REVIEW

## What did you do?

1. Room func 구현(Add/ inquire / Modify)
2. AcceptControllerTest Modify
3. AdminController.checkOut() func Modify (Now)


## AcceptControllerTest Mvc Error

*  🤦‍♂Cause

mvc 도메인 상태에 대해 지속적으로 400에러(BadRequest)가 발생
<b> @RequestBody Annotation </b> 을 원인으로 확인 

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
* 🙆‍♂Debuging
  @RequestBody Annotation을 사용할 경우 요청 데이터에 적합한 json Data를 전송해줘야함
  

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


## 양방향 Mapping 과 N+1 Error
양방향 Mapping으로 Room Entity로부터 Admin Entity의 Id컬럼을 조회하여 Delete 기능을 구현하는 과정에서

### 원인
<b> N+1 Error</b> 가 발생하여 두 Entity 사이의 무한한 조회가 발생  

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

### My github link
[해당 프로젝트 링크](https://github.com/KIM-JS-95/AbstractCnS.git)