---
layout: post
title: "내 채널 페이지 메인화면을 꾸며보자"
date: 2022-01-16 14:05:21 +0800
tags: Self-CodeReview
color: rgb(255,90,90)
author: KIM-JS-95
subtitle: 'CutLinePage를 업데이트 하자!'
---

## 목표
![main](https://user-images.githubusercontent.com/65659478/149650552-d1e7f3e1-8ddf-4c00-b0c5-7f03e6b4ed0d.png)

🤔🤔 메인화면을 보자 하니... 🤔🤔

굉장히 허전하다. 게다가 저 화면 링크는 어떠한 기능도 없는 장식이다.

게임 관련 사이트이며 포트폴리오로 사용하기 위해 멋을 추가할 것이다.

## 이미지 슬라이드 추가

유튜브 영상에서는 썸네일을 사용하는데 메인 페이지에도 추가하면 좋을듯 하다.

```text
많은 선배 개발자분들이 이미지를 저장하는 방법은에서는 로컬저장법
AWS를 사용한다면  **AWS Bucket**을 사용한다 말한다.

그러나 난 돈이 없는 취준생이니깐 편법을 사용하겠다

Github -> issue 에서 이미지를 주소로 변환할 수 있다.

고것은 공짜 이구요.
```

### 슬라이드 완성
![제목 없음](https://user-images.githubusercontent.com/65659478/149651966-ab0ab521-4fd9-4da4-9d0d-1e042d9b1e38.png)

확실이 이전과 다르게 뭔가 있어보이는 느낌이다.

Spring을 사용하지 않고 Js / CSS / HTML으로 만들었다. 

대부분의 이미지를 `.png` 형식의 썸네일을 추가 하여 볼 수 있도록 할 예정이다.


## 내 채널 유튜브 영상 크롤링

🤔🤔 동영상을 어떻게 가져올까 ... 🤔🤔

초기 영상의 경우 크롤링을 통해 영상 전부 획득이 가능할까? 쉽지많은 않은 듯 하다.

사실 영상 전부를 가져오기보다 최근에 없로드 된 영상 몇개만 필요할 뿐이라서...

고민을 하다가 `Mysql`에 유튜브 URL을 저장해서 불러오는 방법은 어떨까 생각했다. 영상의 주소는 모두 저장되지만 상위 3개만을 가져와서
화면에 영상으로 띄어주면 되지 않을까? 생각한다.

자! `YouTube 공식 API`를 사용해서 내 채널 영상정보를 가져온 데이터이다.

![1](https://user-images.githubusercontent.com/65659478/149764685-29247257-f1ac-47b2-bb39-12837b1dd499.png)

 <h1><center>  🤔🤔🤔🤔🤔🤔 </center> </h1>

생각해보니 썸네일까지 주소를 가져와 DB에 저장하면 이미지 슬라이드에 적용할 수 있는거 아님?



## 관련 포스팅

* [YouTube crawling](https://chulkang.tistory.com/65)
* [YouTube-API](https://developers.google.com/youtube/player_parameters?hl=ko#Manual_IFrame_Embeds)
* [엔티티 매핑 최적화](https://kim-js-95.github.io/2022/01/09/%EA%B0%9D%EC%B2%B4-%EB%A7%A4%ED%95%91.html)
* [이미지 슬라이드는 요기서 확인](http://kenwheeler.github.io/slick/)



## My GitHub link
* [해당 프로젝트 링크](https://github.com/KIM-JS-95/CutLinePages)