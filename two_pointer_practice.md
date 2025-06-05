# 🟩 Two Pointer / Sliding Window Practice

**연속된 범위 탐색**, **조건 만족 구간 최적화**, **정렬된 데이터 처리** 등에 자주 등장하는  
투 포인터/슬라이딩 윈도우 기반 문제 10선

---

## ✅ 문제 1. 특정 합 구간 개수 세기

**설명**: 수열에서 연속된 구간 중, 합이 `M`인 경우의 수를 구하라.

**입력**
```
8 3
1 2 1 1 1 2 1 3
```

**출력**
```
5
```

**풀이**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
left = right = s = cnt = 0

while True:
    if s >= m:
        s -= a[left]
        left += 1
    elif right == n:
        break
    else:
        s += a[right]
        right += 1
    if s == m:
        cnt += 1

print(cnt)
```

---

## ✅ 문제 2. 서로 다른 숫자 개수가 K개 이하인 최장 연속 부분 수열

**입력**
```
9 2
1 2 1 3 4 2 3 5 1
```

**출력**
```
4
```

**풀이**
```python
from collections import defaultdict

n, k = map(int, input().split())
a = list(map(int, input().split()))
cnt = defaultdict(int)
left = res = 0

for right in range(n):
    cnt[a[right]] += 1
    while len(cnt) > k:
        cnt[a[left]] -= 1
        if cnt[a[left]] == 0:
            del cnt[a[left]]
        left += 1
    res = max(res, right - left + 1)

print(res)
```

---

## ✅ 문제 3. 고유 문자로만 이루어진 가장 긴 부분 문자열 길이

**입력**
```
abcabcbb
```

**출력**
```
3
```

**풀이**
```python
s = input()
seen = set()
left = res = 0

for right in range(len(s)):
    while s[right] in seen:
        seen.remove(s[left])
        left += 1
    seen.add(s[right])
    res = max(res, right - left + 1)

print(res)
```

---

## ✅ 문제 4. K개의 다른 수를 포함하는 가장 긴 연속 부분 배열

**입력**
```
10 3
1 2 1 3 4 2 3 5 1 2
```

**출력**
```
5
```

**풀이**
```python
from collections import defaultdict

n, k = map(int, input().split())
a = list(map(int, input().split()))
cnt = defaultdict(int)
left = res = 0

for right in range(n):
    cnt[a[right]] += 1
    while len(cnt) > k:
        cnt[a[left]] -= 1
        if cnt[a[left]] == 0:
            del cnt[a[left]]
        left += 1
    res = max(res, right - left + 1)

print(res)
```

---

## ✅ 문제 5. 슬라이딩 윈도우 최대값

**설명**: 윈도우 크기 `k`로 오른쪽으로 이동하면서 최대값을 출력

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
res = []

for i in range(n):
    while dq and a[dq[-1]] <= a[i]:
        dq.pop()
    dq.append(i)
    if dq[0] <= i - k:
        dq.popleft()
    if i >= k - 1:
        res.append(a[dq[0]])

print(*res)
```
---

## ✅ 문제 6. 부분합이 S 이상인 최소 길이

**설명**: 연속된 부분 수열 중 합이 `S` 이상인 것 중 가장 짧은 길이를 출력. 없으면 0

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
total = left = 0
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

## ✅ 문제 7. 정렬된 배열 두 개의 합이 M이 되는 경우의 수

**설명**: 정렬된 배열 A, B에서 두 수의 합이 `M`이 되는 쌍의 개수를 출력

**입력**
```
4 4 5
1 3 4 6
1 2 3 4
```

**출력**
```
3
```

**풀이**
```python
n, m, target = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
b.sort(reverse=True)
i = j = cnt = 0

while i < n and j < m:
    s = a[i] + b[j]
    if s == target:
        cnt += 1
        i += 1
        j += 1
    elif s < target:
        i += 1
    else:
        j += 1

print(cnt)
```

---

## ✅ 문제 8. 가장 긴 1의 연속된 구간 (최대 k개의 0을 1로 바꿀 수 있음)

**입력**
```
10 2
1 0 0 1 1 0 1 0 1 1
```

**출력**
```
6
```

**풀이**
```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
left = res = zero = 0

for right in range(n):
    if a[right] == 0:
        zero += 1
    while zero > k:
        if a[left] == 0:
            zero -= 1
        left += 1
    res = max(res, right - left + 1)

print(res)
```

---

## ✅ 문제 9. 가장 긴 괄호 부분 문자열

**설명**: '('와 ')'로 이루어진 문자열 중 올바른 괄호로 된 가장 긴 부분 문자열의 길이 출력

**입력**
```
(()())()
```

**출력**
```
8
```

**풀이**
```python
s = input()
stack = []
last = -1
res = 0

for i, c in enumerate(s):
    if c == '(':
        stack.append(i)
    else:
        if not stack:
            last = i
        else:
            stack.pop()
            if not stack:
                res = max(res, i - last)
            else:
                res = max(res, i - stack[-1])

print(res)
```

---

## ✅ 문제 10. 아나그램 찾기

**설명**: 문자열에서 특정 문자열과 아나그램인 부분 문자열의 개수 구하기

**입력**
```
cbaebabacd
abc
```

**출력**
```
2
```

**풀이**
```python
from collections import Counter

s = input()
p = input()
n, k = len(s), len(p)
p_cnt = Counter(p)
s_cnt = Counter()
res = 0

for i in range(n):
    s_cnt[s[i]] += 1
    if i >= k:
        if s_cnt[s[i - k]] == 1:
            del s_cnt[s[i - k]]
        else:
            s_cnt[s[i - k]] -= 1
    if s_cnt == p_cnt:
        res += 1

print(res)
```

---