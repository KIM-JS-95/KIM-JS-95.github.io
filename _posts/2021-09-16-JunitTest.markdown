---
layout: post 
title: "2021-09-16-CODE-REVIEW"
date: 2021-09-17
tags: Self-CodeReview @MockBean(JpaMetamodelMappingContext.class)
color: rgb(255,90,90)
author: KIM-JS-95 
subtitle: '2021-09-16-Self Code REVIEW'
---

# CODE-REVIEW

## What did you do?

엔티티에 `@EnableJpaAuditing`을 구성하여 자동으로 현재 시간이 입력되도록 구성하였다. 

Junit Test를 통해 구성 테스트를 체크하는 과정에서 `extend class` 내부의 Auditing 메소드는 @Builder
를 할 수 없다는 것을 알게 되었다. 때문에 이 과정은 무시 해주어야 하는 부분인다.

다음 어노테이션을 통해 해당 문제를 해결할 수 있었다.

### @MockBean(JpaMetamodelMappingContext.class)

`@EnableJpaAuditing`를 적용하는 경우 사용해야할 어노테이션이며
테스트에서 데이터베이스를 사용하지 않았기 때문에 Spring에서이 예외가 발생한다.
JpaMetamodelMappingContext 클래스를 적용하면 필요한 메타 모델을 우회 할 수 있다.

### JpaAuditing Junit Test 구성

```java
@MockBean(JpaMetamodelMappingContext.class)
@RunWith(SpringRunner.class)
@WebMvcTest(GallaryRestController.class)
public class GallaryRestControllerTest {

    @Autowired
    private MockMvc mvc;

    @MockBean
    private GallaryService gallaryService;

    @Test
    public void create() throws Exception{
        String title = "Hello";
        String content = "hi";
        String type = "잡담";

        Gallary gallary = Gallary.builder()
                .id(1L)
                .title(title)
                .content(content)
                .type(type)
                .build();

        given(gallaryService.create(gallary)).willReturn(gallary);

        mvc.perform(post("/gallary/create")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"title\" : \"1\", \"content\" : \"1\", \"type\" : \"잡담\"}"))
                .andExpect(status().isOk());

    }

}
```

### Reference
[Class JpaMetamodelMappingContext](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/mapping/JpaMetamodelMappingContext.html)