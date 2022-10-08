---
layout: post 
title:  "Kadane's_Algorithm"
date:   2021-12-06 12:05:21 +0800 
tags: ALGORITHM
color: rgb(154,133,255)
subtitle: '최대 부분 합 구하기'
--- 

## 최대 부분 합 구하기

SSAFY 7기 CT 문제에 출제되었던 문제로 1차원 배열에서 가장 큰 값을 가질 수 있는 연속적인 부분 배열을 찾는 문제에 사용되는 알고리즘이다.

많은 문제들이 시간, 공간 복잡성이 적은 카디안 알고리즘을 적용한 문제풀이를 선호한다.

카디안 알고리즘에서는 시,공간 복잡성이 **O(n)** 으로 다음과 같이 구현한다.


[LeetCode 53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) 문제 링크
```java
public int maxSubArray(int[]nums){
        int sum=0;
        int max=Integer.MIN_VALUE;

        for(int i=0;i<nums.length;i++){
            sum+=nums[i];
            max=Math.max(sum,max);
            if(sum< 0)
            sum=0;
        }
        return max;
}
```
전 배열을 순회하면서 값을 하나씩 더해 현재 위치 까지의 배열의 최대 값과 비교하여 더 큰값을 가져간다.

확인해야 할 점은 배열에 음수 값이 존재한다는 점으로 sum 값을 구할 때 마다 0보다 작을 경우 0으로 바꾸어 주어야한다.


![](https://assets.leetcode.com/users/images/2715e7ab-8fad-4f5f-ace9-cfe83acba68f_1637809592.7767313.jpeg)


### 🧾Reference
[카데인 알고리즘](https://www.geeksforgeeks.org/largest-sum-contiguous-subarray/)