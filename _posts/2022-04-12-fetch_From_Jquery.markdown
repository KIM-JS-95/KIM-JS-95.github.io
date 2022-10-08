---
layout: post 
title: "제이쿼리를 떠나 순수JS로!"
date: 2022-04-12 10:05:21 +0800
tags: AJAX 자바스크립트 제이쿼리
color: rgb(98,170,255)
subtitle: 제이쿼리를 보내주자
---
 
# 제이쿼리를 보내드리자

지난 포스팅에서 ES6의 등장과 함께 `fetch 통신`이 가능해 졌다는 이야기였다.

그래서 이번 포스팅에서 내가 제이쿼리에서 fetch 형식의 문법으로 바꾸려한다.

변경 대상은 다음 Repository를 통해 구현한다.
    - [게임 게시판](https://github.com/KIM-JS-95/CutLinePages.git)

## Post 변경

### 제이쿼리

``` javascript

$('#save').on('click', function () {
    var data={
             title: $('#title').val(),
             content: $('#content').val(),
             link: $('#link').val()
         };

     $.ajax({
         type: 'POST',
         url: '/gallary/create',
         contentType:'application/json; charset=utf-8',
         data: JSON.stringify(data)
     }).done(function(){
      alert('저장 성공');
      window.location.href='/home/guest'
     }).fail(function (error) {
     alert(JSON.stringify(error));
     });
 });

```

### fetch

``` javascript

    const save = document.getElementById("save");
    save.addEventListener("click", save_func);

    function save_func() {

        const title = document.getElementById("title").value;
        const content = document.getElementById("content").value;
        const link = document.getElementById("link").value;

        var data = {
            title: title,
            content: content,
            link: link
        };

        fetch("/gallary/create", {
            method: "POST", // POST
            headers: { // 헤더 조작
                "Content-Type": "application/json;",
            },
            body: JSON.stringify(data),
        })
            .then(alert('저장 성공'))
            .then(function (res) {
                if (res.ok) {
                    window.location.href = '/home/guest';
                }
            })
    }

```

핵심은
 `Id`를 읽어와 `addEventListener`로 함수를 호출하는 방법과 개별 변수값을 읽어오는 과정이다.

 자바스크립트가 DOM 에서 input / text 등의 값을 받으려면 `값을 호출하는` 명령어가 필요하다.
 꼭 알고 넘어가야하는 부분은 제이쿼리과 자바스크립트가 값을 호출하는 메소드가 다르다는 점 이란것이다.

 |제이쿼리|자바스크립트|
 |:---:|:---:|
 |.val()|.value|



## GET 변경 (학습중 )
    
### 제이쿼리
### 자바 스크립트


## PUT 변경

### 제이쿼리

```javascripts
    $('#update').on('click', function () {
    
        if (confirm("수정하시겠습니까?") == true) {
    
            var id = document.getElementById("id1").innerText;
    
            var data = {
                title: $('#title').val(),
                content: $('#content').val(),
                link: $('#link').val()
            };
    
            $.ajax({
                type: 'PUT',
                url: '/gallary/update/' + id,
                contentType: 'application/json; charset=utf-8',
                data: JSON.stringify(data)
            }).done(function () {
                alert('저장 성공');
                window.location.href = '/gallary/view/' + id;
            }).fail(function (error) {
                alert(JSON.stringify(error));
            });
        }
    });
```
### 자바 스크립트

```javascript
   const updata_val = document.getElementById("updata");
    updata_val.addEventListener("click", updata_fun);


    function updata_fun() {

        const title = document.getElementById("title").value;
        const content = document.getElementById("content").value;
        const link = document.getElementById("link").value;

        var data = {
            title: title,
            content: content,
            link: link
        };

        fetch('/gallary/update/' + id, {
            method: "PUT",
            headers: {
                "Content-Type": "application/json;"
            },
            body: JSON.stringify(data),
        })
            .then(alert("수정되었습니다."))
            .then(function (res) {
                if (res.ok) {
                    window.location.href = '/home/guest';
                }
            })
    }
```


## DELETE 변경

### 제이쿼리

```javascript
$('#delete').on('click', function() {
if(confirm("삭제하시겠습니까?")==true){

 var id = document.getElementById("id1").innerText;

              $.ajax({
               type: 'DELETE',
               url: '/gallary/delete/' + id,
           }).done(function(){
           alert('글이 삭제되었습니다.');
            window.location.href='/home/guest'
           }).fail(function (error) {
           alert(JSON.stringify(error));
     });
}
});
```

### 자바 스크립트

```javascript
    const dele = document.getElementById("delete");
    dele.addEventListener("click", delete_function);


    function delete_function() {
        if (confirm("삭제하시겠습니까?") == true) {
            var id = document.getElementById("id1").innerText;

            fetch('/gallary/delete/' + id, {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json;"
                }
            })
                .then(alert('글이 삭제되었습니다.'))
                .then(function (res) {
                    if (res.ok) {
                        window.location.href = '/home/guest';
                    }
                })
        }
    };
```
## 정리

두 방식의 AJAX 방식을 표현하자면 <b>제이쿼리는 윈도우 / 자바스크립트는 리눅스</b> 같았다.

분명 제이쿼리 라이브러리에서 제공하는 DOM 을 다루는 기능을 많지만
모든것을 제이쿼리로 구현하기보다는 처음부터 JS로 구현하는 것이 시간적으로 이점이 강하다 생각한다.

또한 JS가 진화하면서 기본 언어를 이해하고 있는 주니어 개발자라면 1주일만 학습해도 충분히 `fetch 기능`을 구현할 수 있을 것이다.


## Repository
- [게임 게시판](https://github.com/KIM-JS-95/CutLinePages.git)

## Reference
- [Ajax, fetch](https://velog.io/@ksh4820/Ajax-fetch)
- [AJAX 서버 요청 및 응답 fetch api 방식](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-AJAX-%EC%84%9C%EB%B2%84-%EC%9A%94%EC%B2%AD-%EB%B0%8F-%EC%9D%91%EB%8B%B5-fetch-api-%EB%B0%A9%EC%8B%9D)