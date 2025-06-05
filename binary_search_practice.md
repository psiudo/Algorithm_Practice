# 🟥 이진 탐색(binary search) 유형 문제집

Python의 `bisect` 모듈, 직접 구현한 이분탐색 함수로  
**탐색 위치, 구간 길이, 조건 만족값 최적화** 등을 훈련하는 문제 10선

---

## ✅ 문제 1. 값 존재 여부

**설명**: 정렬된 리스트 A와 수 x가 주어진다. x가 존재하면 YES, 없으면 NO

**입력**
```
5
1 3 5 7 9
3
```

**출력**
```
YES
```

**풀이**
```python
import bisect

input()
a = list(map(int, input().split()))
x = int(input())
idx = bisect.bisect_left(a, x)
print("YES" if idx < len(a) and a[idx] == x else "NO")
```

---

## ✅ 문제 2. 삽입 위치 출력

**설명**: 정렬된 배열에서 x가 들어갈 수 있는 가장 왼쪽 인덱스를 출력

**입력**
```
6
1 3 3 5 7 9
3
```

**출력**
```
1
```

**풀이**
```python
import bisect

input()
a = list(map(int, input().split()))
x = int(input())
print(bisect.bisect_left(a, x))
```

---

## ✅ 문제 3. 값 개수 세기

**설명**: 정렬된 배열에서 값 x가 몇 번 등장하는지 출력

**입력**
```
7
1 2 2 2 3 4 5
2
```

**출력**
```
3
```

**풀이**
```python
import bisect

input()
a = list(map(int, input().split()))
x = int(input())
l = bisect.bisect_left(a, x)
r = bisect.bisect_right(a, x)
print(r - l)
```

---

## ✅ 문제 4. 특정 범위 원소 개수

**설명**: 배열에서 x 이상 y 이하인 수의 개수를 출력

**입력**
```
8
1 2 3 4 5 6 7 8
3 6
```

**출력**
```
4
```

**풀이**
```python
import bisect

input()
a = list(map(int, input().split()))
x, y = map(int, input().split())
print(bisect.bisect_right(a, y) - bisect.bisect_left(a, x))
```

---

## ✅ 문제 5. 가장 가까운 수

**설명**: 정렬된 배열에서 x와 가장 가까운 수를 출력. 두 개면 작은 수 출력

**입력**
```
6
1 3 5 7 9 11
8
```

**출력**
```
7
```

**풀이**
```python
import bisect

input()
a = list(map(int, input().split()))
x = int(input())
i = bisect.bisect_left(a, x)

candidates = []
if i < len(a):
    candidates.append(a[i])
if i > 0:
    candidates.append(a[i - 1])

print(min(candidates, key=lambda y: (abs(y - x), y)))
```

---

## ✅ 문제 6. 랜선 자르기

**설명**: 랜선 길이 배열과 필요한 개수 K가 주어진다. 만들 수 있는 최대 길이를 출력

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
n, k = map(int, input().split())
arr = list(map(int, input().split()))

def count(length):
    return sum(x // length for x in arr)

left, right = 1, max(arr)
res = 0
while left <= right:
    mid = (left + right) // 2
    if count(mid) >= k:
        res = mid
        left = mid + 1
    else:
        right = mid - 1
print(res)
```

---

## ✅ 문제 7. 용액 두 개

**설명**: n개의 수 중 두 개를 골라 합이 0에 가장 가까운 쌍을 출력

**입력**
```
5
-2 4 -99 -1 98
```

**출력**
```
-99 98
```

**풀이**
```python
n = int(input())
arr = sorted(map(int, input().split()))

left, right = 0, n - 1
best = (arr[left], arr[right])
min_abs = abs(sum(best))

while left < right:
    total = arr[left] + arr[right]
    if abs(total) < min_abs:
        min_abs = abs(total)
        best = (arr[left], arr[right])
    if total < 0:
        left += 1
    else:
        right -= 1
print(*best)
```

---

## ✅ 문제 8. 나무 자르기

**설명**: 나무 높이 배열과 요구 길이 m이 주어진다. 최소한 m을 얻기 위한 절단기 최대 높이를 출력

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
arr = list(map(int, input().split()))

def total(h):
    return sum((x - h) for x in arr if x > h)

left, right = 0, max(arr)
res = 0
while left <= right:
    mid = (left + right) // 2
    if total(mid) >= m:
        res = mid
        left = mid + 1
    else:
        right = mid - 1
print(res)
```

---

## ✅ 문제 9. 입국심사

**설명**: 심사관마다 걸리는 시간이 주어질 때, N명을 심사하는 최소 시간을 구하라.

**입력**
```
2 6
7 10
```

**출력**
```
28
```

**풀이**
```python
n, m = map(int, input().split())
time = list(map(int, input().split()))

left, right = 1, max(time) * m
res = 0
while left <= right:
    mid = (left + right) // 2
    cnt = sum(mid // t for t in time)
    if cnt >= m:
        res = mid
        right = mid - 1
    else:
        left = mid + 1
print(res)
```

---

## ✅ 문제 10. k번째 수 찾기 (배열 없이)

**설명**: n x n 곱셈 표에서 k번째 작은 수를 출력하라

**입력**
```
3 5
```

**출력**
```
3
```

**풀이**
```python
n, k = map(int, input().split())

left, right = 1, n * n
res = 0
while left <= right:
    mid = (left + right) // 2
    cnt = sum(min(mid // i, n) for i in range(1, n + 1))
    if cnt >= k:
        res = mid
        right = mid - 1
    else:
        left = mid + 1
print(res)
```

