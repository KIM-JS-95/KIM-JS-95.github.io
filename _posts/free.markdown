## 🚀 카카오 실패율 풀이

### 🌠 내 풀이(틀린것)

```python
def solution(N, stages):
    answer = []
    player = [0] * (max(stages)+1)
    length = len(stages)

    for i in stages:
        player[i] += 1


    j=0
    for i in player:
        j +=1
        if length == 0:
            fail = 0
        else:
            fail = i / length

        answer.append((j,fail))
        length -= i
    
    answer = sorted(answer, key=lambda t: t[1], reverse=True)
    answer = [i[0] for i in answer]
    return answer

n = 5
stages = [2, 1, 2, 6, 2, 4, 3, 3]
solution(n,stages)
```

### 🌠 정확한 풀이 
```python
def solution(N, stages):
    answer = []
    length = len(stages)


    for i in range(1, N + 1):
        count = stages.count(i)

        if length == 0:
            fail = 0
        else:
            fail = count / length

        answer.append((i, fail))
        length -= count

    answer = sorted(answer, key=lambda t: t[1], reverse=True)
    answer = [i[0] for i in answer]
    return answer

n = 5
stages = [2, 1, 2, 6, 2, 4, 3, 3]
solution(n,stages)
```

### 🌠 개선

계수 정렬로 접근하여 계산하는 시도는 좋았지만 시간 복잡도가 `O(2*N)` 이며  실패율 계산은 정확했지만 출력을 위한
`answer.append()`에 문제가 있었다.

특히나 반복문은 `player`배열 요소를 하나씩 출력하기보다 1 ~ N+1 으로 반복하여 계산하는 것이 좋았다.

계수 정렬 코드의 개선점으로 파이썬 에서는 `arr.count(요소)` 메소드가 있다.

반복문 내부에 계수정렬 메소드를 한번에 넣는 다면 충분히 `O(N)` 복잡도를 구현할 수 있다.


### 🌠 손 코딩
![KakaoTalk_20220312_142340458](https://user-images.githubusercontent.com/65659478/158005102-10129c96-b311-4992-9ce4-0b3144a96ba0.jpg)