## ๐ #15. 3SUM

### ๐  ๋ด ํ์ด (3 ํฌ์ธํฐ)


```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:

        # 3 ํฌ์ธํฐ??

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

### ๐  2ํฌ์ธํฐ ๋ ๊ฐ๋ฅ ์ ์ ํ์ด (2 ํฌ์ธํฐ) 

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

### ๐   ๊ณผ์ 

3๊ฐ์ ํฌ์ธํฐ๋ฅผ ์ด๋ํด ๊ฐ๋ฉฐ ํฉ์ด 0์ธ ๊ฒ์ ์ฐพ๋ ๋ฐฉ์์ผ๋ก
p1, p3 ํฌ์ธํฐ ๋ฒ์ ๋ด๋ถ์์ p2๋ฅผ `for๋ฌธ`์ผ๋ก ์ํํ๋ฉฐ ์ฐพ์๋ด๋ ๊ณผ์ ์ด๋ค.

`์ฝ๋๋ ์ํผ` ์ฌ์ดํธ์ ๋ฐ๋ฅด๋ฉด **์๊ฐ ๋ณต์ก๋๊ฐ O(n^2)**์ด์ง๋ง
๋จ์ `๋ธ๋ฃจํธ ํฌ์ค`๋ฅผ ์ฌ์ฉํ  ๊ฒฝ์ฐ `3์ค for๋ฌธ`์ ์ฌ์ฉ์ผ๋ก **์๊ฐ ๋ณต์ก๋๊ฐ O(n^3)** ๊ฐ ๋๋ค.


### ๐  ์ ์ฝ๋ฉ

![KakaoTalk_20220316_155138405](https://user-images.githubusercontent.com/65659478/158533360-421c2416-01c6-4ce2-8282-41f7592d5747.jpg)
