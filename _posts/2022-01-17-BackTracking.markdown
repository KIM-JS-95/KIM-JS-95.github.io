---
layout: post
title: "Back Tracking에 대해 아라보자"
date: 2022-01-17 14:05:21 +0800
tags: Algorithm Java
color: rgb(255,90,90)
author: KIM-JS-95
subtitle: 'leetcode 문제'
---

## 목표
 이건 공부해도 모르겠더라 

### BackTracking

`백트래킹 알고리즘`이 모든 경우의 수 `brute force`와 비슷해서 이해를 못하는 듯 하다.

### 누가 정리한 `백D브` 차이
#### 백트래킹 vs DFS

* `백트래킹`은 이미 지나쳐온 곳을 다시 돌아가서 다른 가능성을 시도해보는 걸 반복하는 기법으로, 반드시 DFS만으로 가능하지 않고 BFS등으로도 가능한 기법이다.
* 즉, 정리를 하자면 백트레킹은 `하나의 문제 해결 방법론`이고 이러한 <u>백트레킹을 구현하는 방법론 중 하나가 DFS이다.</u>

#### 백트래킹 vs 브루트포스

* `브루트 포스`는 모든 경우의 수를 탐색하는 문제 해결 방법론이다.
* 이와 달리, 백트래킹은 단계마다 조건을 충족하는지 검사하여 <u>조건을 충족하는 경우에만 계속해서 탐색한다.</u>
* 즉, 정리를 하자면 브루트포스 모든 가지를 탐색하는 방법론이고 백트레킹을 조건을 보고 가지치기를 해나가면서 탐색하는 방법론이다.

### 문제로 풀어보자

#### N-Queen
```java
import java.io.*;

public class Main {

    static int n;
    static int[] map;
    static int count=0;
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        n = Integer.parseInt(br.readLine());

        for(int i=1; i<n+1; i++) {
            map = new int[n+1];
            map[1] = i; // (1,i)에 queen
            dfs(2);
        }

        System.out.println(count);
    }



    static void dfs(int depth) {

        if(depth > n) {
            count++;
        }
        else {
            for(int i=1; i<n+1; i++) {
                map[depth] = i; // (depth,i)에 queen 
                if(check(depth)) {
                    dfs(depth+1);
                }
                // 없어도 됨. 이미 map[depth] = i에서 부모 퀸의 위치가 초기화 되었기 때문이다.
                // map[depth] = 0;   
            }
        }
    }

    static boolean check(int depth) {

        for(int i=1; i<depth; i++) {
            // 열이 같으면 false
            if(map[i] == map[depth]) return false;

            // 00 01 02 03
            // 10 11 12 13
            // 20 21 22 23
            // 30 31 32 33

            // (/\) 양방향 대각선 
            if(Math.abs(i-depth) == Math.abs(map[i]-map[depth])) return false;

        }
        return true;
    }
}
```


## 관련 포스팅 (LeetCode)
* [범범스의 코딩놀이터](https://bumbums.tistory.com/3)
* [트래킹 vs DFS vs 브루트포스](https://github-wiki-see.page/m/joi0104/BOJ/wiki/%EB%B0%B1%ED%8A%B8%EB%9E%98%ED%82%B9-vs-DFS-vs-%EB%B8%8C%EB%A3%A8%ED%8A%B8%ED%8F%AC%EC%8A%A4)
