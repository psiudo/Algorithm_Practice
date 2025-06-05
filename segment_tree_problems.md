# 🟧 Segment Tree 유형 문제집

누적합/최댓값/최솟값/구간 갱신 등을  
효율적으로 처리하기 위한 세그먼트 트리 구현/활용 문제 10선

---

## ✅ 문제 1. 구간 합 쿼리 (초기값 고정)

**설명**: 정수 배열이 주어질 때, 여러 개의 구간 [L, R]에 대해 합을 빠르게 구하라.  
(단, 값 갱신은 없음)

**입력**
```
5 3
1 3 5 7 9
1 3
2 5
1 5
```

**출력**
```
9
24
25
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] + tree[node*2+1]

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, node*2, start, mid, l, r) + query(tree, node*2+1, mid+1, end, l, r)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    l, r = map(int, input().split())
    print(query(tree, 1, 0, n-1, l-1, r-1))
```

---

## ✅ 문제 2. 구간 합 쿼리 + 단일값 변경

**설명**: 배열의 원소를 수정하는 명령과 구간 합을 구하는 명령이 섞여 있음.

**입력**
```
5 5
1 2 3 4 5
sum 1 3
update 2 10
sum 1 3
update 5 0
sum 1 5
```

**출력**
```
6
14
20
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] + tree[node*2+1]

def update(tree, node, start, end, idx, val):
    if start == end:
        tree[node] = val
    else:
        mid = (start + end) // 2
        if idx <= mid:
            update(tree, node*2, start, mid, idx, val)
        else:
            update(tree, node*2+1, mid+1, end, idx, val)
        tree[node] = tree[node*2] + tree[node*2+1]

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, node*2, start, mid, l, r) + query(tree, node*2+1, mid+1, end, l, r)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    parts = input().split()
    if parts[0] == 'sum':
        l, r = int(parts[1]), int(parts[2])
        print(query(tree, 1, 0, n-1, l-1, r-1))
    else:
        idx, val = int(parts[1]), int(parts[2])
        update(tree, 1, 0, n-1, idx-1, val)
```

---

## ✅ 문제 3. 구간 최소값 쿼리

**설명**: 여러 쿼리에 대해 [L, R] 구간의 최소값을 구하라.

**입력**
```
5 3
3 1 4 1 5
1 3
2 4
1 5
```

**출력**
```
1
1
1
```

**풀이**
```python
import math
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = min(tree[node*2], tree[node*2+1])

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return math.inf
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return min(query(tree, node*2, start, mid, l, r), query(tree, node*2+1, mid+1, end, l, r))

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    l, r = map(int, input().split())
    print(query(tree, 1, 0, n-1, l-1, r-1))
```

---

## ✅ 문제 4. 구간 최대값 쿼리 + 업데이트

**설명**: 여러 쿼리에 대해 최대값을 구하거나 특정 값을 갱신하라.

**입력**
```
5 4
2 1 3 5 4
max 1 3
update 2 10
max 1 3
max 3 5
```

**출력**
```
3
10
5
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = max(tree[node*2], tree[node*2+1])

def update(tree, node, start, end, idx, val):
    if start == end:
        tree[node] = val
    else:
        mid = (start + end) // 2
        if idx <= mid:
            update(tree, node*2, start, mid, idx, val)
        else:
            update(tree, node*2+1, mid+1, end, idx, val)
        tree[node] = max(tree[node*2], tree[node*2+1])

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return -float('inf')
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return max(query(tree, node*2, start, mid, l, r), query(tree, node*2+1, mid+1, end, l, r))

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    parts = input().split()
    if parts[0] == 'max':
        l, r = int(parts[1]), int(parts[2])
        print(query(tree, 1, 0, n-1, l-1, r-1))
    else:
        idx, val = int(parts[1]), int(parts[2])
        update(tree, 1, 0, n-1, idx-1, val)
```

---

## ✅ 문제 5. 홀수 갯수 구간 쿼리

**설명**: 배열에서 특정 구간 내에 홀수가 몇 개 있는지 출력하라.

**입력**
```
6 3
1 4 5 6 3 2
1 3
2 6
3 5
```

**출력**
```
2
2
2
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start] % 2
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] + tree[node*2+1]

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, node*2, start, mid, l, r) + query(tree, node*2+1, mid+1, end, l, r)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    l, r = map(int, input().split())
    print(query(tree, 1, 0, n-1, l-1, r-1))
```

---

## ✅ 문제 6. 짝수 갯수 구간 쿼리 + 단일 변경

**설명**: 쿼리는 다음 두 가지가 있다:  
1. 특정 위치의 값을 변경  
2. 구간 [L, R]에서 짝수의 개수를 출력

**입력**
```
6 4
2 3 4 5 6 7
count 1 3
update 2 10
count 1 3
count 1 6
```

**출력**
```
2
3
4
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = 1 if arr[start] % 2 == 0 else 0
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] + tree[node*2+1]

def update(tree, node, start, end, idx, val):
    if start == end:
        tree[node] = 1 if val % 2 == 0 else 0
    else:
        mid = (start + end) // 2
        if idx <= mid:
            update(tree, node*2, start, mid, idx, val)
        else:
            update(tree, node*2+1, mid+1, end, idx, val)
        tree[node] = tree[node*2] + tree[node*2+1]

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, node*2, start, mid, l, r) + query(tree, node*2+1, mid+1, end, l, r)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    parts = input().split()
    if parts[0] == 'count':
        l, r = int(parts[1]), int(parts[2])
        print(query(tree, 1, 0, n-1, l-1, r-1))
    else:
        idx, val = int(parts[1]), int(parts[2])
        update(tree, 1, 0, n-1, idx-1, val)
```

