---
layout: post 
title: "kakao map"
date: 2022-04-13 10:05:21 +0800
tags: kakao
color: rgb(98,170,255)
subtitle: 카카오 맵
---
 
# 프로젝트 준비
> Spring Boot 환경에서 개발
 
카카오 에서는 `카테고리로 지역검색 api`를 지원해 주고있다.

다음과 같은 조건으로 개발을 시작한다.
1. kakao api를 사용할 것
2. 접속한 사용자의 위치 정보를 획득하여 반경(2000m) 이내의 충전소 정보를 제공한다.
3. 사용자와 목표 지점까지의 거리를 계산하여 표시한다.  


## ✖ 문제 발생 ✖ - 충전소 현황 웹 파싱 불가 

api 호출은 문제가 없었지만 좌표를 지도에 표시하고 **디테일 정보를 추가하는 과정에서 문제가 발생했다.**

**카카오 에서 제공하는 detail 페이지는 완전하게 불러올 수 없다는 것이다.**

해당 방법에 대해 kakao 포럼에 질문 했지만 자신들도 외부 CP를 사용하기 때문에 정보를 제공할 수 없다는 문제였다.

[직접문의 - https://devtalk.kakao.com/t/kakao-map-jsoup/123707/3](https://devtalk.kakao.com/t/kakao-map-jsoup/123707/3)
![image](https://user-images.githubusercontent.com/65659478/177565817-472aa01c-b994-400b-b030-e8e4f302d6e5.png)


그 즉은 어떤 언어로든지 `파싱`은 불가능 하다는 것이였다.


`POSTMan`으로 url를 직접호출할 경우
```html
<!DOCTYPE html>
<html lang="ko">

...

<body>
	<div id="kakaoIndex">
		<a href="#kakaoBody">본문 바로가기</a>
		<a href="#kakaoGnb">메뉴 바로가기</a>
	</div>
	<div id="kakaoWrap" class="wrap_mapdetail">
		<!-- 스크롤이 내려 왔을 때 클래스 lbar_on  -->
		<div id="wrapMinidaum"></div>
	</div>
	<div id="daumWrap" style="display:none;">
		<div id="daumContent">
			<div id="shareContainer"></div>
		</div>
	</div>
	<script type="text/javascript" src="//ssl.daumcdn.net/dmaps/map_js_init/v3.js"></script>
	<script type="text/javascript" src="//t1.daumcdn.net/tiara/js/v1/tiara.min.js"></script>
	<script type="text/javascript" src="//t1.daumcdn.net/daumtop_deco/socialshare/socialshare_pc-2.4.3.js"></script>
	<script>
		window.ENV = 'PROD';
        window.browserversion = 'none0';
        
        // 티아라 초기화
        try {
            TiaraTracker.getInstance().init({
                svcDomain: 'place.map.kakao.com',
                deployment: window.ENV === 'PROD' ? 'production' : 'dev'
            });
        } catch(e) {}
        window.placeRestrictType = 'NONE'
        window._cp = '';
	</script>



	<script type="text/javascript" src="//s1.daumcdn.net/svc/attach/U0301/cssjs/mm/1482483925476/Chart.min.js"></script>
	<script type="text/javascript" src="//t1.daumcdn.net/kakaomapweb/place/jscss/pc.cade37dd.js"></script>

</body>
</html>
```

결과적으로 당장 구현할 수 있는 방법은 window.open(url) 방식으로 충전소 현황을 불러오는 방식이였고 나름 만족한다.

## My Code

### Spring Restcontroller
```java

@GetMapping("/kakao_url/{lat}/{lng}")
public JSONArray kakao_charge(@PathVariable("lat") String lat, @PathVariable("lng") String lng) throws IOException, ParseException {

        /*
        lat( x 좌표 ) lng( y 좌표 ) 가 카카오에서는 두 값을 바꾸어 입력해야 정상값이 도출된다.
         */
        StringBuilder urlBuilder = new StringBuilder(url); /*URL*/
        urlBuilder.append("?" + URLEncoder.encode("page", "UTF-8") + "=" + 1);
        urlBuilder.append("&" + URLEncoder.encode("sort", "UTF-8") + "=" + "accuracy");
        urlBuilder.append("&" + URLEncoder.encode("query", "UTF-8") + "=" + URLEncoder.encode("전기차충전소", "UTF-8"));
        urlBuilder.append("&" + URLEncoder.encode("x", "UTF-8") + "=" + lng);
        urlBuilder.append("&" + URLEncoder.encode("y", "UTF-8") + "=" + lat);
        urlBuilder.append("&" + URLEncoder.encode("radius", "UTF-8") + "=" + 2000); // 1km
        urlBuilder.append("&" + URLEncoder.encode("size", "UTF-8") + "=" + 15); // 0< x < 15


        return kakaoservice.getMap(urlBuilder);
        }
```

**위에서 제시한 조건**에서 반경 2000m 이내의 충전소 위치를 불러오는 방식은 kakao api 입력 정보 문서에 잘 나와 있으니 확인하자.


### JS Geolocation

```javascript
    function onGeoError() {
        alert("Can't find you. No weather for you.");
    }
    
    function init() {
        navigator.geolocation.getCurrentPosition(fetchData, onGeoError);
    }
    
    async function fetchData(position) {
        
        var mapContainer = document.getElementById('map'), // 지도를 표시할 div
        mapOption = {
        center: new kakao.maps.LatLng(position.coords.latitude, position.coords.longitude), // 지도의 중심좌표
        level: 3 // 지도의 확대 레벨
        };
        
        
        var map = new kakao.maps.Map(mapContainer, mapOption); // 지도를 생성합니다
        
        const lat = position.coords.latitude;
        const lng = position.coords.longitude;
        var url = "/kakao_url/" + lat + "/" + lng
        
        const response = await fetch(url);
    }
```

### View

```html
                for (let i = 0; i < len; i++) {

                    var iwContent =
                        `
                        <div class="info-wrap bg-primary w-100 p-md-5 p-4">
                            <h3>` + itemlist[i].place_name + `</h3>
                            <p class="mb-4">실시간 충전소 현황은 아래 링크를 클릭해주세요.</p>
                            <div class="dbox w-100 d-flex align-items-start">
                                <div class="icon d-flex align-items-center justify-content-center">
                                    <span class="fa fa-map-marker"></span>
                                </div>
                                <div class="text pl-3">
                                    <p>` + itemlist[i].road_address_name + `</p>
                                </div>
                            </div>
		
                            <div class="dbox w-100 d-flex align-items-center">
                                <div class="icon d-flex align-items-center justify-content-center">
                                    <span class="fa fa-phone"></span>
                                </div>
                                <div class="text pl-3">
                                    <p>+ ` + itemlist[i].phone + `</p>
                                </div>
                            </div>

                            <div class="dbox w-100 d-flex align-items-center">
                                <div class="icon d-flex align-items-center justify-content-center">
                                    <span class="fa fa-car"></span>
                                </div>
                                <div class="text pl-3">
                                    <p> `+ itemlist[i].distance + ` 미터</p>
                                </div>
                            </div>

                            <div class="dbox w-100 d-flex align-items-center">
                                <div class="icon d-flex align-items-center justify-content-center">
                                    <span class="fa fa-globe"></span>
                                </div>
                                <div class="text pl-3">
                                    <p><a href="`+ itemlist[i].place_url + `" onclick="window.open(this.href, '_blank', 'width=930, height=700'); return false;">` + itemlist[i].place_url + `</a></p>
                                </div>
                            </div>
	                    </div>`,

                        iwRemoveable = true;
                    position = {
                        content: iwContent,
                        removable: iwRemoveable,
                        latlng: new kakao.maps.LatLng(itemlist[i].y, itemlist[i].x),
                    }


                    positions.push(position)
                }
```

controller 에서 불러온 데이터를 마커에 주입하여 화면에 노출시키면 깔끔하게 적용된 모습을 볼 수 있다.
마커 이벤트의 겨우 html 태그또한 문자열로 지원되어 custom html 을 제작하여 데이터를 입력해 주면 된다.

![image](https://user-images.githubusercontent.com/65659478/177568265-69604959-53c1-4ec2-9c46-0954e0f04e25.png)

종종 나오지 않는 정보는 api에서 존재하지 않기 때문이니 문제는 없다.


모든 코드는 나의 Github 을 확인하면 된다.

## 👨‍💻 Result 👨‍💻

![image](https://user-images.githubusercontent.com/65659478/177564036-e17de689-fd07-4d3d-aa7d-c00a32727786.png)

![image](https://user-images.githubusercontent.com/65659478/177564150-50668d6b-266e-4376-a5de-82f0a2dbdfde.png)

## 후기

### 🍳 과거의 결과물
![image](https://user-images.githubusercontent.com/65659478/177570201-698d07c8-6a20-4701-a91d-b3dec7f5a998.png)

이 프로젝트는 2020년에 실패했던 [github - kakao Map](https://github.com/KIM-JS-95/KAKAOMAP) 를 다시 살려보고 싶어서 시작한 프로젝트이다.


**빈약한 코드는 리팩토링**하고 **웹 디자인에 신경**을 써가며 개발하며 느낀것은 지금 까지의 개발 공부가 헛되지 않았다는 것이였다.

`kakao api`에 대해서는 과거의 실패 경험으로 api를 다루는 것에 두려움이 있었지만 이번을 계기로 조금 익숙해진것 같다.


## Reference

- [githib](https://github.com/KIM-JS-95/Rent-Car-electtronic)
- [직접문의 - https://devtalk.kakao.com/t/kakao-map-jsoup/123707/3](https://devtalk.kakao.com/t/kakao-map-jsoup/123707/3)
