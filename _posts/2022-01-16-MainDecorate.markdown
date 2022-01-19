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


## 내 채널 유튜브 영상 크롤링

🤔🤔 동영상을 어떻게 가져올까 ... 🤔🤔

초기 영상의 경우 크롤링을 통해 영상 전부 획득이 가능할까? 쉽지많은 않은 듯 하다.

사실 영상 전부를 가져오기보다 최근에 없로드 된 영상 몇개만 필요할 뿐이라서...

고민을 하다가 `Mysql`에 유튜브 URL을 저장해서 불러오는 방법은 어떨까 생각했다. 영상의 주소는 모두 저장되지만 상위 3개만을 가져와서
화면에 영상으로 띄어주면 되지 않을까? 생각한다.

자! `YouTube 공식 API`를 사용해서 내 채널 영상정보를 가져온 데이터이다.

![1](https://user-images.githubusercontent.com/65659478/149764685-29247257-f1ac-47b2-bb39-12837b1dd499.png)


<center>
 <h1>🤔🤔🤔🤔🤔🤔</h1>
생각해보니 ... 

썸네일까지 주소를 가져와 DB에 저장하면
이미지 슬라이드에 적용할 수 있는거 아님?
<h1>🤔🤔🤔🤔🤔🤔</h1>
</center>


### 슬라이드 완성

그래서! 

썸네일 주소를 저장하려 보니깐 VideoId 값만 가져와서 `YouTube 썸네일 주소`에 입력하면 깔끔하게 정리를 할 수 있다.
![ezgif com-gif-maker](https://user-images.githubusercontent.com/65659478/150127762-368d5b2e-b905-4ce6-9d35-808e3ddb2b15.gif)


### YouTube API Code

자세한 설명은 아래 `YouTube API 공식 홈페이지`에 잘 나와있다.


```java

@RestController
public class MyUploads {

    @Autowired
    YoutubeTableRepository youtubeTableRepository;

    /**
     * Define a global instance of a Youtube object, which will be used
     * to make YouTube Data API requests.
     */
    private static YouTube youtube;

    /**
     * Authorize the user, call the youtube.channels.list method to retrieve
     * the playlist ID for the list of videos uploaded to the user's channel,
     * and then call the youtube.playlistItems.list method to retrieve the
     * list of videos in that playlist.
     *
     * @param args command line args (not used).
     */
    @PostMapping("/youtube")
    public void YoutubeCrowling(String[] args) {

        // This OAuth 2.0 access scope allows for read-only access to the
        // authenticated user's account, but not other types of account access.
        List<String> scopes = Lists.newArrayList("https://www.googleapis.com/auth/youtube.readonly");

        try {
            // Authorize the request.
            Credential credential = Auth.authorize(scopes, "myuploads");

            // This object is used to make YouTube Data API requests.
            youtube = new YouTube.Builder(Auth.HTTP_TRANSPORT, Auth.JSON_FACTORY, credential).setApplicationName(
                    "youtube-cmdline-myuploads-sample").build();

            // Call the API's channels.list method to retrieve the
            // resource that represents the authenticated user's channel.
            // In the API response, only include channel information needed for
            // this use case. The channel's contentDetails part contains
            // playlist IDs relevant to the channel, including the ID for the
            // list that contains videos uploaded to the channel.
            YouTube.Channels.List channelRequest = youtube.channels().list("contentDetails");
            channelRequest.setMine(true);
            channelRequest.setFields("items/contentDetails,nextPageToken,pageInfo");
            ChannelListResponse channelResult = channelRequest.execute();

            List<Channel> channelsList = channelResult.getItems();

            if (channelsList != null) {
                // The user's default channel is the first item in the list.
                // Extract the playlist ID for the channel's videos from the
                // API response.
                String uploadPlaylistId =
                        channelsList.get(0).getContentDetails().getRelatedPlaylists().getUploads();

                // Define a list to store items in the list of uploaded videos.
                List<PlaylistItem> playlistItemList = new ArrayList<PlaylistItem>();

                // Retrieve the playlist of the channel's uploaded videos.
                YouTube.PlaylistItems.List playlistItemRequest =
                        youtube.playlistItems().list("id,contentDetails,snippet");
                playlistItemRequest.setPlaylistId(uploadPlaylistId);

                // Only retrieve data used in this application, thereby making
                // the application more efficient. See:
                // https://developers.google.com/youtube/v3/getting-started#partial
                playlistItemRequest.setFields(
                        "items(contentDetails/videoId,snippet/title,snippet/publishedAt,snippet/thumbnails/medium/url),nextPageToken,pageInfo");

                String nextToken = "";

                // Call the API one or more times to retrieve all items in the
                // list. As long as the API response returns a nextPageToken,
                // there are still more items to retrieve.
                int cnt = 0;
                do {
                    playlistItemRequest.setPageToken(nextToken);
                    PlaylistItemListResponse playlistItemResult = playlistItemRequest.execute();

                    playlistItemList.addAll(playlistItemResult.getItems());
                    nextToken = playlistItemResult.getNextPageToken();
                } while (playlistItemList.size() <= 3);

                // Prints information about the results.
                prettyPrint(playlistItemList.size(), playlistItemList.iterator());


            }

        } catch (GoogleJsonResponseException e) {
            e.printStackTrace();
            System.err.println("There was a service error: " + e.getDetails().getCode() + " : "
                    + e.getDetails().getMessage());

        } catch (Throwable t) {
            t.printStackTrace();
        }
    }

    /*
     * Print information about all of the items in the playlist.
     *
     * @param size size of list
     *
     * @param iterator of Playlist Items from uploaded Playlist
     */


    static List<YoutubeTable> list = new ArrayList<>();

    private void prettyPrint(int size, Iterator<PlaylistItem> playlistEntries) {

        while (playlistEntries.hasNext()) {
            PlaylistItem playlistItem = playlistEntries.next();

            YoutubeTable youtubeTable = YoutubeTable.builder()
                    .VideoId(playlistItem.getContentDetails().getVideoId())
                    .Title(playlistItem.getSnippet().getTitle())
                    .build();

            list.add(youtubeTable);
        }
        youtubeTableRepository.saveAll(list);

    }
}

```

## 관련 포스팅

* [YouTube crawling](https://chulkang.tistory.com/65)
* [YouTube-API](https://developers.google.com/youtube/player_parameters?hl=ko#Manual_IFrame_Embeds)
* [엔티티 매핑 최적화](https://kim-js-95.github.io/2022/01/09/%EA%B0%9D%EC%B2%B4-%EB%A7%A4%ED%95%91.html)
* [이미지 슬라이드는 요기서 확인](http://kenwheeler.github.io/slick/)



## My GitHub link
* [해당 프로젝트 링크](https://github.com/KIM-JS-95/CutLinePages)