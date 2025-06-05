# 🔺 힙(heapq) 유형 문제집

Python `heapq` 모듈을 활용한 **우선순위 큐, 최소/최대힙, k번째 수 찾기** 등을  
실전 스타일로 반복 연습할 수 있는 문제 10선.

---

## ✅ 문제 1. k번째 최소값

**설명**: 수열에서 k번째로 작은 수를 출력하라.

**입력**
```
5 3
4 1 7 9 2
```

**출력**
```
4
```

**풀이**
```python
import heapq

n, k = map(int, input().split())
arr = list(map(int, input().split()))
heapq.heapify(arr)
for _ in range(k-1):
    heapq.heappop(arr)
print(heapq.heappop(arr))
```

---

## ✅ 문제 2. 최대힙 구현

**설명**: 최대힙처럼 작동하는 큐를 구현하라. 삽입과 꺼낸 값 출력

**입력**
```
5
push 3
push 1
pop
push 4
pop
```

**출력**
```
3
4
```

**풀이**
```python
import heapq

hq = []
for _ in range(int(input())):
    cmd = input().split()
    if cmd[0] == 'push':
        heapq.heappush(hq, -int(cmd[1]))
    elif cmd[0] == 'pop':
        print(-heapq.heappop(hq))
```

---

## ✅ 문제 3. 배열 병합 정렬

**설명**: 여러 정렬 배열을 하나로 병합해 출력하라.

**입력**
```
2
3 1 3 5
4 2 4 6 8
```

**출력**
```
1 2 3 4 5 6 8
```

**풀이**
```python
import heapq

n = int(input())
arr = []
for _ in range(n):
    tmp = list(map(int, input().split()))[1:]
    arr.extend(tmp)
heapq.heapify(arr)
while arr:
    print(heapq.heappop(arr), end=' ')
```

---

## ✅ 문제 4. 최소 회의실 수

**설명**: 회의 시작, 끝 시간이 주어진다. 겹치지 않게 배정할 최소 회의실 개수 출력

**입력**
```
3
1 4
2 5
3 6
```

**출력**
```
3
```

**풀이**
```python
import heapq

n = int(input())
meetings = [tuple(map(int, input().split())) for _ in range(n)]
meetings.sort()

rooms = []
for s, e in meetings:
    if rooms and rooms[0] <= s:
        heapq.heappop(rooms)
    heapq.heappush(rooms, e)
print(len(rooms))
```

---

## ✅ 문제 5. 온라인 중앙값

**설명**: 수열이 한 개씩 주어진다. 그때마다 현재까지의 중앙값을 출력하라.

**입력**
```
5
1 5 2 10 -1
```

**출력**
```
1
1
2
2
2
```

**풀이**
```python
import heapq

left = []   # 최대힙 (작은 값)
right = []  # 최소힙 (큰 값)

for x in map(int, input().split()):
    heapq.heappush(left, -x)
    if right and -left[0] > right[0]:
        heapq.heappush(right, -heapq.heappop(left))
        heapq.heappush(left, -heapq.heappop(right))
    if len(left) > len(right) + 1:
        heapq.heappush(right, -heapq.heappop(left))
    print(-left[0])
```

---

## ✅ 문제 6. 우선순위 작업 처리

**설명**: 작업이 (이름, 우선순위)로 주어진다. 우선순위 높은 작업부터 이름을 출력하라.

**입력**
```
3
a 2
b 5
c 3
```

**출력**
```
b
c
a
```

**풀이**
```python
import heapq

hq = []
for _ in range(int(input())):
    name, pri = input().split()
    heapq.heappush(hq, (-int(pri), name))
while hq:
    print(heapq.heappop(hq)[1])
```

---

## ✅ 문제 7. 최대 이득 스케줄링

**설명**: n일 중 하나를 택해 이득을 받을 수 있는 작업들이 주어진다. 최대 이득을 구하라.

**입력**
```
3
1 20
2 30
2 10
```

**출력**
```
50
```

**풀이**
```python
import heapq

n = int(input())
jobs = []
for _ in range(n):
    d, p = map(int, input().split())
    jobs.append((d, p))
jobs.sort()

res = []
day = 1
for d, p in jobs:
    heapq.heappush(res, p)
    if len(res) > d:
        heapq.heappop(res)
print(sum(res))
```

---

## ✅ 문제 8. 최소 연산 비용

**설명**: 수들을 하나로 합칠 때, 매번 두 수를 더하는 비용이 발생. 전체 최소 비용을 출력하라.

**입력**
```
4
1 2 3 4
```

**출력**
```
19
```

**풀이**
```python
import heapq

arr = list(map(int, input().split()))
heapq.heapify(arr)
cost = 0
while len(arr) > 1:
    a = heapq.heappop(arr)
    b = heapq.heappop(arr)
    cost += a + b
    heapq.heappush(arr, a + b)
print(cost)
```

---

## ✅ 문제 9. 상위 K개 숫자

**설명**: 수열에서 가장 큰 숫자 K개를 오름차순으로 출력하라.

**입력**
```
6 3
4 1 9 2 10 5
```

**출력**
```
5 9 10
```

**풀이**
```python
import heapq

n, k = map(int, input().split())
arr = list(map(int, input().split()))
res = heapq.nlargest(k, arr)
print(*sorted(res))
```

---

## ✅ 문제 10. 남은 음식 수

**설명**: 각 음식이 먹는 데 걸리는 시간 배열이 주어진다. k초 후에 먹을 음식 번호를 출력하라. (그리디+힙 응용)

**입력**
```
3
3 1 2
5
```

**출력**
```
1
```

**풀이**
```python
import heapq

n = int(input())
food = list(map(int, input().split()))
k = int(input())

if sum(food) <= k:
    print(-1)
else:
    hq = []
    for i, time in enumerate(food):
        heapq.heappush(hq, (time, i + 1))
    total = 0
    prev = 0
    length = n

    while hq and total + (hq[0][0] - prev) * length <= k:
        now = heapq.heappop(hq)[0]
        total += (now - prev) * length
        length -= 1
        prev = now

    res = sorted(hq, key=lambda x: x[1])
    print(res[(k - total) % length][1])
```