---

## ✅ 문제 7. 구간 곱 쿼리 (mod 1_000_000_007)

**설명**: 여러 쿼리에 대해 [L, R]의 곱을 구하되, 결과는 1_000_000_007로 나눈다.

**입력**
```
4 2
1 2 3 4
1 3
2 4
```

**출력**
```
6
24
```

**풀이**
```python
MOD = 10**9+7

def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = (tree[node*2] * tree[node*2+1]) % MOD

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 1
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return (query(tree, node*2, start, mid, l, r) * query(tree, node*2+1, mid+1, end, l, r)) % MOD

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [1] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    l, r = map(int, input().split())
    print(query(tree, 1, 0, n-1, l-1, r-1))
```

---

## ✅ 문제 8. 구간 XOR 쿼리

**설명**: 정수 배열의 [L, R]에 대해 XOR 결과를 출력하라.

**입력**
```
5 2
1 2 3 4 5
1 3
2 5
```

**출력**
```
0
4
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] ^ tree[node*2+1]

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, node*2, start, mid, l, r) ^ query(tree, node*2+1, mid+1, end, l, r)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    l, r = map(int, input().split())
    print(query(tree, 1, 0, n-1, l-1, r-1))
```

---

## ✅ 문제 9. Lazy Propagation: 구간 값 증가

**설명**: 다음 연산을 처리하라.  
1. 구간 [L, R] 모든 값에 +K  
2. 구간 [L, R]의 합 출력

**입력**
```
5 3
1 2 3 4 5
add 1 3 10
sum 1 5
sum 2 4
```

**출력**
```
45
39
```

**풀이**
```python
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end)//2
        build(arr, tree, node*2, start, mid)
        build(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] + tree[node*2+1]

def update_lazy(tree, lazy, node, start, end):
    if lazy[node] != 0:
        tree[node] += (end - start + 1) * lazy[node]
        if start != end:
            lazy[node*2] += lazy[node]
            lazy[node*2+1] += lazy[node]
        lazy[node] = 0

def range_add(tree, lazy, node, start, end, l, r, val):
    update_lazy(tree, lazy, node, start, end)
    if r < start or end < l:
        return
    if l <= start and end <= r:
        tree[node] += (end - start + 1) * val
        if start != end:
            lazy[node*2] += val
            lazy[node*2+1] += val
        return
    mid = (start + end) // 2
    range_add(tree, lazy, node*2, start, mid, l, r, val)
    range_add(tree, lazy, node*2+1, mid+1, end, l, r, val)
    tree[node] = tree[node*2] + tree[node*2+1]

def query(tree, lazy, node, start, end, l, r):
    update_lazy(tree, lazy, node, start, end)
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, lazy, node*2, start, mid, l, r) + query(tree, lazy, node*2+1, mid+1, end, l, r)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
tree = [0] * (4 * n)
lazy = [0] * (4 * n)
build(arr, tree, 1, 0, n-1)

for _ in range(q):
    parts = input().split()
    if parts[0] == 'add':
        l, r, v = map(int, parts[1:])
        range_add(tree, lazy, 1, 0, n-1, l-1, r-1, v)
    else:
        l, r = map(int, parts[1:])
        print(query(tree, lazy, 1, 0, n-1, l-1, r-1))
```

---

## ✅ 문제 10. 세그먼트 트리 직접 구현 (모든 연산 지원)

**설명**: 다음 연산을 모두 지원하는 클래스를 직접 구현하라.  
- `build`, `point_update`, `range_sum`, `range_min`, `range_max` 등

**입력**
```
5 2
4 2 7 1 3
sum 2 4
min 1 5
```

**출력**
```
10
1
```

**풀이**
```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum_tree = [0] * (4 * self.n)
        self.min_tree = [0] * (4 * self.n)
        self.max_tree = [0] * (4 * self.n)
        self.build(arr, 1, 0, self.n-1)

    def build(self, arr, node, start, end):
        if start == end:
            self.sum_tree[node] = self.min_tree[node] = self.max_tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            self.build(arr, node*2, start, mid)
            self.build(arr, node*2+1, mid+1, end)
            self.sum_tree[node] = self.sum_tree[node*2] + self.sum_tree[node*2+1]
            self.min_tree[node] = min(self.min_tree[node*2], self.min_tree[node*2+1])
            self.max_tree[node] = max(self.max_tree[node*2], self.max_tree[node*2+1])

    def query_sum(self, node, start, end, l, r):
        if r < start or end < l:
            return 0
        if l <= start and end <= r:
            return self.sum_tree[node]
        mid = (start + end) // 2
        return self.query_sum(node*2, start, mid, l, r) + self.query_sum(node*2+1, mid+1, end, l, r)

    def query_min(self, node, start, end, l, r):
        if r < start or end < l:
            return float('inf')
        if l <= start and end <= r:
            return self.min_tree[node]
        mid = (start + end) // 2
        return min(self.query_min(node*2, start, mid, l, r), self.query_min(node*2+1, mid+1, end, l, r))

n, q = map(int, input().split())
arr = list(map(int, input().split()))
st = SegmentTree(arr)
for _ in range(q):
    parts = input().split()
    if parts[0] == 'sum':
        l, r = int(parts[1]), int(parts[2])
        print(st.query_sum(1, 0, n-1, l-1, r-1))
    elif parts[0] == 'min':
        l, r = int(parts[1]), int(parts[2])
        print(st.query_min(1, 0, n-1, l-1, r-1))
```


