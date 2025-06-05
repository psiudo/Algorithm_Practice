# 🟦 유니온 파인드(union-find) 유형 문제집

**Disjoint Set (분리 집합)** 자료구조의 핵심인  
`find`, `union`, `path compression`을 반복 연습하는 문제 10선.

---

## ✅ 문제 1. 집합 연결 여부

**설명**: a와 b가 같은 집합에 속해있는지 판별

**입력**
```
5 3
union 1 2
union 2 3
check 1 3
```

**출력**
```
YES
```

**풀이**
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    x = find(x)
    y = find(y)
    if x != y:
        parent[y] = x

n, q = map(int, input().split())
parent = list(range(n + 1))

for _ in range(q):
    cmd, a, b = input().split()
    a, b = int(a), int(b)
    if cmd == "union":
        union(a, b)
    else:
        print("YES" if find(a) == find(b) else "NO")
```

---

## ✅ 문제 2. 연결 요소 개수

**설명**: union 연산을 거친 후, 전체 집합의 개수를 출력

**입력**
```
5 3
1 2
3 4
4 5
```

**출력**
```
2
```

**풀이**
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    parent[find(y)] = find(x)

n, m = map(int, input().split())
parent = list(range(n + 1))
for _ in range(m):
    a, b = map(int, input().split())
    union(a, b)

roots = set(find(x) for x in range(1, n + 1))
print(len(roots))
```

---

## ✅ 문제 3. 사이클 판별

**설명**: 간선을 연결할 때 사이클이 생기면 YES, 아니면 NO

**입력**
```
4 4
1 2
2 3
3 4
4 1
```

**출력**
```
YES
```

**풀이**
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    x = find(x)
    y = find(y)
    if x == y:
        return False
    parent[y] = x
    return True

n, m = map(int, input().split())
parent = list(range(n + 1))
for _ in range(m):
    a, b = map(int, input().split())
    if not union(a, b):
        print("YES")
        break
else:
    print("NO")
```

---

## ✅ 문제 4. 그룹 번호 출력

**설명**: 각 원소가 어떤 그룹에 속하는지 번호를 출력

**입력**
```
6 3
1 2
3 4
5 6
```

**출력**
```
1 1 3 3 5 5
```

**풀이**
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    parent[find(y)] = find(x)

n, m = map(int, input().split())
parent = list(range(n + 1))
for _ in range(m):
    a, b = map(int, input().split())
    union(a, b)

res = [find(i) for i in range(1, n + 1)]
print(*res)
```

---

## ✅ 문제 5. 유니온 순서 유지

**설명**: union 순서를 유지하여, 루트 번호가 항상 작도록 관리

**입력**
```
4 2
2 1
3 4
```

**출력**
```
1 1 3 3
```

**풀이**
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    x = find(x)
    y = find(y)
    if x < y:
        parent[y] = x
    else:
        parent[x] = y

n, m = map(int, input().split())
parent = list(range(n + 1))
for _ in range(m):
    a, b = map(int, input().split())
    union(a, b)

print(*[find(i) for i in range(1, n + 1)])
```

---

## ✅ 문제 6. 친구 네트워크 크기

**설명**: 이름 쌍으로 친구가 된다. 친구가 연결되었을 때 전체 네트워크 크기를 출력

**입력**
```
3
Fred Barney
Barney Betty
Betty Wilma
```

**출력**
```
2
3
4
```

**풀이**
```python
def find(x):
    if x != parent[x]:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    x, y = find(x), find(y)
    if x != y:
        parent[y] = x
        size[x] += size[y]
    return size[x]

f = int(input())
parent = {}
size = {}

for _ in range(f):
    a, b = input().split()
    for name in [a, b]:
        if name not in parent:
            parent[name] = name
            size[name] = 1
    print(union(a, b))
```

---

## ✅ 문제 7. 도시 연결

**설명**: 도시 간 도로 연결이 가능한지 쿼리를 통해 판별

**입력**
```
4 2
1 2
3 4
1 2
1 3
```

**출력**
```
YES
NO
```

**풀이**
```python
def find(x):
    if x != parent[x]:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    parent[find(y)] = find(x)

n, m = map(int, input().split())
parent = list(range(n + 1))
for _ in range(m):
    a, b = map(int, input().split())
    union(a, b)

q = int(input())
for _ in range(q):
    a, b = map(int, input().split())
    print("YES" if find(a) == find(b) else "NO")
```

---

## ✅ 문제 8. MST용 유니온

**설명**: 간선 정보 (비용, a, b)가 주어진다. MST 비용 출력

**입력**
```
4 5
1 1 2
3 2 3
2 1 3
4 3 4
5 2 4
```

**출력**
```
7
```

**풀이**
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    x = find(x)
    y = find(y)
    if x == y:
        return False
    parent[y] = x
    return True

n, m = map(int, input().split())
edges = [tuple(map(int, input().split())) for _ in range(m)]
edges.sort()
parent = list(range(n + 1))
res = 0
for cost, a, b in edges:
    if union(a, b):
        res += cost
print(res)
```

---

## ✅ 문제 9. 그룹 쿼리 처리

**설명**: union 또는 find 쿼리를 통해 그룹화 처리. 최종 그룹 수 출력

**입력**
```
5 3
union 1 2
union 3 4
union 2 3
```

**출력**
```
2
```

**풀이**
```python
def find(x):
    if x != parent[x]:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    parent[find(y)] = find(x)

n, q = map(int, input().split())
parent = list(range(n + 1))
for _ in range(q):
    cmd, a, b = input().split()
    union(int(a), int(b))

roots = set(find(i) for i in range(1, n + 1))
print(len(roots))
```

---

## ✅ 문제 10. 경로 압축 비교

**설명**: 유니온 파인드에서 경로 압축 전후로 find 호출 횟수 차이를 출력

**입력**
```
5 4
1 2
2 3
3 4
4 5
```

**출력**
```
Before: 8
After: 4
```

**풀이**
```python
cnt_before = 0
cnt_after = 0

def find1(x):
    global cnt_before
    cnt_before += 1
    if parent1[x] != x:
        return find1(parent1[x])
    return x

def find2(x):
    global cnt_after
    cnt_after += 1
    if parent2[x] != x:
        parent2[x] = find2(parent2[x])
    return parent2[x]

n, m = map(int, input().split())
edges = [tuple(map(int, input().split())) for _ in range(m)]

parent1 = list(range(n + 1))
parent2 = list(range(n + 1))

for a, b in edges:
    parent1[find1(b)] = find1(a)
    parent2[find2(b)] = find2(a)

for i in range(1, n + 1):
    find1(i)
    find2(i)

print(f"Before: {cnt_before}")
print(f"After: {cnt_after}")
```

