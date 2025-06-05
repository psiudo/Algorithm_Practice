# 🟨 정렬(sort) 유형 문제집

Python의 `sort()`와 `sorted()`를 활용한  
**기본 정렬부터 lambda 커스텀 정렬, 다중 키 정렬, 안정 정렬 유지**까지 실전 문제 10선

---

## ✅ 문제 1. 숫자 정렬

**설명**: 주어진 수열을 오름차순으로 정렬하라.

**입력**
```
5
3 1 4 5 2
```

**출력**
```
1 2 3 4 5
```

**풀이**
```python
input()
arr = list(map(int, input().split()))
arr.sort()
print(*arr)
```

---

## ✅ 문제 2. 길이 우선 단어 정렬

**설명**: 단어들을 길이 오름차순으로 정렬하고, 길이가 같으면 사전순으로 출력

**입력**
```
5
apple
pie
banana
ant
app
```

**출력**
```
ant
app
pie
apple
banana
```

**풀이**
```python
words = [input() for _ in range(int(input()))]
words.sort(key=lambda x: (len(x), x))
print('\n'.join(words))
```

---

## ✅ 문제 3. 국영수 정렬

**설명**: 학생 이름, 국어 영어 수학 점수가 주어진다.  
정렬 기준: 국어 내림 → 영어 오름 → 수학 내림 → 이름 사전순

**입력**
```
3
james 90 80 70
amy 90 80 80
bruce 90 70 80
```

**출력**
```
bruce
amy
james
```

**풀이**
```python
students = [input().split() for _ in range(int(input()))]
students.sort(key=lambda x: (-int(x[1]), int(x[2]), -int(x[3]), x[0]))
for s in students:
    print(s[0])
```

---

## ✅ 문제 4. 좌표 정렬

**설명**: (x, y) 좌표를 y 오름차순, 같으면 x 오름차순으로 정렬

**입력**
```
3
1 2
3 1
2 2
```

**출력**
```
3 1
1 2
2 2
```

**풀이**
```python
arr = [tuple(map(int, input().split())) for _ in range(int(input()))]
arr.sort(key=lambda x: (x[1], x[0]))
for x, y in arr:
    print(x, y)
```

---

## ✅ 문제 5. 정렬된 배열 병합

**설명**: 두 정렬 배열이 주어진다. 하나의 정렬된 배열로 병합해 출력

**입력**
```
3
1 3 5
4
2 4 6 8
```

**출력**
```
1 2 3 4 5 6 8
```

**풀이**
```python
input()
a = list(map(int, input().split()))
input()
b = list(map(int, input().split()))
print(*sorted(a + b))
```

---

## ✅ 문제 6. 숫자 문자열 정렬

**설명**: 숫자 문자열 n개가 주어진다. 숫자값 기준으로 오름차순 정렬

**입력**
```
4
30
2
10
100
```

**출력**
```
2
10
30
100
```

**풀이**
```python
arr = [input() for _ in range(int(input()))]
arr.sort(key=int)
print('\n'.join(arr))
```

---

## ✅ 문제 7. 사전순 인덱스 정렬

**설명**: 문자열이 주어진다. 각 접미사 인덱스를 사전순으로 정렬해 출력

**입력**
```
banana
```

**출력**
```
5 3 1 0 4 2
```

**풀이**
```python
s = input()
res = sorted(range(len(s)), key=lambda i: s[i:])
print(*res)
```

---

## ✅ 문제 8. 절댓값 기준 정렬

**설명**: 정수 배열을 절댓값 기준 오름차순, 같으면 실제 값 기준으로 정렬

**입력**
```
7
-1 -3 2 3 -2 0 -3
```

**출력**
```
0 -1 -2 2 -3 -3 3
```

**풀이**
```python
arr = list(map(int, input().split()))
arr.sort(key=lambda x: (abs(x), x))
print(*arr)
```

---

## ✅ 문제 9. 안정 정렬 확인

**설명**: (문자, 숫자) 쌍이 주어진다. 숫자 기준 정렬 후 문자 순서가 보존되면 "STABLE", 아니면 "NOT STABLE"

**입력**
```
4
a 1
b 3
c 1
d 2
```

**출출력**
```
STABLE
```

**풀이**
```python
n = int(input())
arr = [input().split() for _ in range(n)]
sorted_arr = sorted(arr, key=lambda x: int(x[1]))
stable = True
prev = {}
for i, (c, num) in enumerate(arr):
    if num not in prev:
        prev[num] = []
    prev[num].append(i)

for i, (c, num) in enumerate(sorted_arr):
    if arr.index([c, num]) != prev[num].pop(0):
        stable = False
        break
print("STABLE" if stable else "NOT STABLE")
```

---

## ✅ 문제 10. 마지막 글자 기준 정렬

**설명**: 문자열이 주어질 때, 마지막 글자 기준으로 정렬하고 출력하라.

**입력**
```
5
map
zip
cap
tap
nap
```

**출력**
```
cap
nap
map
tap
zip
```

**풀이**
```python
arr = [input() for _ in range(int(input()))]
arr.sort(key=lambda x: x[-1])
print('\n'.join(arr))
```

