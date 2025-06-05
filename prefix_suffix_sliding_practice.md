# 🟩 Prefix Sum / Sliding Window Practice

**누적합 + 슬라이딩 윈도우 패턴 문제 10선**  
삼성 B형 포함 코딩 테스트 구간합 문제 필수 드릴

---

## ✅ 문제 1. 부분 합 (최소 길이)

**설명**  
길이가 N인 수열에서 **부분합이 S 이상이 되는 가장 짧은 연속 부분 수열의 길이**를 구하라.  
(없으면 0 출력)

**입력**
```
10 15
5 1 3 5 10 7 4 9 2 8
```

**출력**
```
2
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

print(res if res <= n else 0)
```

---

## ✅ 문제 2. 구간 합 구하기 (누적합 활용)

**설명**  
1부터 N까지 수열이 주어지고, M개의 구간 [i, j]에 대한 합을 구하라.

**입력**
```
5 3
5 4 3 2 1
1 3
2 4
3 5
```

**출력**
```
12
9
6
```

**풀이**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
prefix = [0] * (n + 1)

for i in range(n):
    prefix[i + 1] = prefix[i] + a[i]

for _ in range(m):
    i, j = map(int, input().split())
    print(prefix[j] - prefix[i - 1])
```

---

## ✅ 문제 3. 연속 부분합 최대값 (고정 길이 K)

**설명**  
길이가 N인 수열에서 **길이 K짜리 연속 부분합의 최댓값**을 구하라.

**입력**
```
10 3
3 -2 -4 -9 0 3 7 13 8 -3
```

**출력**
```
28
```

**풀이**
```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
curr = sum(a[:k])
res = curr

for i in range(k, n):
    curr = curr + a[i] - a[i - k]
    res = max(res, curr)

print(res)
```

---

## ✅ 문제 4. 구간 최대합 (Kadane 알고리즘)

**설명**  
**어떤 길이든 상관없이** 연속된 부분 수열 중 **합이 최대인 경우의 값을 구하라**

**입력**
```
10
-1 3 -2 5 -3 2 2 -5 4 2
```

**출력**
```
9
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
curr = res = a[0]

for i in range(1, n):
    curr = max(a[i], curr + a[i])
    res = max(res, curr)

print(res)
```

---

## ✅ 문제 5. 구간 평균 최댓값

**설명**  
길이 N짜리 배열에서, 길이 **K짜리 연속 부분 배열**의 평균값이 최대가 되도록 하는 구간의 평균을 구하라.  
(정수로 출력)

**입력**
```
5 2
1 3 2 6 -1
```

**출력**
```
4
```

**풀이**
```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
window_sum = sum(a[:k])
max_sum = window_sum

for i in range(k, n):
    window_sum += a[i] - a[i - k]
    max_sum = max(max_sum, window_sum)

print(max_sum // k)
```

---

## ✅ 문제 6. 2차원 구간합 구하기

**설명**  
N×N 행렬이 주어질 때, (x1, y1) ~ (x2, y2)의 사각형 구간 합을 구하라.

**입력**
```
4 1
1 2 3 4
2 3 4 5
3 4 5 6
4 5 6 7
2 2 3 4
```

**출력**
```
24
```

**풀이**
```python
n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]
prefix = [[0] * (n + 1) for _ in range(n + 1)]

for i in range(n):
    for j in range(n):
        prefix[i + 1][j + 1] = a[i][j] + prefix[i][j + 1] + prefix[i + 1][j] - prefix[i][j]

for _ in range(m):
    x1, y1, x2, y2 = map(int, input().split())
    res = prefix[x2][y2] - prefix[x1 - 1][y2] - prefix[x2][y1 - 1] + prefix[x1 - 1][y1 - 1]
    print(res)
```

---

## ✅ 문제 7. 슬라이딩 윈도우 내 최댓값 구하기

**설명**  
길이 N 배열에서, **길이 K짜리 윈도우마다 최댓값**을 출력

**입력**
```
8 3
1 3 -1 -3 5 3 6 7
```

**출력**
```
3 3 5 5 6 7
```

**풀이**
```python
from collections import deque

n, k = map(int, input().split())
a = list(map(int, input().split()))
dq = deque()

for i in range(n):
    while dq and dq[-1][0] <= a[i]:
        dq.pop()
    dq.append((a[i], i))

    if dq[0][1] <= i - k:
        dq.popleft()

    if i >= k - 1:
        print(dq[0][0], end=' ')
```

---

## ✅ 문제 8. 투 포인터로 두 수의 합 찾기

**설명**  
정렬된 배열 A에서, 두 수를 더해 M이 되는 쌍의 개수를 구하라.

**입력**
```
9 13
1 2 3 4 5 6 7 8 9
```

**출력**
```
3
```

**풀이**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
left, right = 0, n - 1
count = 0

while left < right:
    s = a[left] + a[right]
    if s == m:
        count += 1
        left += 1
        right -= 1
    elif s < m:
        left += 1
    else:
        right -= 1

print(count)
```

---

## ✅ 문제 9. 누적합을 이용한 특정 합 조건 만족 개수 (누적합 + 딕셔너리)

**설명**  
누적합 중 합이 K가 되는 경우의 수를 구하라.

**입력**
```
5 3
1 1 1 1 1
```

**출력**
```
2
```

**풀이**
```python
from collections import defaultdict

n, k = map(int, input().split())
a = list(map(int, input().split()))
prefix = 0
count = 0
counter = defaultdict(int)
counter[0] = 1

for num in a:
    prefix += num
    count += counter[prefix - k]
    counter[prefix] += 1

print(count)
```

---

## ✅ 문제 10. K 이하의 최대 구간합 찾기 (투 포인터 + 누적합)

**설명**  
수열에서 구간합이 K를 넘지 않으면서, 가장 큰 구간합을 구하라.

**입력**
```
5 7
2 1 2 3 1
```

**출력**
```
7
```

**풀이**
```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
left = total = res = 0

for right in range(n):
    total += a[right]
    while total > k:
        total -= a[left]
        left += 1
    res = max(res, total)

print(res)
```

