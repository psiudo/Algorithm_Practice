# 🟥 문자열(string) 유형 문제집

문자열 탐색, 비교, 변형, 정규 표현 및 투 포인터 등  
**실전 문자열 처리 알고리즘 10선**

---

## ✅ 문제 1. 아나그램 판별

**설명**: 두 문자열이 같은 글자 구성(아나그램)이면 1, 아니면 0 출력

**입력**
```
listen
silent
```

**출력**
```
1
```

**풀이**
```python
from collections import Counter

a = input()
b = input()
print(1 if Counter(a) == Counter(b) else 0)
```

---

## ✅ 문제 2. 회문 판별

**설명**: 대소문자 구분 없이, 회문이면 YES, 아니면 NO 출력

**입력**
```
RaceCar
```

**출력**
```
YES
```

**풀이**
```python
s = input().lower()
print("YES" if s == s[::-1] else "NO")
```

---

## ✅ 문제 3. 문자열 압축

**설명**: 연속된 문자를 개수로 압축 (예: `aaabb` → `a3b2`)

**입력**
```
aaabbccaaa
```

**출력**
```
a3b2c2a3
```

**풀이**
```python
s = input()
res = []
cnt = 1
for i in range(1, len(s)):
    if s[i] == s[i-1]:
        cnt += 1
    else:
        res.append(f"{s[i-1]}{cnt}")
        cnt = 1
res.append(f"{s[-1]}{cnt}")
print(''.join(res))
```

---

## ✅ 문제 4. 가장 많이 나온 문자

**설명**: 문자열에서 가장 많이 등장한 문자를 출력. 동점이면 알파벳 순

**입력**
```
banana
```

**출력**
```
a
```

**풀이**
```python
from collections import Counter

s = input()
c = Counter(s)
max_freq = max(c.values())
res = sorted([k for k, v in c.items() if v == max_freq])
print(res[0])
```

---

## ✅ 문제 5. 부분 문자열 여부

**설명**: 두 문자열 s, t가 주어질 때 t가 s의 부분 문자열이면 1, 아니면 0 출력

**입력**
```
abcdef
cde
```

**출력**
```
1
```

**풀이**
```python
s = input()
t = input()
print(1 if t in s else 0)
```

---

## ✅ 문제 6. 문자열 뒤집기

**설명**: 문자열이 주어진다. 단어 단위로 뒤집은 결과 출력

**입력**
```
hello world python
```

**출력**
```
python world hello
```

**풀이**
```python
s = input()
print(' '.join(reversed(s.split())))
```

---

## ✅ 문제 7. 투 포인터 부분합

**설명**: 숫자 문자열과 목표합 m이 주어진다. 연속된 부분 문자열 중 합이 m인 구간 개수를 출력

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
arr = list(map(int, input().split()))

cnt = 0
s = 0
left = 0

for right in range(n):
    s += arr[right]
    while s > m:
        s -= arr[left]
        left += 1
    if s == m:
        cnt += 1
print(cnt)
```

---

## ✅ 문제 8. 숫자만 추출

**설명**: 문자열에서 숫자만 추출하여 정수로 출력

**입력**
```
g0en2T0s8eSoft
```

**출력**
```
208
```

**풀이**
```python
s = input()
res = ''.join([c for c in s if c.isdigit()])
print(int(res))
```

---

## ✅ 문제 9. 가장 긴 동일 문자

**설명**: 문자열에서 같은 문자가 연속된 가장 긴 구간의 길이 출력

**입력**
```
aaabbbaaacccaaa
```

**출력**
```
4
```

**풀이**
```python
s = input()
max_len = cnt = 1

for i in range(1, len(s)):
    if s[i] == s[i-1]:
        cnt += 1
        max_len = max(max_len, cnt)
    else:
        cnt = 1
print(max_len)
```

---

## ✅ 문제 10. 문자열 회전 비교

**설명**: 두 문자열 A, B가 주어졌을 때 A를 회전시켜서 B로 만들 수 있으면 YES 출력

**입력**
```
abcde
deabc
```

**출력**
```
YES
```

**풀이**
```python
a = input()
b = input()
print("YES" if b in a + a else "NO")
```

