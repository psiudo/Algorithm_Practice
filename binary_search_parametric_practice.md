# 🟦 Binary Search / Parametric Search Practice

**이분 탐색**과 **조건 판단에 따라 값의 범위를 좁히는**  
**매개변수 탐색(Parameter Search)** 문제 10선

---

## ✅ 문제 1. 정렬된 배열에서 특정 값 개수 세기

**입력**
```
7 3
1 2 2 2 3 4 5
```

**출력**
```
3
```

**풀이**
```python
import bisect

n, x = map(int, input().split())
a = list(map(int, input().split()))
print(bisect.bisect_right(a, x) - bisect.bisect_left(a, x))
```

---

## ✅ 문제 2. 정렬된 배열에서 값 찾기 (이분 탐색 직접 구현)

**입력**
```
5 3
1 2 3 4 5
```

**출력**
```
2
```

(값 `3`의 인덱스, 없으면 -1)

**풀이**
```python
def binary_search(arr, x):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == x:
            return mid
        elif arr[mid] < x:
            left = mid + 1
        else:
            right = mid - 1
    return -1

n, x = map(int, input().split())
a = list(map(int, input().split()))
print(binary_search(a, x))
```

---

## ✅ 문제 3. 나무 자르기 (매개변수 탐색)

**설명**: 최소한 `M`미터의 나무를 가져가야 할 때, 절단기의 최대 높이 구하기

**입력**
```
4 7
20 15 10 17
```

**출력**
```
15
```

**풀이**
```python
n, m = map(int, input().split())
trees = list(map(int, input().split()))
left, right = 0, max(trees)
res = 0

while left <= right:
    mid = (left + right) // 2
    total = sum((t - mid for t in trees if t > mid))
    if total >= m:
        res = mid
        left = mid + 1
    else:
        right = mid - 1

print(res)
```

---

## ✅ 문제 4. 랜선 자르기 (최대 길이)

**입력**
```
4 11
802 743 457 539
```

**출력**
```
200
```

**풀이**
```python
k, n = map(int, input().split())
a = [int(input()) for _ in range(k)]

left, right = 1, max(a)
res = 0

while left <= right:
    mid = (left + right) // 2
    count = sum(x // mid for x in a)
    if count >= n:
        res = mid
        left = mid + 1
    else:
        right = mid - 1

print(res)
```

---

## ✅ 문제 5. 공유기 설치 (최소 거리의 최대값)

**입력**
```
5 3
1 2 8 4 9
```

**출력**
```
3
```

**풀이**
```python
n, c = map(int, input().split())
a = sorted(int(input()) for _ in range(n))

def is_possible(dist):
    count = 1
    last = a[0]
    for i in range(1, n):
        if a[i] - last >= dist:
            count += 1
            last = a[i]
    return count >= c

left, right = 1, a[-1] - a[0]
res = 0

while left <= right:
    mid = (left + right) // 2
    if is_possible(mid):
        res = mid
        left = mid + 1
    else:
        right = mid - 1

print(res)
```

---

## ✅ 문제 6. 떡볶이 떡 만들기 (누적합 조건)

**설명**: 절단기 높이를 정해놓고, 그보다 높은 부분만 잘라 가져감. 최소한 `M`만큼 얻도록 할 때 절단기 높이의 최댓값.

**입력**
```
4 6
19 15 10 17
```

**출력**
```
15
```

**풀이**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))

left, right = 0, max(a)
res = 0

while left <= right:
    mid = (left + right) // 2
    total = sum(x - mid for x in a if x > mid)
    if total >= m:
        res = mid
        left = mid + 1
    else:
        right = mid - 1

print(res)
```

---

## ✅ 문제 7. 최소 작업 시간 (작업량이 주어졌을 때 최소 시간 구하기)

**입력**
```
3 6
7 10 5
```

**출력**
```
14
```

**풀이**
```python
n, t = map(int, input().split())
machines = list(map(int, input().split()))

left, right = 1, max(machines) * t
res = right

while left <= right:
    mid = (left + right) // 2
    total = sum(mid // m for m in machines)
    if total >= t:
        res = mid
        right = mid - 1
    else:
        left = mid + 1

print(res)
```

---

## ✅ 문제 8. 블루레이 만들기

**설명**: 강의들을 순서대로 나눠 담되, 한 블루레이에는 연속된 강의만 담을 수 있음. 총 `M`개의 블루레이에 담기 위한 최소 크기 구하라.

**입력**
```
9 3
1 2 3 4 5 6 7 8 9
```

**출력**
```
17
```

**풀이**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))

left, right = max(a), sum(a)
res = right

while left <= right:
    mid = (left + right) // 2
    cnt = 1
    total = 0
    for x in a:
        if total + x > mid:
            cnt += 1
            total = 0
        total += x
    if cnt <= m:
        res = mid
        right = mid - 1
    else:
        left = mid + 1

print(res)
```

---

## ✅ 문제 9. 입국 심사 최소 시간

**입력**
```
6 7
7 10
```

**출력**
```
28
```

**풀이**
```python
n, m = map(int, input().split())
times = list(map(int, input().split()))

left, right = 1, max(times) * m
res = right

while left <= right:
    mid = (left + right) // 2
    total = sum(mid // t for t in times)
    if total >= m:
        res = mid
        right = mid - 1
    else:
        left = mid + 1

print(res)
```

---

## ✅ 문제 10. 특정 조건을 만족하는 최소값 찾기

**설명**: 배열에서 특정 조건을 만족하는 최소값 또는 최대값을 찾는 전형적인 결정 문제 구조

**입력**
```
6
1 3 2 5 4 6
```

조건: 연속된 구간에서 최대값이 4 이상이 되는 최소 구간 길이

**출력**
```
2
```

**풀이**
```python
a = list(map(int, input().split()))
n = len(a)

def is_possible(k):
    for i in range(n - k + 1):
        if max(a[i:i+k]) >= 4:
            return True
    return False

left, right = 1, n
res = n + 1

while left <= right:
    mid = (left + right) // 2
    if is_possible(mid):
        res = mid
        right = mid - 1
    else:
        left = mid + 1

print(res if res <= n else 0)
```

---

