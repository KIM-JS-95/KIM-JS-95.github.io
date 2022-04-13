## 🚀 #15. 3SUM

### 🌠 내 풀이 (3 포인터)


```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:

        # 3 포인터??

        answer=[]
        nums.sort()

        for i in range(len(nums) -2):
            p1 = 0
            p2 = i+1
            p3=len(nums)-1

            while p1<p2 and p2<p3:

                if nums[p1] + nums[p2]+nums[p3] == 0:
                    if [nums[p1] , nums[p2], nums[p3]] not in answer:
                        answer.append([nums[p1], nums[p2], nums[p3]])

                    p1 +=1
                    p3 -=1

                elif nums[p1] + nums[p2]+nums[p3] < 0:
                    p1 +=1

                else:
                    p3 -=1
        return answer
```

### 🌠 2포인터 도 가능 제안 풀이 (2 포인터) 

```python
class Solution:
	def threeSum(self, nums: List[int]) -> List[List[int]]:
		#we can use a set so that we make sure that no duplicates are added
		output = set()
		nums.sort()
		for i in range(len(nums)):
			l, r = i + 1, len(nums) - 1
			while l < r:
				if nums[i] + nums[l] + nums[r] == 0:
					output.add((nums[i], nums[l], nums[r]))
					l += 1
					r -= 1
				elif nums[i] + nums[l] + nums[r] > 0:
					r -= 1
				else:
					l += 1
		return output

```

### 🌠  과정

3개의 포인터를 이동해 가며 합이 0인 것을 찾는 방식으로
p1, p3 포인터 범위 내부에서 p2를 `for문`으로 순회하며 찾아내는 과정이다.

`코드레시피` 사이트에 따르면 **시간 복잡도가 O(n^2)**이지만
단순 `브루트 포스`를 사용할 경우 `3중 for문`을 사용으로 **시간 복잡도가 O(n^3)** 가 된다.


### 🌠 손 코딩

![KakaoTalk_20220316_155138405](https://user-images.githubusercontent.com/65659478/158533360-421c2416-01c6-4ce2-8282-41f7592d5747.jpg)
