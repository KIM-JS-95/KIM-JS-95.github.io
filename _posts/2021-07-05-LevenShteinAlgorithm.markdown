---
layout: post
title:  "Leven Shtein Algorithm"
date:   2021-07-05
tags: Algorithm
color: rgb(98,170,255)
subtitle: '편집거리 알고리즘'
---
## 편집거리 알고리즘

### Leven Shtein Algorithm
두 개의 문자열 A, B가 주어졌을 때 두 문자열이 얼마나 유사한 지를 알아낼 수 있는 알고리즘입니다. 
그러니까, 문자열 A가 문자열 B와 같아지기 위해서는 몇 번의 연산(DELETE / INSERT / REPLACE)을 진행해야 하는 지 계산할 수 있습니다.

### 예시

두 문자열 s1 , s2 가 주어진다면 s1을 s2로 만드는데 필요한 최소한의 연산을 구하라

s1 = "sunday"   /   s2 = "saturday"

분석) 
1. 두 문자가 같을 경우 어떠한 연산도 수행하지 않는다.
2. 두 문자가 다를 경우 현재 위치 d[i][j]에서 d[i-1][j], d[i][j-1], d[i-1][j-1] 의 요소중에 가장 작은 값에서
+1 을 연산한 값을 배열에 추가한다.
   
정리)
 분석 단계에서 분석한 1,2 번의 요소를 공식으로 표현하면 다음과 같다.
![1](https://user-images.githubusercontent.com/58545240/112936241-6312da80-9160-11eb-99c8-11c0980cf63a.png)

공식에 따라 최종적으로 배열에 표현한 값은 다음과 같아지며 두 문자를 동일하게 만들기 위해 필요한 
최소한의 비용은 배열의 가장 마지막 d[n][m]의 값이 된다. 

|...|.|S|U|N|D|A|Y|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|.|0|1|2|3|4|5|6|
|S|1|0|1|2|3|4|5|
|A|2|1|1|2|3|3|4|
|T|3|2|2|2|3|4|4|
|U|4|3|2|3|3|4|5|
|R|5|4|3|3|4|4|5|
|D|6|5|4|4|3|4|5|
|A|7|6|5|5|4|3|4|
|Y|8|7|6|6|5|4|3*|


### 코드

> 초기 배열 설정에서 arr[i][0] , arr[0][j]는 배열의 길이 -1 만큼 i++ / j++ 반복 연산을 해주어야 한다.


```java

public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        String s1= sc.nextLine();
        String s2 = sc.nextLine();

        int n = s1.length();
        int m = s2.length();

        int[][] dp = new int[n+1][m+1];

        for (int i = 1; i <= n; i++) {
            dp[i][0] = i;
        }
        for (int j = 1; j <= m; j++) {
            dp[0][j] = j;
        }

        for(int i=1; i<=n; i++){
            for(int j=1; j<=m; j++){

                if(s1.charAt(i-1)==s2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1];
                }else{
                    dp[i][j] = 1 + Math.min(dp[i][j - 1], Math.min(dp[i - 1][j], dp[i - 1][j - 1]));
                }
            }
        }
        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++) {
            System.out.print(dp[i][j] + " ");
            }
            System.out.println();
            }
    }
}

```

### 🧾Reference
1. [Leven Shtein Algorithm](https://madplay.github.io/post/levenshtein-distance-edit-distance)
2. [이것이 코딩 테스트다](https://github.com/ndb796/python-for-coding-test)
