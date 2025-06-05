# 🟩 그래프 탐색 (DFS / BFS) 유형 문제집

**DFS/BFS 탐색 패턴**, **인접리스트/행렬**, **최단 거리**, **연결성 판별** 등  
실전에서 반복되는 그래프 패턴을 손에 익히기 위한 10문제

---

## ✅ 문제 1. 연결 요소 개수

**설명**: 무방향 그래프에서 연결된 컴포넌트 개수를 출력

**입력**
```
6 5
1 2
2 5
5 1
3 4
4 6
```

**출력**
```
2
```

**풀이**
```python
from collections import defaultdict

n, m = map(int, input().split())
graph = defaultdict(list)
for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)

visited = [False] * (n + 1)

def dfs(x):
    visited[x] = True
    for nx in graph[x]:
        if not visited[nx]:
            dfs(nx)

cnt = 0
for i in range(1, n + 1):
    if not visited[i]:
        dfs(i)
        cnt += 1
print(cnt)
```

---

## ✅ 문제 2. 미로 최단거리 (BFS)

**설명**: 0은 벽, 1은 길. (1,1)부터 (n,m)까지 최단 거리 출력. 이동은 상하좌우

**입력**
```
5 6
101010
111111
000001
111111
111111
```

**출력**
```
10
```

**풀이**
```python
from collections import deque

n, m = map(int, input().split())
maze = [list(map(int, list(input()))) for _ in range(n)]
visited = [[0]*m for _ in range(n)]

q = deque()
q.append((0, 0))
visited[0][0] = 1

while q:
    x, y = q.popleft()
    for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
        nx, ny = x+dx, y+dy
        if 0<=nx<n and 0<=ny<m and maze[nx][ny] == 1 and visited[nx][ny] == 0:
            visited[nx][ny] = visited[x][y] + 1
            q.append((nx, ny))
print(visited[n-1][m-1])
```

---

## ✅ 문제 3. DFS 순회 순서

**설명**: 인접리스트로 주어진 그래프를 DFS로 순회하며 방문 순서 출력

**입력**
```
4 5 1
1 2
1 3
1 4
2 4
3 4
```

**출력**
```
1 2 4 3
```

**풀이**
```python
from collections import defaultdict

n, m, start = map(int, input().split())
graph = defaultdict(list)
for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)
for v in graph:
    graph[v].sort()

visited = [False] * (n + 1)
res = []

def dfs(x):
    visited[x] = True
    res.append(x)
    for nx in graph[x]:
        if not visited[nx]:
            dfs(nx)

dfs(start)
print(*res)
```

---

## ✅ 문제 4. BFS 거리 배열

**설명**: 무방향 그래프에서 시작점부터 각 노드까지의 최단 거리 출력

**입력**
```
6 5 1
1 2
1 3
2 4
3 5
5 6
```

**출력**
```
0 1 1 2 2 3
```

**풀이**
```python
from collections import deque, defaultdict

n, m, start = map(int, input().split())
graph = defaultdict(list)
for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)

dist = [-1] * (n + 1)
dist[start] = 0
q = deque([start])

while q:
    cur = q.popleft()
    for nxt in graph[cur]:
        if dist[nxt] == -1:
            dist[nxt] = dist[cur] + 1
            q.append(nxt)

print(*dist[1:])
```

---

## ✅ 문제 5. 단지 번호 붙이기

**설명**: 1이 연결된 영역(상하좌우)을 단지로 보고, 단지 수와 각 단지의 집 수를 오름차순 출력

**입력**
```
7
0110100
0110101
1110101
0000111
0100000
0111110
0111000
```

**출력**
```
3
7
8
9
```

**풀이**
```python
n = int(input())
grid = [list(map(int, list(input()))) for _ in range(n)]
visited = [[False]*n for _ in range(n)]

def dfs(x, y):
    visited[x][y] = True
    cnt = 1
    for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
        nx, ny = x+dx, y+dy
        if 0<=nx<n and 0<=ny<n and not visited[nx][ny] and grid[nx][ny]==1:
            cnt += dfs(nx, ny)
    return cnt

res = []
for i in range(n):
    for j in range(n):
        if grid[i][j]==1 and not visited[i][j]:
            res.append(dfs(i,j))
print(len(res))
for c in sorted(res):
    print(c)
```

