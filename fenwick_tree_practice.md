# 🟦 Fenwick Tree (BIT) 유형 문제집

Binary Indexed Tree를 활용한 누적합, 구간 갱신, 차분 배열 처리 연습 문제 10선  
(시간복잡도: `O(log N)` 수준의 쿼리와 갱신)

---

## ✅ 문제 1. 구간 누적합 쿼리 (기초)

**설명**: 수열 A가 주어지고, 여러 번 [1, R]의 누적합을 구하라. (0-based 아님)

**입력**
```
5 3
1 2 3 4 5
1
3
5
```

**출력**
```
1
6
15
```

**풀이**
```python
def update(bit, i, x):
    while i <= len(bit)-1:
        bit[i] += x
        i += i & -i

def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+1)

for idx, val in enumerate(arr):
    update(bit, idx+1, val)

for _ in range(q):
    r = int(input())
    print(query(bit, r))
```

---

## ✅ 문제 2. 구간 합 쿼리 [L, R]

**설명**: 누적합 쿼리 두 번으로 [L, R] 구간 합을 구하라.

**입력**
```
5 2
1 2 3 4 5
1 3
2 5
```

**출력**
```
6
14
```

**풀이**
```python
def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

def update(bit, i, x):
    while i <= len(bit)-1:
        bit[i] += x
        i += i & -i

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+1)

for i in range(n):
    update(bit, i+1, arr[i])

for _ in range(q):
    l, r = map(int, input().split())
    print(query(bit, r) - query(bit, l-1))
```

---

## ✅ 문제 3. 값 변경 + 구간 합 쿼리

**설명**: `update i x`로 i번째 값을 x로 바꾸고, `sum l r`로 합을 구하라.

**입력**
```
5 4
1 2 3 4 5
sum 1 3
update 2 10
sum 1 3
sum 1 5
```

**출력**
```
6
14
28
```

**풀이**
```python
def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

def update(bit, i, diff):
    while i < len(bit):
        bit[i] += diff
        i += i & -i

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+1)

for i in range(n):
    update(bit, i+1, arr[i])

for _ in range(q):
    parts = input().split()
    if parts[0] == 'sum':
        l, r = map(int, parts[1:])
        print(query(bit, r) - query(bit, l-1))
    else:
        idx, new_val = int(parts[1]), int(parts[2])
        diff = new_val - arr[idx-1]
        arr[idx-1] = new_val
        update(bit, idx, diff)
```

---

## ✅ 문제 4. 누적 XOR 쿼리 (BIT 활용)

**설명**: BIT로 XOR 누적합을 처리할 수 있는지 시험하는 연습  
(트릭 기반 구현)

**입력**
```
4 2
1 2 3 4
1 3
2 4
```

**출력**
```
0
5
```

**풀이**
```python
def query(bit, i):
    res = 0
    while i > 0:
        res ^= bit[i]
        i -= i & -i
    return res

def update(bit, i, val):
    while i < len(bit):
        bit[i] ^= val
        i += i & -i

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+1)

for i in range(n):
    update(bit, i+1, arr[i])

for _ in range(q):
    l, r = map(int, input().split())
    print(query(bit, r) ^ query(bit, l-1))
```

---

## ✅ 문제 5. Point Add + Prefix Sum 응용

**설명**: 다음 쿼리를 처리하라  
1. `add i x` : i번째 수에 x를 더한다  
2. `sum r` : [1, r]까지의 합을 출력한다

**입력**
```
5 4
1 2 3 4 5
sum 3
add 2 5
sum 3
sum 5
```

**출력**
```
6
11
26
```

**풀이**
```python
def update(bit, i, val):
    while i < len(bit):
        bit[i] += val
        i += i & -i

def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+1)

for i in range(n):
    update(bit, i+1, arr[i])

for _ in range(q):
    parts = input().split()
    if parts[0] == 'add':
        idx, x = int(parts[1]), int(parts[2])
        update(bit, idx, x)
    else:
        r = int(parts[1])
        print(query(bit, r))
```

---

## ✅ 문제 6. 구간 전체에 값 더하기 + 단일 값 조회

**설명**: BIT를 2개 사용해 range add + point query를 구현  
쿼리 1: `add l r x` → 구간 [l, r]에 x 더하기  
쿼리 2: `get i` → i번째 수 출력

**입력**
```
5 4
0 0 0 0 0
add 1 3 10
get 2
add 2 5 5
get 4
```

**출력**
```
10
5
```

