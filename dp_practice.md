# 🟪 동적 계획법 (DP) 유형 문제집

점화식 기반 동적 계획법 문제 10선  
**배낭, 연속합, 계단, 타일, 최장 부분 수열 등 실전 핵심 DP 패턴 집중 연습**

---

## ✅ 문제 1. 1로 만들기

**설명**: 정수 n이 주어진다. 연산 3가지 중 선택하여 1로 만드는 최소 횟수를 출력  
(1) -1  
(2) /2  
(3) /3

**입력**
```
10
```

**출력**
```
3
```

**풀이**
```python
n = int(input())
dp = [0] * (n + 1)

for i in range(2, n + 1):
    dp[i] = dp[i - 1] + 1
    if i % 2 == 0:
        dp[i] = min(dp[i], dp[i // 2] + 1)
    if i % 3 == 0:
        dp[i] = min(dp[i], dp[i // 3] + 1)

print(dp[n])
```

---

## ✅ 문제 2. 계단 오르기

**설명**: 계단마다 점수가 주어진다. 연속 3칸은 못 밟을 때 최대 점수를 출력

**입력**
```
6
10
20
15
25
10
20
```

**출력**
```
75
```

**풀이**
```python
n = int(input())
stairs = [0] + [int(input()) for _ in range(n)]
dp = [0] * (n + 1)
if n >= 1: dp[1] = stairs[1]
if n >= 2: dp[2] = stairs[1] + stairs[2]

for i in range(3, n + 1):
    dp[i] = max(dp[i - 2], dp[i - 3] + stairs[i - 1]) + stairs[i]
print(dp[n])
```

---

## ✅ 문제 3. 포도주 시식

**설명**: 연속 3잔은 못 마신다. 최대 마실 수 있는 양 출력

**입력**
```
6
6
10
13
9
8
1
```

**출력**
```
33
```

**풀이**
```python
n = int(input())
wine = [0] + [int(input()) for _ in range(n)]
dp = [0] * (n + 1)
dp[1] = wine[1]
if n >= 2: dp[2] = wine[1] + wine[2]

for i in range(3, n + 1):
    dp[i] = max(dp[i - 1], dp[i - 2] + wine[i], dp[i - 3] + wine[i - 1] + wine[i])
print(dp[n])
```

---

## ✅ 문제 4. 타일 채우기 1 (2×n)

**설명**: 2×n 타일을 1×2, 2×1로 채우는 경우의 수

**입력**
```
5
```

**출력**
```
8
```

**풀이**
```python
n = int(input())
dp = [0] * (n + 2)
dp[1] = 1
dp[2] = 2

for i in range(3, n + 1):
    dp[i] = dp[i - 1] + dp[i - 2]
print(dp[n])
```

---

## ✅ 문제 5. 연속합

**설명**: 연속된 수의 합 중 최대합을 출력하라.

**입력**
```
10
10 -4 3 1 5 6 -35 12 21 -1
```

**출력**
```
33
```

**풀이**
```python
n = int(input())
arr = list(map(int, input().split()))
dp = arr[:]

for i in range(1, n):
    dp[i] = max(arr[i], dp[i - 1] + arr[i])
print(max(dp))
```

---

## ✅ 문제 6. 정수 삼각형

**설명**: 삼각형의 꼭대기부터 아래로 내려가면서 최대 합을 출력

**입력**
```
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5
```

**출력**
```
30
```

**풀이**
```python
n = int(input())
tri = [list(map(int, input().split())) for _ in range(n)]

for i in range(1, n):
    for j in range(i + 1):
        if j == 0:
            tri[i][j] += tri[i - 1][j]
        elif j == i:
            tri[i][j] += tri[i - 1][j - 1]
        else:
            tri[i][j] += max(tri[i - 1][j], tri[i - 1][j - 1])
print(max(tri[-1]))
```

---

## ✅ 문제 7. 가장 긴 증가 부분 수열

**설명**: 주어진 수열에서 증가하는 부분 수열의 최대 길이를 출력

**입력**
```
6
10 20 10 30 20 50
```

**출력**
```
4
```

**풀이**
```python
n = int(input())
a = list(map(int, input().split()))
dp = [1]*n

for i in range(n):
    for j in range(i):
        if a[j] < a[i]:
            dp[i] = max(dp[i], dp[j]+1)
print(max(dp))
```

---

## ✅ 문제 8. 계단 수

**설명**: 인접한 자리 차가 1인 수를 계단 수라 한다. 길이 n일 때 계단 수 개수를 10^9로 나눈 나머지 출력

**입력**
```
2
```

**출력**
```
17
```

**풀이**
```python
n = int(input())
mod = 10**9
dp = [[0]*10 for _ in range(n+1)]

for i in range(1, 10):
    dp[1][i] = 1

for i in range(2, n+1):
    for j in range(10):
        if j > 0:
            dp[i][j] += dp[i-1][j-1]
        if j < 9:
            dp[i][j] += dp[i-1][j+1]
        dp[i][j] %= mod

print(sum(dp[n]) % mod)
```

---

## ✅ 문제 9. 2차원 DP: LCS

**설명**: 두 문자열의 가장 긴 공통 부분 수열 길이를 출력

**입력**
```
ACAYKP
CAPCAK
```

**출력**
```
4
```

**풀이**
```python
a = input()
b = input()
n, m = len(a), len(b)
dp = [[0]*(m+1) for _ in range(n+1)]

for i in range(n):
    for j in range(m):
        if a[i] == b[j]:
            dp[i+1][j+1] = dp[i][j] + 1
        else:
            dp[i+1][j+1] = max(dp[i][j+1], dp[i+1][j])
print(dp[n][m])
```

---

## ✅ 문제 10. 평범한 배낭

**설명**: 무게와 가치가 주어진 n개의 물건에서, 최대 무게를 넘지 않게 담았을 때 최대 가치를 출력

**입력**
```
4 7
6 13
4 8
3 6
5 12
```

**출력**
```
14
```

**풀이**
```python
n, k = map(int, input().split())
items = [tuple(map(int, input().split())) for _ in range(n)]
dp = [0] * (k + 1)

for w, v in items:
    for i in range(k, w - 1, -1):
        dp[i] = max(dp[i], dp[i - w] + v)
print(max(dp))
```

