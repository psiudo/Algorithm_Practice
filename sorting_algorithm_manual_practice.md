# 🟨 Sorting Algorithm Manual Practice

**정렬 알고리즘 10선**  
모두 **직접 구현**을 통해 정렬 방식의 차이와 작동 원리를 정확히 익히기 위한 연습

---

## ✅ 문제 1. 선택 정렬 구현

**입력**
```
5
5 4 3 2 1
```

**출력**
```
1 2 3 4 5
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))

for i in range(n):
    min_idx = i
    for j in range(i + 1, n):
        if a[j] < a[min_idx]:
            min_idx = j
    a[i], a[min_idx] = a[min_idx], a[i]

print(*a)
```

---

## ✅ 문제 2. 버블 정렬 구현

**입력**
```
5
5 1 4 2 8
```

**출력**
```
1 2 4 5 8
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))

for i in range(n):
    for j in range(n - i - 1):
        if a[j] > a[j + 1]:
            a[j], a[j + 1] = a[j + 1], a[j]

print(*a)
```

---

## ✅ 문제 3. 삽입 정렬 구현

**입력**
```
6
5 2 4 6 1 3
```

**출력**
```
1 2 3 4 5 6
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))

for i in range(1, n):
    key = a[i]
    j = i - 1
    while j >= 0 and a[j] > key:
        a[j + 1] = a[j]
        j -= 1
    a[j + 1] = key

print(*a)
```

---

## ✅ 문제 4. 퀵 정렬 (호어 분할 방식)

**입력**
```
5
5 3 8 4 2
```

**출력**
```
2 3 4 5 8
```

**풀이**
```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    left = [x for x in arr[1:] if x <= pivot]
    right = [x for x in arr[1:] if x > pivot]
    return quicksort(left) + [pivot] + quicksort(right)

n = int(input())
a = list(map(int, input().split()))
print(*quicksort(a))
```

---

## ✅ 문제 5. 병합 정렬 구현

**입력**
```
5
38 27 43 3 9
```

**출력**
```
3 9 27 38 43
```

**풀이**
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    L = merge_sort(arr[:mid])
    R = merge_sort(arr[mid:])
    res = []
    i = j = 0
    while i < len(L) and j < len(R):
        if L[i] < R[j]:
            res.append(L[i])
            i += 1
        else:
            res.append(R[j])
            j += 1
    res += L[i:]
    res += R[j:]
    return res

n = int(input())
a = list(map(int, input().split()))
print(*merge_sort(a))
```

---

## ✅ 문제 6. 셸 정렬 (Shell Sort) 구현

**입력**
```
6
23 12 1 8 34 54
```

**출력**
```
1 8 12 23 34 54
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
gap = n // 2

while gap > 0:
    for i in range(gap, n):
        temp = a[i]
        j = i
        while j >= gap and a[j - gap] > temp:
            a[j] = a[j - gap]
            j -= gap
        a[j] = temp
    gap //= 2

print(*a)
```

---

## ✅ 문제 7. 힙 정렬 (Heap Sort) 구현

**입력**
```
5
4 10 3 5 1
```

**출력**
```
1 3 4 5 10
```

**풀이**
```python
import heapq

n = int(input())
a = list(map(int, input().split()))
heap = []

for x in a:
    heapq.heappush(heap, x)

res = [heapq.heappop(heap) for _ in range(n)]
print(*res)
```

---

## ✅ 문제 8. 카운팅 정렬 (Counting Sort) 구현

**입력**
```
6
4 2 2 8 3 3
```

**출력**
```
2 2 3 3 4 8
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
max_val = max(a)
count = [0] * (max_val + 1)

for x in a:
    count[x] += 1

res = []
for i in range(len(count)):
    res.extend([i] * count[i])

print(*res)
```

---

## ✅ 문제 9. 기수 정렬 (Radix Sort) 구현 (비음수 정수만)

**입력**
```
6
170 45 75 90 802 24
```

**출력**
```
24 45 75 90 170 802
```

**풀이**
```python
def counting_sort_exp(a, exp):
    n = len(a)
    output = [0] * n
    count = [0] * 10

    for i in range(n):
        index = a[i] // exp % 10
        count[index] += 1

    for i in range(1, 10):
        count[i] += count[i - 1]

    for i in reversed(range(n)):
        index = a[i] // exp % 10
        output[count[index] - 1] = a[i]
        count[index] -= 1

    for i in range(n):
        a[i] = output[i]

n = int(input())
a = list(map(int, input().split()))
max_num = max(a)
exp = 1

while max_num // exp > 0:
    counting_sort_exp(a, exp)
    exp *= 10

print(*a)
```

---

## ✅ 문제 10. 파이썬 내장 정렬과 직접 구현 속도 비교

**입력**
```
5
4 1 3 9 7
```

**출력**
```
1 3 4 7 9
```

**설명**: 내장 `sorted()`와 비교하여 결과 일치 확인만 수행

**풀이**
```python
a = list(map(int, input().split()))
print(*sorted(a))  # 참조
```

