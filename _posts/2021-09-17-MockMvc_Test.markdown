---
layout: post title: "2021-09-17-CODE-REVIEW"
date: 2021-09-17 
tags: MvcTest SpringBoot
color: rgb(255,90,90)
author: KIM-JS-95 
subtitle: '@WebMvcTest와 @SpringBootTest가 같이 사용될 수 없는 이유'
---

## 서론

내가 처음 Springboot 를 배울때 mvc 테스트 하는 방식에 대해 방법이 많았지만  모두 이해할 수 없었다.

이제는 MVC 테스트에 대해 정리가 필요하다 느껴 정리해본다.

## 본론

### 1. @WebMvcTest

컨트롤러의 정확한 동작 여부를 확인하기 위한 도구 때문에 `@WebMvcTest(대상컨트롤러.class)` 처럼 명시해줘야한다.

- @WebMvcTest 어노테이션을 사용시 다음 내용만 스캔 하도록 제한한다. (보다 가벼운 테스팅이 가능하다.)

```text
    @Controller
    @ControllerAdvice,
    @JsonComponent,
    Converter,
    GenericConverter,
    Filter,
    HandlerInterceptor,
    WebMvcConf
```

● 장점

- WebApplication 관련된 Bean들만 등록하기 때문에 통합 테스트보다 빠르다.
- 통합 테스트를 진행하기 어려운 테스트를 진행 가능하다.

ex) 결제 모듈 API를 콜하며 안되는 상황에서 Mock을 통해 가짜 객체를 만들어 테스트 가능.

● 단점
- 요청부터 응답까지 모든 테스트를 Mock 기반으로 테스트하기 때문에 실제 환경에서는 제대로 동작하지 않을 수 있다.


다음은 [갓대희](https://goddaehee.tistory.com/211) 님의 테스트 예시를 보자

```java

//@RunWith(SpringRunner.class) // ※ Junit4 사용시
@WebMvcTest(TestRestController.class) 
@Slf4j 
class TestRestControllerTest { 
    
    @Autowired 
    MockMvc mvc; 
    
    @MockBean
    private TestService testService;
    
    @Test
    void getListTest() throws Exception { 
        
        //given
        TestVo testVo = TestVo.builder() 
                .id("goddaehee") 
                .name("갓대희") 
                .build(); 
        
        //given
        given(testService.selectOneMember("goddaehee")) 
                .willReturn(testVo);
        
        
        //when
        final ResultActions actions = mvc.perform(get("/testValue2") 
                .contentType(MediaType.APPLICATION_JSON)) 
                .andDo(print());
        
        
        //then 
        actions .andExpect(status().isOk())
                .andExpect(content()
                .contentType(MediaType.APPLICATION_JSON)) 
                .andExpect(jsonPath("$.name", is("갓대희"))) 
                .andDo(print());
    }
}

```



### 2. @SpringBootTest

Spring Boot 의 통합테스트 어노테이션으로 `단위테스트`와 같이 기능 검증을 위한 도구가 아닌 전체적으로 흐름에 맞게 동작하는지 
검증하기 위해 사용된다.

● 장점
- 애플리케이션의 설정, 모든 Bean을 모두 로드하기 때문에 운영환경과 가장 유사한 테스트가 가능하다.
- 전체적인 Flow를 쉽게 테스트 가능하다.

● 단점
- 애플리케이션의 설정, 모든 Bean을 모두 로드하기 때문에 시간이 오래걸리고 무겁다.
- 테스트 단위가 크기 때문에 디버깅이 어려운 편이다.



## 결론


## Reference
[MockMvc를 이용해서 테스트하기](https://elevatingcodingclub.tistory.com/61)
[@SpringBootTest](https://goddaehee.tistory.com/211)