# 🟨 Monotonic Stack / Queue Practice

**단조 증가/감소 스택 및 큐를 활용한 O(N) 최적화 문제 10선**  
핵심 로직은 “앞의 값을 버리거나 유지하면서” 조건을 만족하는 범위 추출

---

## ✅ 문제 1. 오큰수

**설명**: 자기 오른쪽에 있는 수 중 처음으로 자기보다 큰 수를 출력. 없으면 -1

**입력**
```
4
3 5 2 7
```

**출력**
```
5 7 7 -1
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
res = [-1] * n
stack = []

for i in range(n):
    while stack and a[stack[-1]] < a[i]:
        res[stack.pop()] = a[i]
    stack.append(i)

print(*res)
```

---

## ✅ 문제 2. 오큰수 위치 반환

**설명**: 자기보다 큰 수가 나오는 위치(index)를 출력. 없으면 0

**입력**
```
4
3 5 2 7
```

**출력**
```
2 4 4 0
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
res = [0] * n
stack = []

for i in range(n):
    while stack and a[stack[-1]] < a[i]:
        res[stack.pop()] = i + 1
    stack.append(i)

print(*res)
```

---

## ✅ 문제 3. 오큰수 반대로 (오작은수)

**설명**: 왼쪽에서 자기보다 작은 수가 처음 나오는 값. 없으면 -1

**입력**
```
5
10 9 8 11 7
```

**출력**
```
-1 -1 -1 8 -1
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
res = [-1] * n
stack = []

for i in range(n):
    while stack and a[stack[-1]] >= a[i]:
        stack.pop()
    if stack:
        res[i] = a[stack[-1]]
    stack.append(i)

print(*res)
```

---

## ✅ 문제 4. 스택으로 푸는 히스토그램 최대 직사각형

**설명**: 높이 배열에서 만들 수 있는 가장 큰 직사각형 넓이 출력

**입력**
```
7
2 1 4 5 1 3 3
```

**출력**
```
8
```

**풀이**
```python
n = int(input())
heights = list(map(int, input().split())) + [0]
stack = []
max_area = 0

for i, h in enumerate(heights):
    while stack and heights[stack[-1]] > h:
        height = heights[stack.pop()]
        width = i if not stack else i - stack[-1] - 1
        max_area = max(max_area, height * width)
    stack.append(i)

print(max_area)
```

---

## ✅ 문제 5. 슬라이딩 윈도우 최소값

**설명**: 길이 k의 윈도우를 오른쪽으로 이동하면서 최소값 출력

**입력**
```
7 3
1 3 -1 -3 5 3 6
```

**출력**
```
1 -1 -3 -3 3
```

**풀이**
```python
from collections import deque

n, k = map(int, input().split())
a = list(map(int, input().split()))
dq = deque()
res = []

for i in range(n):
    while dq and a[dq[-1]] > a[i]:
        dq.pop()
    dq.append(i)
    if dq[0] <= i - k:
        dq.popleft()
    if i >= k - 1:
        res.append(a[dq[0]])

print(*res)
```

---

## ✅ 문제 6. 길이 제한 없는 오큰수 합계

**설명**: 오른쪽으로 가면서 자기보다 큰 수가 처음 나오는 위치까지 그 수로 채워서 합을 구하라.

**입력**
```
4
3 5 2 7
```

**출력**
```
5 7 7 0
```

(총합은 5 + 7 + 7 + 0 = 19)

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
res = [0] * n
stack = []

for i in range(n):
    while stack and a[stack[-1]] < a[i]:
        res[stack.pop()] = a[i]
    stack.append(i)

print(*res)
print(sum(res))
```

---

## ✅ 문제 7. 온도 상승일 계산

**설명**: 각 날의 온도가 주어질 때, 며칠 뒤에 더 따뜻한 날이 오는지 구하라. 없으면 0

**입력**
```
8
73 74 75 71 69 72 76 73
```

**출력**
```
1 1 4 2 1 1 0 0
```

**풀이**
```python
temps = list(map(int, input().split()))
n = len(temps)
res = [0] * n
stack = []

for i in range(n):
    while stack and temps[i] > temps[stack[-1]]:
        idx = stack.pop()
        res[idx] = i - idx
    stack.append(i)

print(*res)
```

---

## ✅ 문제 8. 이전보다 높은 빌딩 수

**설명**: 왼쪽부터 오른쪽으로 갈 때, 자신보다 낮은 빌딩을 볼 수 있는 개수 출력

**입력**
```
5
6 9 5 7 4
```

**출력**
```
0 1 0 2 0
```

**풀이**
```python
a = list(map(int, input().split()))
n = len(a)
res = [0] * n
stack = []

for i in range(n):
    cnt = 0
    while stack and a[stack[-1]] <= a[i]:
        cnt += 1
        stack.pop()
    if stack:
        cnt += 1
    res[i] = cnt
    stack.append(i)

print(*res)
```

---

## ✅ 문제 9. 투포인터 + 모노톤 윈도우 최대합 조건 유지

**설명**: 수열에서 연속된 구간 중 최댓값과 최솟값 차이가 K 이하인 최대 구간 길이를 출력

**입력**
```
8 3
1 3 5 7 9 2 4 6
```

**출력**
```
3
```

**풀이**
```python
from collections import deque

n, k = map(int, input().split())
a = list(map(int, input().split()))
min_dq = deque()
max_dq = deque()
left = res = 0

for right in range(n):
    while min_dq and a[min_dq[-1]] > a[right]:
        min_dq.pop()
    while max_dq and a[max_dq[-1]] < a[right]:
        max_dq.pop()
    min_dq.append(right)
    max_dq.append(right)

    while a[max_dq[0]] - a[min_dq[0]] > k:
        left += 1
        if min_dq[0] < left:
            min_dq.popleft()
        if max_dq[0] < left:
            max_dq.popleft()
    res = max(res, right - left + 1)

print(res)
```

---

## ✅ 문제 10. 모노톤 큐를 이용한 최소 구간합

**설명**: 수열과 목표 합 S가 주어질 때, 합이 S 이상인 가장 짧은 구간의 길이를 출력하라.

**입력**
```
5 11
1 2 3 4 5
```

**출력**
```
3
```

**풀이**
```python
n, s = map(int, input().split())
a = list(map(int, input().split()))
left = 0
total = 0
res = n + 1

for right in range(n):
    total += a[right]
    while total >= s:
        res = min(res, right - left + 1)
        total -= a[left]
        left += 1

print(res if res != n + 1 else 0)
```

---