---

## ✅ 문제 6. 토마토

**설명**: 익은 토마토(1)가 안 익은 것(0)을 4방향으로 익힌다. 모두 익는 최소 일수를 출력. 못 익으면 −1

**입력**
```
6 4
0 0 0 0 0 0
1 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1
```

**출력**
```
8
```

**풀이**
```python
from collections import deque

m, n = map(int, input().split())
box = [list(map(int, input().split())) for _ in range(n)]
q = deque()

for i in range(n):
    for j in range(m):
        if box[i][j] == 1:
            q.append((i,j))

while q:
    x, y = q.popleft()
    for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
        nx, ny = x+dx, y+dy
        if 0<=nx<n and 0<=ny<m and box[nx][ny] == 0:
            box[nx][ny] = box[x][y] + 1
            q.append((nx, ny))

res = max(map(max, box))
print(res - 1 if all(0 not in row for row in box) else -1)
```

---

## ✅ 문제 7. 숨바꼭질

**설명**: n에서 시작해, 1초에 x-1, x+1, 2x로 이동 가능. k까지 가는 최소 시간

**입력**
```
5 17
```

**출력**
```
4
```

**풀이**
```python
from collections import deque

n, k = map(int, input().split())
dist = [-1] * 100001
q = deque([n])
dist[n] = 0

while q:
    cur = q.popleft()
    for nxt in (cur-1, cur+1, cur*2):
        if 0 <= nxt <= 100000 and dist[nxt] == -1:
            dist[nxt] = dist[cur] + 1
            q.append(nxt)

print(dist[k])
```

---

## ✅ 문제 8. BFS로 색칠 영역 세기

**설명**: 격자에서 1로 연결된 모든 영역 수 출력. 대각선 제외

**입력**
```
3 3
1 0 1
0 1 0
1 0 1
```

**출력**
```
5
```

**풀이**
```python
from collections import deque

n, m = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(n)]
visited = [[False]*m for _ in range(n)]

def bfs(x, y):
    q = deque([(x,y)])
    visited[x][y] = True
    while q:
        x, y = q.popleft()
        for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
            nx, ny = x+dx, y+dy
            if 0<=nx<n and 0<=ny<m and not visited[nx][ny] and grid[nx][ny]==1:
                visited[nx][ny] = True
                q.append((nx, ny))

cnt = 0
for i in range(n):
    for j in range(m):
        if grid[i][j]==1 and not visited[i][j]:
            bfs(i,j)
            cnt += 1
print(cnt)
```

---

## ✅ 문제 9. 방향 그래프 사이클 탐지

**설명**: 방향 그래프에서 사이클이 존재하면 YES

**입력**
```
3 3
1 2
2 3
3 1
```

**출력**
```
YES
```

**풀이**
```python
from collections import defaultdict

n, m = map(int, input().split())
graph = defaultdict(list)
for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)

visited = [0] * (n + 1)
found = False

def dfs(x):
    global found
    visited[x] = 1
    for nxt in graph[x]:
        if visited[nxt] == 0:
            dfs(nxt)
        elif visited[nxt] == 1:
            found = True
    visited[x] = 2

for i in range(1, n + 1):
    if visited[i] == 0:
        dfs(i)
print("YES" if found else "NO")
```

---

## ✅ 문제 10. DFS로 모든 경로 탐색

**설명**: 1번 정점에서 n번 정점까지 가능한 모든 경로의 수를 출력

**입력**
```
5 5
1 2
1 3
2 4
3 4
4 5
```

**출력**
```
2
```

**풀이**
```python
from collections import defaultdict

n, m = map(int, input().split())
graph = defaultdict(list)
for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)

cnt = 0
visited = [False] * (n + 1)

def dfs(x):
    global cnt
    if x == n:
        cnt += 1
        return
    for nxt in graph[x]:
        if not visited[nxt]:
            visited[nxt] = True
            dfs(nxt)
            visited[nxt] = False

visited[1] = True
dfs(1)
print(cnt)
```

