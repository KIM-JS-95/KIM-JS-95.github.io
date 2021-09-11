---
layout: post
title:  "Ajax vs Axios"
date:   2021-09-10 14:05:21 +0800
tags: Ajax Axios JS데이터
color:  rgb(154,133,255)
subtitle: 유닛테스트 방법
---
## 서론

클라이언트와 서버 간 데이터를 주고받기 위해 HTTP 통신을 사용하며 이 과정에서 사용되는 HTTP 통신 기술 js에서 비동기 HTTP 통신을 위해 사용되는 Ajax, Axios 에 대해 설명한다.


### AJAX(Asynchronous JavaScript And XML)
JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 데이터를 주고받는 기술이다. 이전 포스팅에서 AJAX에 대한 포스팅을 한적이 있다.



#### jQuery와의 연관성

Ajax를 JQuery를 통해 보다 더 쉽게 사용할 수 있기에 우리는 JQuery와 Ajax를 함께 묶어서 말할 때가 많은 것 뿐이이다.
순수 Ajax의 코드는 지저분했기에 이에 대한 보완으로 jQuery에서 Ajax를 편리하게 사용할 수 있도록 정립하면서 jQuery의 활용이 많아진 것이다.
또한 jQuery를 사용하여 Ajax를 구현할 경우 브라우저에 구애받지 않고 동일한 코드로 같은 작업을 구현할 수 있습니다.

- 순수 Ajax 
```jS
function reqListener (e) {
    console.log(e.currentTarget.response);
}

var oReq = new XMLHttpRequest();
var serverAddress = "https://jsonplaceholder.typicode.com/posts";

oReq.addEventListener("load", reqListener);
oReq.open("GET", serverAddress);
oReq.send();
```

- Jquery 기반 Ajax
```js
var serverAddress = 'https://jsonplaceholder.typicode.com/posts';

// jQuery의 .get 메소드 사용
$.ajax({
    url: ,
    type: 'GET',
    success: function onData (data) {
        console.log(data);
    },
    error: function onError (error) {
        console.error(error);
    }
});
```


### Axios(Promise based HTTP client for the browser and node.js)

node.js와 브라우저를 위한 HTTP통신 라이브러리이다.
비동기 HTTP 통신을 구성해주며 return을 promise 객체로 해주기 때문에 response 데이터를 다루기도 쉽다.

```mysql-sql
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Yongseong',
    lastName: 'Kim'
  }
});
```



### 🧾Reference
[Ajax와 Axios 그리고 fetch](https://velog.io/@kysung95/%EA%B0%9C%EB%B0%9C%EC%83%81%EC%8B%9D-Ajax%EC%99%80-Axios-%EA%B7%B8%EB%A6%AC%EA%B3%A0-fetch)