**풀이**
```python
def update(bit, i, x):
    while i < len(bit):
        bit[i] += x
        i += i & -i

def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+2)  # +2 to avoid overflow on r+1

for _ in range(q):
    parts = input().split()
    if parts[0] == 'add':
        l, r, x = map(int, parts[1:])
        update(bit, l, x)
        update(bit, r+1, -x)
    else:
        i = int(parts[1])
        print(arr[i-1] + query(bit, i))
```

---

## ✅ 문제 7. 차분 배열과 BIT로 구간 일괄 갱신

**설명**: 구간 [l, r]에 x 더하고, 전체 누적합 쿼리 처리

**입력**
```
5 3
1 2 3 4 5
add 1 3 2
sum 1 5
sum 2 4
```

**출력**
```
19
13
```

**풀이**
```python
def update(bit, i, x):
    while i < len(bit):
        bit[i] += x
        i += i & -i

def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = [0] * (n+1)

for i in range(n):
    update(bit, i+1, arr[i])

for _ in range(q):
    parts = input().split()
    if parts[0] == 'add':
        l, r, x = map(int, parts[1:])
        for i in range(l, r+1):
            update(bit, i, x)
    else:
        l, r = map(int, parts[1:])
        print(query(bit, r) - query(bit, l-1))
```

---

## ✅ 문제 8. 2차원 BIT: 구간합

**설명**: N×N 격자에 대해 (x1,y1)~(x2,y2) 구간 합 쿼리 처리

**입력**
```
3 2
1 2 3
4 5 6
7 8 9
1 1 2 2
2 2 3 3
```

**출력**
```
12
28
```

**풀이**
```python
def update(bit, x, y, val):
    xi = x
    while xi < len(bit):
        yi = y
        while yi < len(bit):
            bit[xi][yi] += val
            yi += yi & -yi
        xi += xi & -xi

def query(bit, x, y):
    res = 0
    xi = x
    while xi > 0:
        yi = y
        while yi > 0:
            res += bit[xi][yi]
            yi -= yi & -yi
        xi -= xi & -xi
    return res

n, q = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]
bit = [[0]*(n+2) for _ in range(n+2)]

for i in range(n):
    for j in range(n):
        update(bit, i+1, j+1, grid[i][j])

for _ in range(q):
    x1, y1, x2, y2 = map(int, input().split())
    def region_sum(x, y):
        return query(bit, x, y)
    ans = region_sum(x2, y2) - region_sum(x1-1, y2) - region_sum(x2, y1-1) + region_sum(x1-1, y1-1)
    print(ans)
```

---

## ✅ 문제 9. 좌표 압축 + 누적합 BIT

**설명**: 큰 범위의 정수값들을 좌표 압축 후 BIT로 처리  
→ `N ≤ 10^5`, 값 범위 `1 ≤ A_i ≤ 10^9`일 때 빠르게 누적합 쿼리 처리

**입력**
```
5 2
10 100 20 30 90
30
90
```

**출력**
```
2
4
```

**풀이**
```python
def update(bit, i, x):
    while i < len(bit):
        bit[i] += x
        i += i & -i

def query(bit, i):
    res = 0
    while i > 0:
        res += bit[i]
        i -= i & -i
    return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))
sorted_vals = sorted(set(arr))
compress = {v: i+1 for i, v in enumerate(sorted_vals)}
bit = [0] * (len(compress) + 2)

for a in arr:
    update(bit, compress[a], 1)

for _ in range(q):
    x = int(input())
    # 누적합: compress[x] 이하의 개수
    keys = [k for k in sorted_vals if k <= x]
    if not keys:
        print(0)
    else:
        idx = compress[keys[-1]]
        print(query(bit, idx))
```

---

## ✅ 문제 10. Prefix sum 상수 배포용 BIT 클래스

**설명**: 전체 연산을 캡슐화한 BIT 클래스 작성  
→ sum, add, set 등 연산을 포함해 실전 구현 연습

**입력**
```
5 2
1 2 3 4 5
sum 3
add 2 10
sum 3
```

**출력**
```
6
16
```

**풀이**
```python
class BIT:
    def __init__(self, size):
        self.n = size
        self.tree = [0] * (self.n + 1)

    def update(self, i, x):
        while i <= self.n:
            self.tree[i] += x
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            res += self.tree[i]
            i -= i & -i
        return res

    def range_sum(self, l, r):
        return self.query(r) - self.query(l-1)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
bit = BIT(n)
for i in range(n):
    bit.update(i+1, arr[i])

for _ in range(q):
    parts = input().split()
    if parts[0] == 'sum':
        print(bit.query(int(parts[1])))
    else:
        idx, x = int(parts[1]), int(parts[2])
        bit.update(idx, x)
```

