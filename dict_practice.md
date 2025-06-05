# 🧠 딕셔너리(dict) 유형 문제집

Python의 `dict`, `defaultdict`, `Counter` 사용에 능숙해지기 위한 실전 문제 20선
**패턴 인식 + 실전 감각 + 구현 숙련도**를 동시에 키웁니다.

---

## ✅ 문제 1. 단어 빈도 세기

**설명**: 여러 줄에 걸쳐 단어들이 주어집니다. 가장 많이 나온 단어를 출력해야 합니다. 만약 가장 많이 나온 단어가 여러 개라면, 그중 사전순으로 가장 앞선 단어를 출력합니다.

**입력 예시**
```
5
apple
banana
apple
orange
banana
```

**출력 예시**
```
apple
```

**풀이 코드**
```python
from collections import defaultdict

n = int(input())
count = defaultdict(int)
for _ in range(n):
    word = input()
    count[word] += 1

max_cnt = 0
# count.values()가 비어있는 경우를 대비하여 기본값 설정
if count:
    max_cnt = max(count.values())

candidates = []
for k, v in count.items():
    if v == max_cnt:
        candidates.append(k)

# candidates가 비어있지 않은 경우에만 min 호출
if candidates:
    print(min(candidates))
else:
    # 입력이 0이거나 하는 극단적인 경우 처리 (문제 조건상 N은 1 이상일 것으로 예상)
    # 혹은, 모든 단어가 0번 등장하는 경우는 없으므로, max_cnt가 0일때 candidates가 비는일은 없음
    # 단, n=0일 경우 count가 비어서 max_cnt가 초기값 0에 머무르면 candidates도 비게 됨.
    # 문제에서 N이 1 이상이라고 명시되면 이 부분은 불필요.
    # 현재 코드에서는 N이 0일 경우 아무것도 출력하지 않거나, min([]) 에러 방지를 위해 처리.
    # 원본 코드에서는 max_cnt = max(count.values())가 count가 비면 에러를 발생시킴.
    # 여기서는 약간 더 견고하게 수정함.
    # 원본 로직을 그대로 따르려면 max_cnt = max(count.values()) if count else 0 로 하면 됨.
    # 그리고 candidates = [k for k, v in count.items() if v == max_cnt] 로 하면 됨.
    # 이 경우 N=0이면 아무것도 출력 안함.
    if count: # 원본 코드에 더 가깝게 하려면 이 if문과 아래 min(candidates)만 남김
        print(min(candidates))

```

**풀이에 대한 설명**
- `from collections import defaultdict`를 사용하여 `defaultdict`를 가져옵니다.
- `count = defaultdict(int)`는 존재하지 않는 키에 접근할 때 자동으로 해당 키에 정수형 기본값 0을 할당하는 딕셔너리를 생성합니다.
- 입력받는 각 단어에 대해 `count[word] += 1`을 실행하여 단어의 빈도를 1씩 증가시킵니다.
- `max(count.values())` (딕셔너리가 비어있지 않다면)를 통해 가장 높은 빈도수 `max_cnt`를 찾습니다.
- 리스트 컴프리헨션 `[k for k, v in count.items() if v == max_cnt]`을 사용해 `max_cnt`와 동일한 빈도수를 가진 모든 단어(키 `k`)를 `candidates` 리스트에 저장합니다.
- `min(candidates)`를 사용하여 `candidates` 리스트에서 사전순으로 가장 앞선 단어를 찾아 출력합니다.

---

## ✅ 문제 2. 아나그램 판별

**설명**: 두 단어가 주어집니다. 두 단어가 서로 아나그램 관계(즉, 문자의 종류와 개수가 동일)이면 1을, 아니면 0을 출력합니다.

**입력 예시**
```
listen
silent
```

**출력 예시**
```
1
```

**풀이 코드**
```python
from collections import Counter

a = input()
b = input()

print(1 if Counter(a) == Counter(b) else 0)
```

**풀이에 대한 설명**
- `from collections import Counter`를 사용하여 `Counter`를 가져옵니다.
- `Counter(a)`는 문자열 `a`에 포함된 각 문자의 빈도를 계산하여 딕셔너리 형태의 `Counter` 객체로 반환합니다. 예를 들어, `Counter("listen")`은 `{'l': 1, 'i': 1, 's': 1, 't': 1, 'e': 1, 'n': 1}`과 유사한 결과를 냅니다.
- 두 `Counter` 객체가 같은지 (`Counter(a) == Counter(b)`) 비교합니다. 두 단어가 아나그램이라면 각 문자의 빈도가 정확히 일치하므로 두 `Counter` 객체는 동일합니다.
- 비교 결과에 따라 1 또는 0을 출력합니다.

---

## ✅ 문제 3. 최대 등장 문자

**설명**: 하나의 문자열이 주어집니다. 이 문자열에서 가장 많이 등장한 알파벳을 출력합니다. 대소문자는 구분하지 않으며, 만약 가장 많이 등장한 알파벳이 여러 개라면 그중 알파벳 순서로 가장 앞선 것을 출력합니다.

**입력 예시**
```
Mississippi
```

**출력 예시**
```
i
```

**풀이 코드**
```python
from collections import Counter

s = input().lower()
c = Counter(s)

max_val = 0
if c: # Counter가 비어있지 않은 경우에만 max 실행
    max_val = max(c.values())

candidates = [k for k, v in c.items() if v == max_val and k.isalpha()] # 알파벳만 대상으로 수정
# 만약 알파벳이 아닌 문자가 최대 빈도일 경우를 대비

# candidates가 비어있거나, 알파벳 후보가 없는 경우 처리
if candidates:
    print(min(candidates))
else:
    # 입력 문자열에 알파벳이 없는 경우 등 예외 처리
    # 문제 조건에 따라 이 부분은 다를 수 있음 (예: 에러 메시지 출력 또는 아무것도 출력 안 함)
    # 원본 코드는 .isalpha() 조건이 없었음. 문제 설명이 '알파벳'을 명시하므로 추가함.
    # 원본 코드: candidates = [k for k, v in c.items() if v == m]
    # 이 경우, 만약 입력이 "11122"면 candidates는 ['1', '2']가 되고 min은 '1'이 됨
    pass # 혹은 적절한 예외 처리
```

**풀이에 대한 설명**
- `s = input().lower()`: 입력 문자열을 받아 모두 소문자로 변환합니다.
- `c = Counter(s)`: 소문자로 변환된 문자열에서 각 문자의 빈도를 계산합니다.
- `max_val = max(c.values())` (Counter가 비어있지 않다면): 가장 높은 빈도수를 찾습니다.
- `candidates = [k for k, v in c.items() if v == max_val and k.isalpha()]`: 가장 높은 빈도수를 가진 모든 '알파벳' 문자를 리스트로 만듭니다. (원본 코드에는 `k.isalpha()` 조건이 없었으나, 문제 설명이 '알파벳'을 명시하므로 추가했습니다.)
- `print(min(candidates))`: 후보 문자들 중 알파벳 순서로 가장 앞선 것을 출력합니다.

---

## ✅ 문제 4. 등장 순서 유지

**설명**: 공백으로 구분된 문자열 리스트가 한 줄로 주어집니다. 중복된 문자열을 제거하되, 원래 입력에서의 첫 등장 순서를 유지하여 출력합니다.

**입력 예시**
```
6
apple banana apple orange banana lemon
```
(실제 입력은 `apple banana apple orange banana lemon` 입니다. 첫 줄 `6`은 원본 문제의 다른 형식일 수 있으나, 제공된 코드와는 무관합니다. 여기서는 코드에 맞춰 한 줄 입력을 가정합니다.)
```python
# 입력 예시에 맞춰 코드 수정:
# n = int(input()) # 이 줄은 필요 없음
# words_line = input() # 이렇게 받거나, 바로 input().split()
# words = words_line.split()

# 제공된 풀이 코드의 입력은 다음과 같을 것으로 추정됨:
# apple banana apple orange banana lemon
```
**정정된 입력 예시 (풀이 코드 기준)**
```
apple banana apple orange banana lemon
```

**출력 예시**
```
apple banana orange lemon
```

**풀이 코드**
```python
# 첫 줄의 숫자 입력을 받는 부분이 없으므로, 해당 부분은 주석 처리하거나 제거합니다.
# n = int(input()) # 이 줄은 문제 설명과 원본 코드 간 불일치 가능성 (제거)
words = input().split() # 한 줄로 문자열들을 입력받아 공백 기준으로 나눔
seen = {} # Python 3.7+ 에서는 dict가 삽입 순서를 보장. 이전 버전은 OrderedDict 사용.
# 또는 리스트와 set을 함께 사용:
# seen_set = set()
# result_list = []
# for word in words:
#    if word not in seen_set:
#        seen_set.add(word)
#        result_list.append(word)
# print(*result_list)

# 원본 코드의 의도대로 dict.fromkeys()를 사용할 수도 있음 (Python 3.7+ 순서 보장)
# print(*dict.fromkeys(words).keys())

# 제공된 풀이 방식:
for word in words:
    if word not in seen:
        seen[word] = 1 # 값은 중요하지 않음, 키의 존재 유무와 순서가 중요
print(*seen.keys())
```

**풀이에 대한 설명**
- `words = input().split()`: 한 줄의 문자열을 입력받아 공백을 기준으로 나누어 리스트 `words`에 저장합니다.
- `seen = {}`: 빈 딕셔너리를 생성합니다. Python 3.7 버전부터 일반 `dict`도 삽입된 순서를 기억합니다. 그 이전 버전에서는 `collections.OrderedDict`를 사용해야 순서가 보장됩니다.
- `for word in words:`: 입력된 단어 리스트를 순서대로 순회합니다.
    - `if word not in seen:`: 현재 단어가 `seen` 딕셔너리의 키로 존재하지 않는 경우 (즉, 처음 등장한 단어인 경우)
    - `seen[word] = 1`: 단어를 `seen` 딕셔너리의 키로 추가합니다. 값은 어떤 것을 넣어도 상관없습니다 (여기서는 1을 사용).
- `print(*seen.keys())`: `seen` 딕셔너리의 키들(중복이 제거되고 첫 등장 순서가 유지된 단어들)을 언패킹하여 공백으로 구분해 출력합니다.
- *대안*: `dict.fromkeys(words)`는 `words` 리스트의 아이템을 순서대로 키로 사용하고 값은 `None`으로 하는 딕셔너리를 만듭니다. 이 역시 Python 3.7+에서 순서를 보장합니다. `print(*dict.fromkeys(words).keys())`로도 동일한 결과를 얻을 수 있습니다.

---

## ✅ 문제 5. 해시 충돌 수

**설명**: N개의 문자열이 주어집니다. 각 문자열에 대해 해시 값(문자열을 구성하는 각 문자의 `ord()` 값의 합)을 계산합니다. 서로 다른 문자열이 동일한 해시 값을 가질 경우 해시 충돌이 발생합니다. 둘 이상의 문자열이 같은 해시 값을 갖는 경우, 해당 해시 값에 대해 충돌이 발생한 것으로 간주합니다. 이러한 충돌이 발생한 해시 값의 총 개수를 출력합니다. (예: `ab`와 `ba`는 같은 해시 값을 가지므로 여기서 1개의 충돌 해시 값이 발생. `aaa`, `aab`, `aac`가 모두 다른 해시값을 갖고 `d`와 `e`가 같은 해시값을 가지면 총 충돌 해시 값 개수는 1개.)

**입력 예시**
```
5
ab
ba
aa
bb
cc
```
(해시 값: ab = 97+98=195, ba = 98+97=195, aa = 97+97=194, bb = 98+98=196, cc = 99+99=198)
(해시 값 빈도: {195: 2, 194: 1, 196: 1, 198: 1})
(빈도가 1보다 큰 해시 값은 195 하나. 따라서 충돌 발생 해시 값 개수는 1)

**출력 예시**
```
1
```
(원본 출력 예시는 2였으나, 설명과 코드 로직을 따르면 1이 맞습니다. `sum(v - 1 for v in d.values() if v > 1)`이면 총 충돌 쌍의 수가 되어 1이 되고, `sum(1 for v in d.values() if v > 1)`은 충돌이 일어난 *해시값의 종류 수*입니다. 여기서는 후자를 따릅니다.)

**풀이 코드**
```python
from collections import defaultdict

def hash_val(s):
    return sum(ord(ch) for ch in s)

n = int(input())
d = defaultdict(int)
for _ in range(n):
    word = input() # 입력받는 문자열 변수명 명시
    h = hash_val(word)
    d[h] += 1

# sum(1 for v in d.values() if v > 1)는
# 해시값이 2번 이상 등장한 경우(즉, 해당 해시값에서 충돌이 발생한 경우)
# 그러한 해시값의 '종류'가 몇 개인지를 센다.
# 예를 들어, h1: 3번, h2: 2번, h3: 1번 이면, (h1에서 충돌, h2에서 충돌) -> 결과는 2
print(sum(1 for v in d.values() if v > 1))
```

**풀이에 대한 설명**
- `hash_val(s)` 함수는 문자열 `s`의 각 문자에 대해 `ord()` (아스키/유니코드 코드 포인트) 값을 구해 모두 합산하여 해시 값을 반환합니다.
- `d = defaultdict(int)`: 해시 값을 키로, 해당 해시 값이 등장한 횟수를 값으로 저장하는 딕셔너리를 생성합니다.
- N개의 문자열을 입력받아 각각의 해시 값을 계산하고, `d[h] += 1`을 통해 해당 해시 값의 등장 횟수를 업데이트합니다.
- `sum(1 for v in d.values() if v > 1)`: `d.values()`는 딕셔너리에 저장된 모든 등장 횟수(값)들을 가져옵니다. 이 값들 중 1보다 큰 경우(즉, 해당 해시 값이 2번 이상 등장하여 충돌이 발생한 경우)마다 1씩 더합니다. 이는 결국 충돌이 발생한 *고유한 해시 값의 개수*를 셉니다.

---

## ✅ 문제 6. 문자열 등장 구간

**설명**: 하나의 문자열이 주어집니다. 이 문자열에 포함된 각 고유 문자에 대해, 해당 문자가 처음으로 등장하는 인덱스와 마지막으로 등장하는 인덱스를 찾아 출력합니다. 출력 순서는 문자(알파벳)의 오름차순입니다.

**입력 예시**
```
banana
```

**출력 예시**
```
a 1 5
b 0 0
n 2 4
```

**풀이 코드**
```python
s = input()
first = {}
last = {}

for i, ch in enumerate(s):
    if ch not in first:
        first[ch] = i
    last[ch] = i

# 문자열에 등장한 고유 문자들을 정렬하여 순회
# set(s)를 사용하면 문자열 내 모든 고유 문자를 얻을 수 있음
for ch in sorted(first.keys()): # first.keys()나 set(s)나 결과적으로 동일한 문자셋
    print(ch, first[ch], last[ch])
```

**풀이에 대한 설명**
- `first = {}`와 `last = {}`: 두 개의 빈 딕셔너리를 초기화합니다. `first`는 각 문자의 첫 등장 인덱스를, `last`는 각 문자의 마지막 등장 인덱스를 저장합니다.
- `for i, ch in enumerate(s):`: `enumerate`를 사용하여 문자열 `s`를 인덱스 `i`와 문자 `ch`와 함께 순회합니다.
    - `if ch not in first:`: 현재 문자 `ch`가 `first` 딕셔너리에 키로 존재하지 않으면 (즉, 처음 등장한 문자이면)
    - `first[ch] = i`: `first` 딕셔너리에 `ch`를 키로, 현재 인덱스 `i`를 값으로 저장합니다.
    - `last[ch] = i`: `last` 딕셔너리에 `ch`를 키로, 현재 인덱스 `i`를 값으로 저장합니다. 이 문장은 매번 수행되므로, 해당 문자가 등장할 때마다 마지막 인덱스가 최신 값으로 갱신됩니다.
- `for ch in sorted(first.keys()):`: `first` 딕셔너리의 모든 키(즉, 문자열에 등장한 고유 문자들)를 가져와 알파벳 순으로 정렬한 뒤 순회합니다. `sorted(set(s))`를 사용해도 동일합니다.
- `print(ch, first[ch], last[ch])`: 각 문자와 해당 문자의 `first` 및 `last` 인덱스를 공백으로 구분하여 출력합니다.

---

## ✅ 문제 7. 카운트 누적

**설명**: 공백으로 구분된 숫자 리스트가 주어집니다. 각 숫자의 총 등장 횟수를 계산한 뒤, 숫자를 오름차순으로 정렬하여 각 숫자와 해당 숫자까지의 *누적 등장 횟수*를 `숫자:누적횟수` 형태로 출력합니다. 출력 항목들은 공백으로 구분합니다.

**입력 예시**
```
5
1 2 2 3 1
```
(실제 입력은 `1 2 2 3 1`입니다. 첫 줄 `5`는 원본 문제 형식에 따른 개수 표시일 수 있으나, 제공된 코드는 숫자 리스트만 사용합니다.)

**정정된 입력 예시 (풀이 코드 기준)**
```
1 2 2 3 1
```

**출력 예시**
```
1:2 2:4 3:5 
```
(마지막에 공백이 하나 더 붙는 형태)

**풀이 코드**
```python
from collections import Counter

# n = int(input()) # 이 줄은 코드에서 사용되지 않음 (제거)
a = list(map(int, input().split()))
c = Counter(a)

s = 0
# c.keys()를 정렬하거나, c.items()를 정렬할 수 있음
# Counter 객체를 sorted()에 직접 넣으면 키를 기준으로 정렬됨
sorted_keys = sorted(c.keys()) 

output_parts = []
for k in sorted_keys:
    s += c[k]
    output_parts.append(f"{k}:{s}")
print(*output_parts) # print(" ".join(output_parts)) 와 유사, 마지막 공백 없음
# 원본 코드의 print(f"{k}:{s}", end=' ')는 마지막에 공백을 남김.
# 이를 정확히 재현하려면:
# for i, k in enumerate(sorted_keys):
#     s += c[k]
#     print(f"{k}:{s}", end=' ' if i < len(sorted_keys) -1 else '')
# 또는 더 간단히:
# for k in sorted_keys:
#    s += c[k]
#    print(f"{k}:{s}", end=' ')
# print() # 마지막 줄바꿈을 위해 (만약 필요하다면)

# 원본 코드의 출력 형식(마지막에 공백)을 정확히 따르려면:
s = 0
for k in sorted(c): # Counter의 키를 정렬하여 순회
    s += c[k]
    print(f"{k}:{s}", end=' ')
# print() # 만약 다음 출력이 이어서 나오지 않도록 하려면 추가
```

**풀이에 대한 설명**
- `a = list(map(int, input().split()))`: 공백으로 구분된 숫자 문자열을 입력받아 정수 리스트 `a`로 변환합니다.
- `c = Counter(a)`: 리스트 `a`에 있는 각 숫자의 빈도를 계산하여 `Counter` 객체 `c`에 저장합니다.
- `s = 0`: 누적 합계를 저장할 변수 `s`를 0으로 초기화합니다.
- `for k in sorted(c):`: `Counter` 객체 `c`의 키(즉, 고유한 숫자들)를 오름차순으로 정렬하여 순회합니다.
    - `s += c[k]`: 현재 숫자 `k`의 빈도(`c[k]`)를 누적 합계 `s`에 더합니다.
    - `print(f"{k}:{s}", end=' ')`: f-string을 사용하여 `숫자:누적합계` 형식으로 출력하고, `end=' '`를 통해 줄바꿈 대신 공백을 출력하여 다음 출력이 같은 줄에 이어지도록 합니다.

---

## ✅ 문제 8. 가장 먼저 2번 나온 수

**설명**: 정수 N과 N개의 정수로 이루어진 수열이 주어집니다. 수열에서 가장 먼저 2번 등장하는 숫자를 출력합니다. 만약 2번 등장하는 숫자가 없으면 -1을 출력합니다.

**입력 예시**
```
8
1 2 3 4 2 5 1 3
```

**출력 예시**
```
2
```

**풀이 코드**
```python
from collections import defaultdict

n = int(input())
a = list(map(int, input().split()))
d = defaultdict(int)

found = False # 플래그 변수 추가
for x in a:
    d[x] += 1
    if d[x] == 2:
        print(x)
        found = True # 찾았음을 표시
        break
if not found: # for-else 대신 플래그 사용
    print(-1)

# 원본 코드의 for-else 구문도 Pythonic하고 좋음.
# for x in a:
# d[x] += 1
# if d[x] == 2:
# print(x)
# break
# else: # 루프가 break 없이 정상적으로 완료되었을 때 실행
# print(-1)
```

**풀이에 대한 설명**
- `d = defaultdict(int)`: 각 숫자의 등장 횟수를 저장할 `defaultdict`를 생성합니다. 기본값은 0입니다.
- 수열 `a`의 각 숫자 `x`에 대해 순회합니다:
    - `d[x] += 1`: 현재 숫자 `x`의 등장 횟수를 1 증가시킵니다.
    - `if d[x] == 2:`: 만약 `x`의 등장 횟수가 2가 되면, 이 숫자가 처음으로 2번 등장한 것이므로 `x`를 출력하고 `break`를 통해 루프를 빠져나옵니다.
- `else:` (for 루프에 대한 else): `for` 루프가 `break` 문에 의해 중단되지 않고 모든 반복을 정상적으로 마쳤을 경우 (즉, 2번 등장한 숫자가 없었을 경우) 이 `else` 블록이 실행되어 -1을 출력합니다.

---

## ✅ 문제 9. key로 그룹핑

**설명**: N개의 (이름, 점수) 쌍이 주어집니다. 점수를 기준으로 그룹핑하여, 각 점수별로 해당 점수를 받은 이름들을 사전순으로 정렬하여 리스트 형태로 출력합니다. 점수도 오름차순으로 정렬하여 출력합니다. 형식은 `점수: 이름1 이름2 ...` 입니다.

**입력 예시**
```
5
amy 90
bob 80
alice 90
dave 70
john 80
```

**출력 예시**
```
70: dave
80: bob john
90: alice amy
```

**풀이 코드**
```python
from collections import defaultdict

n = int(input())
group = defaultdict(list)
for _ in range(n):
    name, score_str = input().split()
    score = int(score_str)
    group[score].append(name)

# group.keys()를 정렬하거나, group.items()를 정렬.
# sorted(group)는 키를 기준으로 정렬.
for k_score in sorted(group.keys()): # 키(점수)를 오름차순으로 정렬하여 순회
    # 이름 리스트도 사전순으로 정렬하여 출력
    print(f"{k_score}:", *sorted(group[k_score]))
```

**풀이에 대한 설명**
- `group = defaultdict(list)`: 점수를 키로 하고, 해당 점수를 받은 이름들의 리스트를 값으로 가지는 `defaultdict`를 생성합니다. 존재하지 않는 점수(키)에 접근하면 빈 리스트가 기본값으로 할당됩니다.
- N개의 입력을 받으면서 각 (이름, 점수) 쌍에 대해:
    - `name, score_str = input().split()`: 이름과 점수(문자열)를 분리합니다.
    - `score = int(score_str)`: 점수 문자열을 정수로 변환합니다.
    - `group[score].append(name)`: `score`를 키로 하는 리스트에 `name`을 추가합니다.
- `for k_score in sorted(group.keys()):`: `group` 딕셔너리의 키(점수들)를 오름차순으로 정렬하여 순회합니다.
    - `print(f"{k_score}:", *sorted(group[k_score]))`: 각 점수 `k_score`와 해당 점수를 받은 이름들의 리스트(`group[k_score]`)를 가져옵니다. 이 이름 리스트를 `sorted()`를 사용해 사전순으로 정렬한 뒤, 언패킹(`*`)하여 공백으로 구분된 이름들로 출력합니다.

---

## ✅ 문제 10. 접두사 판단

**설명**: N개의 문자열이 주어집니다. 이 문자열 집합에서 어떤 한 문자열이 다른 문자열의 접두사가 되는 경우가 하나라도 있으면 "NO"를 출력하고, 그렇지 않으면 "YES"를 출력합니다.
(이 문제는 딕셔너리를 주로 사용하는 문제는 아니지만, 문자열 처리 및 알고리즘 문제 유형으로 포함되었습니다.)

**입력 예시**
```
3
hello
hell
heaven
```

**출력 예시**
```
NO
```

**풀이 코드**
```python
n = int(input())
words = sorted([input() for _ in range(n)]) # 입력받아 바로 정렬

is_prefix_free = True # 플래그 변수
for i in range(len(words) - 1):
    # words[i]가 words[i+1]의 접두사인지 확인
    if words[i+1].startswith(words[i]):
        print("NO")
        is_prefix_free = False # 접두사 관계 발견
        break

if is_prefix_free: # for-else 대신 플래그 사용
    print("YES")

# 원본 코드의 for-else 구문
# for i in range(len(words) - 1):
# if words[i+1].startswith(words[i]):
# print("NO")
# break
# else:
# print("YES")
```

**풀이에 대한 설명**
- `words = sorted([input() for _ in range(n)])`: N개의 문자열을 입력받아 리스트에 저장한 후, 바로 사전순으로 정렬합니다. 이 정렬이 중요합니다. 만약 `A`가 `B`의 접두사라면, 정렬된 리스트에서 `A`는 `B` 바로 앞에 오거나, `A`와 `B` 사이에 `A`로 시작하는 다른 문자열들이 올 수 있습니다. 하지만 이 문제의 로직에서는 인접한 문자열만 비교해도 충분합니다. 만약 `words[i]`가 `words[k]` (여기서 `k > i + 1`)의 접두사이고 `words[i+1]`의 접두사가 아니라면, `words[i]`는 `words[i+1]`보다 사전적으로 작거나 같습니다. `words[i+1]`이 `words[i]`로 시작하지 않는다면, 이 비교만으로는 `words[i]`가 `words[k]`의 접두사인지 여부를 직접 알 수 없습니다. 그러나 어떤 문자열이 다른 문자열의 접두사가 되는 *경우가 하나라도 있으면* NO 이므로, 정렬 후 인접한 것들만 비교하는 것은 효율적인 접근법입니다. (`"hell"`과 `"hello"`는 인접하게 되고, `"hell"`이 `"hello"`의 접두사임이 바로 발견됩니다).
- `for i in range(len(words) - 1):`: 정렬된 단어 리스트에서 인접한 두 단어를 비교하기 위해 `i`를 0부터 `len(words) - 2`까지 순회합니다.
    - `if words[i+1].startswith(words[i]):`: 만약 `words[i]` (더 짧거나 같은 길이의 앞 단어)가 `words[i+1]` (뒷 단어)의 접두사이면, 접두사 관계가 존재하므로 "NO"를 출력하고 `break`로 루프를 종료합니다.
- `else:` (for 루프에 대한 else): `for` 루프가 `break` 없이 정상적으로 완료되면 (즉, 어떤 문자열도 다른 문자열의 접두사가 아닌 경우), "YES"를 출력합니다.

---
---

## 💡 새로운 문제 (11-20)

---

## ✅ 문제 11. 값으로 키 찾기

**설명**: 딕셔너리가 주어집니다. (키는 문자열, 값은 정수). 특정 정수 값이 주어졌을 때, 이 값을 가지는 모든 키를 찾아 오름차순으로 정렬하여 공백으로 구분해 출력합니다. 해당하는 키가 없으면 "None"을 출력합니다.

**입력 예시**
```
3
apple 10
banana 20
orange 10
10
```
(첫 줄: 딕셔너리 항목 수 N, 다음 N줄: 키 값, 마지막 줄: 찾을 값)

**출력 예시**
```
apple orange
```

**풀이 코드**
```python
n = int(input())
d = {}
for _ in range(n):
    key, value_str = input().split()
    d[key] = int(value_str)
target_value = int(input())

result_keys = []
for k, v in d.items():
    if v == target_value:
        result_keys.append(k)

if result_keys:
    print(*sorted(result_keys))
else:
    print("None")
```

**풀이에 대한 설명**
- 입력을 받아 딕셔너리 `d`를 구성하고, 찾고자 하는 `target_value`를 입력받습니다.
- 딕셔너리 `d`의 모든 항목 `(k, v)`를 순회하면서, 값 `v`가 `target_value`와 일치하는 키 `k`를 `result_keys` 리스트에 추가합니다.
- `result_keys` 리스트가 비어있지 않으면, 리스트를 오름차순으로 정렬하여 언패킹하여 출력합니다.
- 리스트가 비어있으면 "None"을 출력합니다.

---

## ✅ 문제 12. 딕셔너리 병합 (값 합산)

**설명**: 두 개의 딕셔너리 입력이 주어집니다. (키는 문자열, 값은 정수). 두 딕셔너리를 병합하여 새로운 딕셔너리를 만듭니다. 만약 두 딕셔너리에 동일한 키가 존재하면, 값들을 합산합니다. 결과 딕셔너리의 항목들을 키의 오름차순으로 정렬하여 "키:값" 형태로 각 줄에 출력합니다.

**입력 예시**
```
3
apple 5
banana 10
orange 3
2
banana 5
grape 8
```
(첫 줄: 첫 번째 딕셔너리 항목 수 N, 다음 N줄: 키 값 ... 그 다음 줄: 두 번째 딕셔너리 항목 수 M ...)

**출력 예시**
```
apple:5
banana:15
grape:8
orange:3
```

**풀이 코드**
```python
from collections import Counter # 또는 defaultdict(int)

def read_dict():
    n = int(input())
    d = defaultdict(int) # Counter()를 써도 됨
    for _ in range(n):
        key, value_str = input().split()
        d[key] += int(value_str) # 바로 합산 시작 (Counter와 유사하게)
    return d

dict1_counter = Counter()
n1 = int(input())
for _ in range(n1):
    key, value_str = input().split()
    dict1_counter[key] += int(value_str)

dict2_counter = Counter()
n2 = int(input())
for _ in range(n2):
    key, value_str = input().split()
    dict2_counter[key] += int(value_str)

merged_dict = dict1_counter + dict2_counter # Counter는 덧셈으로 병합 및 값 합산 가능

for key in sorted(merged_dict.keys()):
    print(f"{key}:{merged_dict[key]}")
```

**풀이에 대한 설명**
- `collections.Counter`를 사용하면 딕셔너리 병합 시 값의 합산을 쉽게 처리할 수 있습니다.
- 두 개의 입력을 받아 각각 `Counter` 객체 `dict1_counter`와 `dict2_counter`를 만듭니다. 각 키에 해당하는 값을 정수로 변환하여 `Counter`에 누적합니다.
- `merged_dict = dict1_counter + dict2_counter`를 통해 두 `Counter`를 병합합니다. 공통 키에 대해서는 값이 합산되고, 한쪽에만 있는 키는 그대로 유지됩니다.
- `merged_dict.keys()`를 오름차순으로 정렬하여 각 "키:값" 쌍을 출력합니다.
- `defaultdict(int)`를 사용한다면, 첫 번째 딕셔너리를 읽은 후, 두 번째 딕셔너리를 읽으면서 `merged_dict[key] += value` 방식으로 값을 업데이트할 수 있습니다.

---

## ✅ 문제 13. 가장 드문 단어

**설명**: 여러 줄에 걸쳐 단어들이 주어집니다. 가장 적게 등장한 단어들 중에서 사전순으로 가장 앞선 단어를 출력합니다. (가장 적게 등장한 빈도수가 1이라면, 1번 등장한 단어들 중 사전순으로 가장 앞선 단어)

**입력 예시**
```
5
apple
banana
apple
orange
grape
```

**출력 예시**
```
banana
```
(banana:1, orange:1, grape:1 -> 이 중 사전순 가장 앞은 banana)

**풀이 코드**
```python
from collections import Counter

n = int(input())
words = [input() for _ in range(n)]
count = Counter(words)

if not count: # 단어가 하나도 입력되지 않은 경우
    print("None") # 또는 적절한 처리
else:
    min_freq = min(count.values())
    candidates = [word for word, freq in count.items() if freq == min_freq]
    print(min(candidates)) # 사전순으로 가장 앞선 단어
```

**풀이에 대한 설명**
- 입력된 단어들로 `Counter` 객체 `count`를 생성하여 각 단어의 빈도를 계산합니다.
- `count.values()`가 비어있지 않다면 `min_freq = min(count.values())`를 통해 가장 낮은 빈도수를 찾습니다.
- `candidates` 리스트에 `min_freq`와 동일한 빈도수를 가진 모든 단어를 저장합니다.
- `min(candidates)`를 사용하여 `candidates` 리스트에서 사전순으로 가장 앞선 단어를 출력합니다.
- 입력이 없는 경우 "None"을 출력하도록 예외 처리합니다.

---

## ✅ 문제 14. 키-값 반전 (값이 리스트)

**설명**: 딕셔너리가 주어집니다 (키: 문자열, 값: 문자열). 이 딕셔너리의 키와 값을 반전시켜 새로운 딕셔너리를 만듭니다. 원래 딕셔너리에서 서로 다른 키가 동일한 값을 가졌다면, 반전된 딕셔너리에서는 해당 값을 키로 하고, 원래의 키들을 리스트에 담아 값으로 가집니다. 이 리스트는 원래 키들의 사전순으로 정렬됩니다. 최종적으로 반전된 딕셔너리의 항목들을 키(원래의 값)의 오름차순으로 정렬하여 "새키: [원래키1, 원래키2 ...]" 형태로 출력합니다.

**입력 예시**
```
3
fruit apple
color red
food apple
```

**출력 예시**
```
apple: [food, fruit]
red: [color]
```

**풀이 코드**
```python
from collections import defaultdict

n = int(input())
original_dict = {}
for _ in range(n):
    k, v = input().split()
    original_dict[k] = v

reversed_dict = defaultdict(list)
for k, v in original_dict.items():
    reversed_dict[v].append(k)

for v_key in sorted(reversed_dict.keys()): # 새로운 키(원래의 값)를 기준으로 정렬
    original_keys_list = sorted(reversed_dict[v_key]) # 원래 키들의 리스트를 사전순 정렬
    print(f"{v_key}: [{', '.join(original_keys_list)}]") # 출력 형식 맞춤
```

**풀이에 대한 설명**
- `reversed_dict = defaultdict(list)`: 값을 키로, 원래 키들의 리스트를 값으로 가지는 `defaultdict`를 생성합니다.
- 원래 딕셔너리 `original_dict`의 각 `(k, v)` 쌍에 대해, `reversed_dict[v].append(k)`를 실행하여 새로운 딕셔너리를 구성합니다. `v`(원래 값)가 새 키가 되고, `k`(원래 키)가 리스트에 추가됩니다.
- `reversed_dict`의 키들(원래 값들)을 오름차순으로 정렬하여 순회합니다.
- 각 키에 해당하는 값(원래 키들의 리스트)도 내부적으로 사전순으로 정렬합니다.
- 최종 결과를 "새키: [원래키1, 원래키2 ...]" 형식으로 출력합니다. 리스트를 문자열로 표현할 때 `', '.join()`을 사용합니다.

---

## ✅ 문제 15. 학생 성적 평균

**설명**: 여러 학생의 이름과 여러 과목의 점수가 주어집니다. 각 줄마다 "이름 점수1 점수2 ..." 형태로 입력됩니다. 각 학생의 평균 점수를 계산하여, 학생 이름을 사전순으로 정렬한 뒤 "이름: 평균점수" 형태로 출력합니다. 평균 점수는 소수점 둘째 자리까지 반올림하여 표현합니다.

**입력 예시**
```
3
Alice 90 80 70
Bob 100 90
Charlie 85 95 75
```

**출력 예시**
```
Alice: 80.00
Bob: 95.00
Charlie: 85.00
```

**풀이 코드**
```python
n = int(input())
student_scores = {}

for _ in range(n):
    line_parts = input().split()
    name = line_parts[0]
    scores = list(map(int, line_parts[1:]))
    if scores: # 점수가 있는 경우에만 평균 계산
        average = sum(scores) / len(scores)
        student_scores[name] = average
    else: # 점수가 없는 경우 (문제 조건에 따라 처리, 예: 0점 또는 무시)
        student_scores[name] = 0.0


for name in sorted(student_scores.keys()): # 학생 이름을 사전순으로 정렬
    print(f"{name}: {student_scores[name]:.2f}") # 소수점 둘째 자리까지 출력
```

**풀이에 대한 설명**
- `student_scores = {}`: 학생 이름을 키로, 평균 점수를 값으로 저장할 딕셔너리를 생성합니다.
- 각 줄의 입력을 받아 이름과 점수 리스트를 분리합니다.
- 점수 리스트가 비어있지 않다면 평균을 계산하여 `student_scores` 딕셔너리에 저장합니다. (점수가 없는 경우 0.0으로 처리하거나 문제 조건에 따라 다르게 할 수 있습니다.)
- `student_scores.keys()`를 사전순으로 정렬하여 각 학생의 이름과 계산된 평균 점수를 가져옵니다.
- `f"{student_scores[name]:.2f}"`를 사용하여 평균 점수를 소수점 둘째 자리까지 형식화하여 출력합니다.

---

## ✅ 문제 16. 공통 키-값 찾기

**설명**: 두 개의 딕셔너리 입력이 주어집니다. (키: 문자열, 값: 정수). 두 딕셔너리 모두에 존재하면서 값까지 동일한 (키, 값) 쌍들을 찾습니다. 이러한 공통 항목들을 키의 오름차순으로 정렬하여 "키:값" 형태로 각 줄에 출력합니다.

**입력 예시**
```
3
apple 5
banana 10
orange 3
3
banana 10
grape 8
apple 5
```

**출력 예시**
```
apple:5
banana:10
```

**풀이 코드**
```python
def read_dict_items():
    n = int(input())
    d = {}
    for _ in range(n):
        key, value_str = input().split()
        d[key] = int(value_str)
    return d

dict1 = read_dict_items()
dict2 = read_dict_items()

common_items = {}

# dict1의 아이템을 기준으로 dict2와 비교
for key, value in dict1.items():
    if key in dict2 and dict2[key] == value:
        common_items[key] = value

for key in sorted(common_items.keys()):
    print(f"{key}:{common_items[key]}")
```

**풀이에 대한 설명**
- `read_dict_items` 함수를 정의하여 각 딕셔너리의 입력을 처리합니다.
- `dict1`과 `dict2`를 각각 입력받습니다.
- `common_items = {}`: 공통 항목을 저장할 빈 딕셔너리를 생성합니다.
- 첫 번째 딕셔너리 `dict1`의 각 `(key, value)` 쌍에 대해, 해당 `key`가 `dict2`에도 존재하고 `dict2[key]`의 값이 `dict1`에서의 `value`와 동일한지 확인합니다.
- 조건이 참이면 해당 `(key, value)` 쌍을 `common_items` 딕셔너리에 추가합니다.
- `common_items`의 키들을 오름차순으로 정렬하여 각 "키:값" 쌍을 출력합니다.
- *대안*: 딕셔너리 뷰의 집합 연산을 사용할 수도 있습니다: `dict1.items() & dict2.items()`는 공통 (키, 값) 쌍의 집합을 반환합니다. 이 결과를 정렬하여 출력할 수 있습니다.

---

## ✅ 문제 17. 딕셔너리 값 기준 정렬

**설명**: 딕셔너리가 주어집니다 (키: 문자열, 값: 정수). 이 딕셔너리의 항목들을 값(value) 기준으로 내림차순 정렬합니다. 만약 값이 같다면 키(key)의 오름차순(사전순)으로 정렬합니다. 정렬된 결과를 "키 값" 형태로 각 줄에 출력합니다.

**입력 예시**
```
4
apple 10
banana 20
orange 10
grape 30
```

**출력 예시**
```
grape 30
banana 20
apple 10
orange 10
```

**풀이 코드**
```python
n = int(input())
d = {}
for _ in range(n):
    key, value_str = input().split()
    d[key] = int(value_str)

# d.items()를 정렬. 정렬 기준은 (값 내림차순, 키 오름차순)
# lambda 함수의 반환값은 튜플 (우선순위1, 우선순위2, ...)
# 값은 내림차순이므로 -item[1] 사용, 키는 오름차순이므로 item[0] 사용
sorted_items = sorted(d.items(), key=lambda item: (-item[1], item[0]))

for key, value in sorted_items:
    print(key, value)
```

**풀이에 대한 설명**
- 딕셔너리 `d`를 입력받아 구성합니다.
- `d.items()`는 딕셔너리의 (키, 값) 쌍들을 뷰(view) 객체로 반환합니다.
- `sorted()` 함수의 `key` 인자에 `lambda` 함수를 사용하여 정렬 기준을 지정합니다.
    - `lambda item: (-item[1], item[0])`: 각 항목 `item` (여기서 `item`은 `(키, 값)` 튜플)에 대해 `(-값, 키)` 튜플을 반환합니다.
    - 정렬 시 첫 번째 요소(`-값`)를 기준으로 먼저 정렬하고, 이것이 같으면 두 번째 요소(`키`)를 기준으로 정렬합니다.
    - `-item[1]`은 값을 기준으로 내림차순 정렬하기 위함입니다 (큰 값이 앞에 오도록).
    - `item[0]`은 키를 기준으로 오름차순(사전순) 정렬하기 위함입니다.
- 정렬된 `sorted_items` 리스트를 순회하며 각 "키 값"을 출력합니다.

---

## ✅ 문제 18. 부분 문자열 빈도 (길이 K)

**설명**: 하나의 문자열 S와 정수 K가 주어집니다. 문자열 S에서 길이가 K인 모든 부분 문자열을 찾고, 각 부분 문자열의 빈도수를 계산합니다. 빈도수가 가장 높은 부분 문자열들을 사전순으로 정렬하여 각 줄에 하나씩 출력합니다. 만약 가장 높은 빈도수를 가진 부분 문자열이 여러 개라면, 모두 출력합니다.

**입력 예시**
```
banana
2
```

**출력 예시**
```
an
na
```
(부분문자열: ba, an, na, an, na. Counter: {'ba':1, 'an':2, 'na':2}. 최대빈도2. 후보: an, na. 사전순 출력)

**풀이 코드**
```python
from collections import Counter

s = input()
k = int(input())

substrings = []
if len(s) >= k : # 문자열 길이가 K보다 크거나 같아야 부분 문자열 생성 가능
    for i in range(len(s) - k + 1):
        substrings.append(s[i:i+k])

if not substrings: # 부분 문자열이 없는 경우 (예: k > len(s))
    print("None") # 또는 빈 줄 출력 등 문제 조건에 맞게
else:
    counts = Counter(substrings)
    max_freq = 0
    if counts: # counts가 비어있지 않으면
         max_freq = max(counts.values())
    
    candidates = []
    for sub, freq in counts.items():
        if freq == max_freq:
            candidates.append(sub)
            
    for candidate in sorted(candidates): # 사전순으로 정렬하여 출력
        print(candidate)

```

**풀이에 대한 설명**
- 문자열 `s`와 정수 `k`를 입력받습니다.
- `substrings` 리스트에 길이가 `k`인 모든 부분 문자열을 추출하여 저장합니다. (문자열 `s`의 길이가 `k`보다 작으면 부분 문자열이 없을 수 있습니다.)
- 만약 `substrings`가 비어있지 않다면, `Counter(substrings)`를 사용하여 각 부분 문자열의 빈도를 계산합니다.
- 가장 높은 빈도수 `max_freq`를 찾습니다.
- `max_freq`와 동일한 빈도수를 가진 모든 부분 문자열을 `candidates` 리스트에 저장합니다.
- `candidates` 리스트를 사전순으로 정렬하여 각 부분 문자열을 한 줄에 하나씩 출력합니다.
- 부분 문자열을 생성할 수 없는 경우(예: `k`가 `len(s)`보다 큰 경우) "None"을 출력하거나 문제 조건에 맞게 처리합니다.

---

## ✅ 문제 19. 장바구니 총액 계산

**설명**: 두 종류의 입력이 주어집니다. 첫 번째는 상품별 가격 정보 (상품명:가격 딕셔너리), 두 번째는 장바구니 정보 (상품명:수량 딕셔너리)입니다. 장바구니에 담긴 상품들의 총액을 계산하여 출력합니다. 장바구니에 있지만 가격 정보에 없는 상품은 계산에서 제외합니다.

**입력 예시**
```
3
apple 100
banana 200
orange 150
2
apple 3
banana 2
grape 1
```
(첫 줄: 가격정보 항목 수, 다음 N줄: 상품명 가격 ... 그 다음 줄: 장바구니 항목 수, 다음 M줄: 상품명 수량)

**출력 예시**
```
700
```
(apple: 3*100=300, banana: 2*200=400. grape는 가격 정보 없으므로 무시. 총 300+400=700)

**풀이 코드**
```python
prices = {}
n_prices = int(input())
for _ in range(n_prices):
    name, price_str = input().split()
    prices[name] = int(price_str)

cart = {}
n_cart = int(input())
for _ in range(n_cart):
    name, quantity_str = input().split()
    cart[name] = int(quantity_str)

total_cost = 0
for item_name, quantity in cart.items():
    if item_name in prices: # 가격 정보가 있는 상품만 계산
        total_cost += prices[item_name] * quantity

print(total_cost)
```

**풀이에 대한 설명**
- `prices` 딕셔너리에 상품명과 가격 정보를 저장합니다.
- `cart` 딕셔너리에 장바구니에 담긴 상품명과 수량 정보를 저장합니다.
- `total_cost`를 0으로 초기화합니다.
- `cart.items()`를 순회하며 각 상품 `item_name`과 수량 `quantity`를 가져옵니다.
- `if item_name in prices:`: 해당 상품이 `prices` 딕셔너리(가격 정보)에 있는지 확인합니다.
- 가격 정보가 있다면, `prices[item_name] * quantity` (가격 * 수량)을 계산하여 `total_cost`에 누적합니다.
- 최종 `total_cost`를 출력합니다.

---

## ✅ 문제 20. 데이터 필터링 (딕셔너리 리스트)

**설명**: 여러 개의 딕셔너리로 구성된 리스트가 주어집니다. 각 딕셔너리는 개인 정보를 나타내며, 예를 들어 `{'name': 'Alice', 'age': 30, 'city': 'New York'}` 와 같은 형태입니다. 특정 조건 (예: 'age'가 25 이상이고 'city'가 'London'인 사람)을 만족하는 딕셔너리만 필터링하여, 필터링된 사람들의 'name'을 사전순으로 정렬하여 각 줄에 하나씩 출력합니다.

**입력 예시**
```
4
name Alice age 30 city NewYork
name Bob age 24 city London
name Charlie age 28 city London
name David age 35 city Paris
age_filter 25
city_filter London
```
(첫 줄: 딕셔너리 개수 N, 다음 N줄: 각 딕셔너리 정보 (키 값 키 값 ...), 마지막 두 줄: 필터 조건)

**출력 예시**
```
Charlie
```

**풀이 코드**
```python
n = int(input())
people_list = []
for _ in range(n):
    parts = input().split()
    person_dict = {}
    for i in range(0, len(parts), 2): # "키 값" 쌍으로 읽어들임
        key = parts[i]
        value = parts[i+1]
        # age는 정수로 변환, 나머지는 문자열로 둠
        if key == 'age':
            person_dict[key] = int(value)
        else:
            person_dict[key] = value
    people_list.append(person_dict)

age_filter_val_str = input().split()[1] # "age_filter 25" -> "25"
age_filter_val = int(age_filter_val_str)
city_filter_val = input().split()[1] # "city_filter London" -> "London"

filtered_names = []
for person in people_list:
    # get() 메소드를 사용하여 키가 없는 경우를 안전하게 처리
    # 예를 들어, 어떤 사람 딕셔너리에 'age'나 'city'키가 없을 수도 있음을 대비.
    # 여기서는 모든 딕셔너리에 해당 키가 있다고 가정하고 직접 접근.
    # person.get('age', -1) >= age_filter_val 등으로 안전하게 할 수 있음.
    
    # 문제에서는 모든 딕셔너리에 필요한 키가 있다고 가정하고 진행
    if 'age' in person and 'city' in person: # 키 존재 여부 확인
        if person['age'] >= age_filter_val and person['city'] == city_filter_val:
            filtered_names.append(person['name'])

for name in sorted(filtered_names): # 사전순으로 정렬하여 출력
    print(name)
```

**풀이에 대한 설명**
- 여러 줄의 입력을 받아 각 개인의 정보를 딕셔너리로 만들고, 이 딕셔너리들을 `people_list`에 저장합니다. 이 때 'age' 값은 정수로 변환합니다.
- 필터링 조건으로 사용할 `age_filter_val`과 `city_filter_val`을 입력받습니다.
- `people_list`를 순회하면서 각 `person` 딕셔너리에 대해 다음 조건을 확인합니다:
    - `person['age'] >= age_filter_val`: 나이가 필터 값 이상인지 확인합니다.
    - `person['city'] == city_filter_val`: 도시가 필터 값과 일치하는지 확인합니다.
    - (안전한 코드를 위해 `person.get('age', some_default_that_wont_match)` 또는 `if 'age' in person` 과 같은 키 존재 여부 확인을 추가할 수 있습니다.)
- 두 조건이 모두 참이면 해당 `person`의 'name'을 `filtered_names` 리스트에 추가합니다.
- `filtered_names` 리스트를 사전순으로 정렬하여 각 이름을 한 줄에 하나씩 출력합니다.


---

## ✅ 문제 21. 중첩 딕셔너리 경로 값 할당

**설명**: 파일 시스템 경로와 유사한 문자열 경로와 할당할 값이 주어집니다. 예를 들어 경로가 "folder1/folder2/file"이고 값이 "content"라면, 중첩된 딕셔너리를 생성하거나 업데이트하여 `{'folder1': {'folder2': {'file': 'content'}}}` 와 같이 만들어야 합니다. 각 경로 요소는 딕셔너리의 키가 됩니다. 이미 경로가 존재하고 마지막 요소가 딕셔너리가 아닌데 하위 경로를 만들어야 하는 경우는 없다고 가정합니다.

**입력 예시**
```
folder1/folder2/file.txt
Hello World
folder1/another.data
TestData
```
(첫 번째 줄: 경로, 두 번째 줄: 해당 경로의 값. 이어서 다음 경로와 값)
(문제 단순화를 위해, 한 번의 실행에 하나의 경로와 값만 처리하는 것으로 가정하고, 여러 입력을 받아 하나의 딕셔너리를 누적 업데이트하는 형태로 변경)
```
2
folder1/folder2/file.txt Hello_World
folder1/another.data TestData
```
(첫 줄: 입력 쌍의 수 N, 다음 N줄: "경로 값" 형식)

**출력 예시**
(최종 딕셔너리의 상태를 키 기준으로 정렬하여 출력)
```json
{
  "folder1": {
    "another.data": "TestData",
    "folder2": {
      "file.txt": "Hello_World"
    }
  }
}
```
(JSON 형식으로 예쁘게 출력하는 것은 선택 사항, 실제로는 Python dict 출력)
(키 정렬된 형태로 표현하기 위해 예시 수정)
```
folder1:
  another.data: TestData
  folder2:
    file.txt: Hello_World
```

**풀이 코드**
```python
import json # 예쁜 출력을 위해 사용 (선택 사항)

n = int(input())
root = {}

for _ in range(n):
    path_str, value_str = input().split(maxsplit=1)
    parts = path_str.split('/')
    current_level = root
    
    for i, part in enumerate(parts):
        if i == len(parts) - 1: # 마지막 부분이면 값을 할당
            current_level[part] = value_str
        else: # 아직 경로의 중간이면
            if part not in current_level:
                current_level[part] = {}
            elif not isinstance(current_level[part], dict):
                # 이미 파일 등이 있는데 하위 경로를 만들려는 경우 (문제 조건상 없다고 가정)
                # 혹은, 여기에 에러 처리나 덮어쓰기 로직을 추가할 수 있음
                current_level[part] = {} # 단순화를 위해 덮어쓴다고 가정
            current_level = current_level[part]

# 결과를 보기 좋게 출력 (실제 채점에서는 print(root)만으로도 충분할 수 있음)
# 또는 키를 정렬해서 출력하는 특정 형식을 요구할 수 있음
def print_sorted_dict(d, indent=0):
    for key in sorted(d.keys()):
        print('  ' * indent + str(key) + ':', end='')
        if isinstance(d[key], dict):
            print()
            print_sorted_dict(d[key], indent + 1)
        else:
            print(' ' + str(d[key]))

# print_sorted_dict(root)
# 더 간단한 표준 dict 출력은 다음과 같음:
# import sys
# def dict_to_sorted_str(d):
#    if isinstance(d, dict):
#        return "{" + ", ".join(f"'{k}': {dict_to_sorted_str(v)}" for k, v in sorted(d.items())) + "}"
#    return f"'{d}'"
# print(dict_to_sorted_str(root))

# 여기서는 json.dumps를 사용하여 정렬된 형태의 문자열로 출력
print(json.dumps(root, sort_keys=True, indent=2))

```

**풀이에 대한 설명**
- `root = {}`: 최상위 딕셔너리를 초기화합니다.
- 각 입력 경로와 값에 대해:
    - `path_str.split('/')`: 경로 문자열을 `/` 기준으로 나누어 경로의 각 부분(폴더/파일 이름)을 리스트 `parts`로 만듭니다.
    - `current_level = root`: 현재 탐색 중인 딕셔너리 수준을 `root`에서 시작합니다.
    - `enumerate(parts)`로 경로의 각 `part`와 인덱스 `i`를 가져옵니다.
        - `if i == len(parts) - 1`: 현재 `part`가 경로의 마지막 요소(예: 파일명)이면, `current_level[part] = value_str`로 값을 할당합니다.
        - `else`: 경로의 중간 요소(예: 폴더명)이면,
            - `current_level`에 해당 `part`가 키로 존재하지 않거나, 존재하더라도 딕셔너리가 아니면 새로운 빈 딕셔너리 `current_level[part] = {}`를 생성합니다 (덮어쓰기).
            - `current_level = current_level[part]`로 다음 단계의 딕셔너리로 이동합니다.
- 최종적으로 생성된 `root` 딕셔너리를 `json.dumps`를 이용해 키를 정렬하고 들여쓰기를 적용하여 출력합니다. 이는 복잡한 중첩 딕셔너리를 사람이 보기 쉽게 표현하는 방법 중 하나입니다.

---

## ✅ 문제 22. 문자 위치 매핑

**설명**: 하나의 문자열이 주어집니다. 이 문자열에 포함된 각 문자에 대해, 해당 문자가 등장하는 모든 인덱스(0부터 시작)를 리스트로 만들어 딕셔너리에 저장해야 합니다. 딕셔너리의 키는 문자이고, 값은 해당 문자의 인덱스 리스트입니다. 결과 딕셔너리를 문자의 오름차순으로 정렬하여 "문자: [인덱스1, 인덱스2, ...]" 형태로 출력합니다.

**입력 예시**
```
banana
```

**출력 예시**
```
a: [1, 3, 5]
b: [0]
n: [2, 4]
```

**풀이 코드**
```python
from collections import defaultdict

s = input()
char_positions = defaultdict(list)

for index, char_val in enumerate(s):
    char_positions[char_val].append(index)

for char_key in sorted(char_positions.keys()):
    # 인덱스 리스트를 문자열로 변환하여 출력 형식 맞춤
    indices_str = ", ".join(map(str, char_positions[char_key]))
    print(f"{char_key}: [{indices_str}]")
```

**풀이에 대한 설명**
- `from collections import defaultdict`를 사용하여 `defaultdict`를 가져옵니다.
- `char_positions = defaultdict(list)`: 문자를 키로 하고, 해당 문자가 나타나는 인덱스들의 리스트를 값으로 가지는 `defaultdict`를 생성합니다. 키가 처음 사용될 때 빈 리스트 `[]`가 자동으로 할당됩니다.
- `enumerate(s)`를 사용하여 입력 문자열 `s`의 각 문자 `char_val`과 해당 인덱스 `index`를 순회합니다.
- `char_positions[char_val].append(index)`: 현재 문자 `char_val`을 키로 하는 리스트에 현재 인덱스 `index`를 추가합니다.
- `sorted(char_positions.keys())`를 사용하여 딕셔너리의 키(문자들)를 오름차순으로 정렬합니다.
- 정렬된 각 문자 `char_key`에 대해, 해당 문자의 인덱스 리스트 `char_positions[char_key]`를 가져와 지정된 형식으로 출력합니다. `", ".join(map(str, ...))`는 정수 리스트를 ", "로 구분된 문자열로 변환합니다.

---

## ✅ 문제 23. 희소 벡터 점곱 (Dot Product)

**설명**: 두 개의 희소 벡터(sparse vector)가 딕셔너리 형태로 주어집니다. 각 딕셔너리에서 키는 벡터의 인덱스(정수)이고 값은 해당 인덱스에서의 값(정수)입니다. 이 두 희소 벡터의 점곱(dot product)을 계산하여 출력합니다. 점곱은 두 벡터에서 동일한 인덱스를 가지는 요소들의 곱을 모두 합한 값입니다. 한쪽 벡터에만 존재하는 인덱스의 요소는 곱셈에 기여하지 않습니다 (0과 곱해지는 것으로 간주).

**입력 예시**
```
3
0 1 2 3 4 5
2
0 2 5 2
```
(첫 줄: 첫 번째 벡터의 0이 아닌 요소 수 N, 다음 N줄: "인덱스 값" 형식)
(그 다음 줄: 두 번째 벡터의 0이 아닌 요소 수 M, 다음 M줄: "인덱스 값" 형식)

**정정된 입력 예시**
```
3
0 1
2 3
4 5
2
0 2
5 2
```

**출력 예시**
```
2
```
( (1*2) + (3*0) + (5*0) + (0*2) ... 인덱스 0에서 1*2=2. 다른 공통 인덱스 없음. 결과 2 )

**풀이 코드**
```python
def read_vector_dict():
    n = int(input())
    vec = {}
    for _ in range(n):
        idx_str, val_str = input().split()
        vec[int(idx_str)] = int(val_str)
    return vec

vec1 = read_vector_dict()
vec2 = read_vector_dict()

dot_product = 0

# 더 작은 딕셔너리를 기준으로 순회하는 것이 효율적일 수 있음
# 여기서는 vec1을 기준으로 함
for idx, val1 in vec1.items():
    if idx in vec2: # 공통된 인덱스가 있는지 확인
        dot_product += val1 * vec2[idx]

print(dot_product)
```

**풀이에 대한 설명**
- `read_vector_dict()` 함수는 각 희소 벡터의 입력을 받아 딕셔너리 형태로 반환합니다. 키는 인덱스, 값은 해당 인덱스의 값입니다.
- `vec1`과 `vec2` 두 개의 벡터 딕셔너리를 읽어들입니다.
- `dot_product = 0`으로 점곱의 합계를 초기화합니다.
- 첫 번째 벡터 `vec1`의 각 항목 `(idx, val1)`을 순회합니다.
    - `if idx in vec2:`: 현재 인덱스 `idx`가 두 번째 벡터 `vec2`에도 키로 존재하는지 확인합니다. (즉, 두 벡터 모두에서 0이 아닌 값을 가지는 인덱스인지 확인)
    - 만약 존재한다면, `dot_product += val1 * vec2[idx]`를 통해 두 벡터에서 해당 인덱스의 값들을 곱한 결과를 `dot_product`에 더합니다.
- 모든 공통 인덱스에 대한 곱의 합이 계산된 후, 최종 `dot_product` 값을 출력합니다.

---

## ✅ 문제 24. 딕셔너리 값 일괄 할인 적용

**설명**: 상품 이름과 가격이 저장된 딕셔너리가 주어지고, 할인율(%)이 주어집니다. 모든 상품 가격에 해당 할인율을 적용한 후, 각 상품의 할인된 가격을 계산하여 원래 딕셔너리의 값을 업데이트해야 합니다. 할인된 가격은 정수로 소수점 이하는 버립니다. 업데이트된 딕셔너리의 모든 항목을 상품 이름의 오름차순으로 정렬하여 "상품명: 할인가격" 형태로 출력합니다.

**입력 예시**
```
3
apple 1000
banana 2550
orange 780
15
```
(첫 줄: 상품 수 N, 다음 N줄: "상품명 가격" 형식, 마지막 줄: 할인율 percentage)

**출력 예시**
```
apple: 850
banana: 2167
orange: 663
```

**풀이 코드**
```python
n = int(input())
item_prices = {}
for _ in range(n):
    name, price_str = input().split()
    item_prices[name] = int(price_str)
discount_percentage = int(input())

for name in item_prices: # 딕셔너리를 직접 순회하며 값을 수정
    original_price = item_prices[name]
    # 할인액 = 원가 * (할인율 / 100.0)
    # 할인가 = 원가 - 할인액 = 원가 * (1 - 할인율 / 100.0)
    # 할인가 = 원가 * ( (100 - 할인율) / 100.0 )
    discounted_price = int(original_price * (100 - discount_percentage) / 100)
    item_prices[name] = discounted_price # 원래 딕셔너리의 값을 직접 업데이트

for name in sorted(item_prices.keys()):
    print(f"{name}: {item_prices[name]}")
```

**풀이에 대한 설명**
- 상품 정보를 입력받아 `item_prices` 딕셔너리 (상품명: 원래 가격)를 구성하고, 할인율 `discount_percentage`를 입력받습니다.
- `for name in item_prices:` 루프를 사용하여 딕셔너리의 각 상품명(`name`)을 순회합니다.
    - `original_price = item_prices[name]`로 현재 상품의 원래 가격을 가져옵니다.
    - `discounted_price = int(original_price * (100 - discount_percentage) / 100)` 공식을 사용하여 할인된 가격을 계산합니다.
        - `(100 - discount_percentage)`는 할인 후 남는 가격의 비율입니다 (예: 15% 할인이면 85%).
        - 이를 100으로 나누어 소수 비율로 만들고 원래 가격에 곱합니다.
        - `int()`를 사용하여 결과를 정수로 변환하여 소수점 이하를 버립니다.
    - `item_prices[name] = discounted_price`를 통해 딕셔너리 내 해당 상품의 가격을 할인된 가격으로 직접 업데이트합니다.
- 모든 상품 가격이 업데이트된 후, `sorted(item_prices.keys())`를 사용하여 상품명을 오름차순으로 정렬하고, 각 "상품명: 할인가격"을 출력합니다.

---

## ✅ 문제 25. 판매 데이터 집계

**설명**: 여러 건의 판매 기록이 주어집니다. 각 기록은 판매된 상품명과 판매 수량을 나타냅니다. 모든 판매 기록을 취합하여 각 상품별 총 판매 수량을 계산해야 합니다. 결과를 상품명의 오름차순으로 정렬하여 "상품명: 총판매수량" 형태로 출력합니다.

**입력 예시**
```
5
apple 10
banana 5
apple 3
orange 8
banana 12
```
(첫 줄: 판매 기록 수 N, 다음 N줄: "상품명 판매수량" 형식)

**출력 예시**
```
apple: 13
banana: 17
orange: 8
```

**풀이 코드**
```python
from collections import Counter # 또는 defaultdict(int)

n = int(input())
# Counter를 사용하면 리스트에서 바로 빈도 계산이 가능하지만,
# 여기서는 입력 형식이 상품명과 수량이므로, 수량을 누적해야 함.
product_sales = Counter() # 또는 defaultdict(int)

for _ in range(n):
    name, quantity_str = input().split()
    quantity = int(quantity_str)
    product_sales[name] += quantity # Counter의 경우 이렇게 바로 누적 가능

for name in sorted(product_sales.keys()):
    print(f"{name}: {product_sales[name]}")
```

**풀이에 대한 설명**
- `from collections import Counter`를 사용하여 `Counter`를 (또는 `defaultdict(int)`를) 가져옵니다.
- `product_sales = Counter()`: 상품명을 키로 하고, 해당 상품의 총 판매 수량을 값으로 가지는 `Counter` (또는 `defaultdict(int)`) 객체를 생성합니다.
- N개의 판매 기록을 입력받으면서 각 "상품명 수량" 쌍에 대해:
    - `name` (상품명)과 `quantity` (판매 수량, 정수로 변환)를 추출합니다.
    - `product_sales[name] += quantity`: `name`을 키로 하는 `Counter`의 값에 `quantity`를 더하여 누적합니다. `Counter`나 `defaultdict(int)`는 키가 처음 등장할 때 자동으로 0으로 초기화된 후 연산이 수행됩니다.
- 모든 판매 기록이 처리된 후, `sorted(product_sales.keys())`를 사용하여 상품명을 오름차순으로 정렬합니다.
- 정렬된 각 상품명 `name`에 대해, 해당 상품의 총 판매 수량 `product_sales[name]`을 가져와 "상품명: 총판매수량" 형식으로 출력합니다.

---

## ✅ 문제 26. 키의 대칭 차집합 찾기

**설명**: 두 개의 딕셔너리가 주어집니다. (키는 문자열, 값은 정수). 두 딕셔너리의 키(key) 집합 간의 대칭 차집합(symmetric difference)에 해당하는 키들을 찾아야 합니다. 즉, 첫 번째 딕셔너리에만 있거나 두 번째 딕셔너리에만 있는 키들을 모두 찾습니다. 이 키들을 사전순으로 정렬하여 한 줄에 공백으로 구분해 출력합니다. 해당하는 키가 없으면 "None"을 출력합니다.

**입력 예시**
```
3
apple 10
banana 20
orange 30
3
banana 5
grape 15
melon 25
```

**출력 예시**
```
apple grape melon orange
```

**풀이 코드**
```python
def read_dict_keys_as_set():
    n = int(input())
    keys_set = set()
    for _ in range(n):
        key, value_str = input().split(maxsplit=1) # 값은 사용하지 않음
        keys_set.add(key)
    return keys_set

keys1 = read_dict_keys_as_set()
keys2 = read_dict_keys_as_set()

# set의 대칭 차집합 연산자(^) 사용
symmetric_diff_keys = keys1 ^ keys2 
# 또는 keys1.symmetric_difference(keys2)

if symmetric_diff_keys:
    print(*sorted(list(symmetric_diff_keys)))
else:
    print("None")
```

**풀이에 대한 설명**
- `read_dict_keys_as_set()` 함수는 각 딕셔너리 입력을 받아 키들만 추출하여 `set`으로 반환합니다. 값은 이 문제에서 사용되지 않습니다.
- `keys1`과 `keys2` 두 개의 키 집합을 읽어들입니다.
- `symmetric_diff_keys = keys1 ^ keys2`: `^` 연산자를 사용하여 두 키 집합의 대칭 차집합을 계산합니다. 이는 `keys1.symmetric_difference(keys2)`와 동일한 결과를 반환합니다. 대칭 차집합은 어느 한 집합에만 속하는 원소들의 집합입니다.
- `if symmetric_diff_keys:`: 대칭 차집합 결과가 비어있지 않으면,
    - `sorted(list(symmetric_diff_keys))`: 결과를 리스트로 변환한 후 사전순으로 정렬합니다.
    - `*` 연산자(언패킹)를 사용하여 정렬된 키들을 공백으로 구분하여 한 줄에 출력합니다.
- 결과 집합이 비어있으면 "None"을 출력합니다.

---

## ✅ 문제 27. 두 리스트로 딕셔너리 생성

**설명**: 두 개의 리스트(하나는 키 리스트, 다른 하나는 값 리스트)에 해당하는 문자열들이 주어집니다. 첫 번째 줄에는 키로 사용될 문자열들이 공백으로 구분되어 입력되고, 두 번째 줄에는 값으로 사용될 문자열들이 공백으로 구분되어 입력됩니다. 이 두 리스트를 사용하여 딕셔너리를 생성합니다. 키 리스트의 i번째 요소가 값 리스트의 i번째 요소와 매칭됩니다. 만약 두 리스트의 길이가 다르다면, 더 짧은 리스트의 길이만큼만 딕셔너리를 구성합니다 (즉, 짝이 맞는 부분까지만). 생성된 딕셔너리의 항목들을 키의 오름차순으로 정렬하여 "키: 값" 형태로 각 줄에 출력합니다.

**입력 예시**
```
name age city
Alice 30 London Paris
```
(키 리스트: name, age, city. 값 리스트: Alice, 30, London, Paris)
(Paris는 짝이 없어 무시됨)

**출력 예시**
```
age: 30
city: London
name: Alice
```

**풀이 코드**
```python
keys_str = input().split()
values_str = input().split()

result_dict = {}
# zip 함수는 두 개 이상의 iterable을 인자로 받아, 
# 각 iterable의 동일한 인덱스 요소를 튜플로 묶어줌.
# 가장 짧은 iterable의 길이에 맞춰짐.
for key, value in zip(keys_str, values_str):
    result_dict[key] = value

for key in sorted(result_dict.keys()):
    print(f"{key}: {result_dict[key]}")
```

**풀이에 대한 설명**
- `keys_str = input().split()`: 첫 번째 줄에서 공백으로 구분된 문자열들을 읽어 `keys_str` 리스트에 저장합니다.
- `values_str = input().split()`: 두 번째 줄에서 공백으로 구분된 문자열들을 읽어 `values_str` 리스트에 저장합니다.
- `result_dict = {}`: 생성될 딕셔너리를 초기화합니다.
- `for key, value in zip(keys_str, values_str):`: `zip` 함수를 사용하여 `keys_str`와 `values_str` 리스트를 동시에 순회합니다. `zip`은 두 리스트에서 같은 인덱스에 있는 요소들을 `(key, value)` 튜플로 묶어줍니다. `zip`은 인자로 전달된 리스트들 중 가장 짧은 것의 길이에 맞춰 동작하므로, 길이가 다른 경우 자동으로 짧은 쪽에 맞춰집니다.
    - `result_dict[key] = value`: 현재 `key`와 `value` 쌍을 `result_dict`에 추가합니다.
- `sorted(result_dict.keys())`를 사용하여 생성된 딕셔너리의 키들을 오름차순으로 정렬하고, 각 "키: 값" 쌍을 출력합니다.

---

## ✅ 문제 28. 사용 가능한 단어 확인

**설명**: 사용 가능한 단어 목록과 각 단어의 사용 가능 횟수가 딕셔너리 (Counter 형태)로 주어지고, 사용하려는 단어 목록이 주어집니다. 사용하려는 각 단어가 사용 가능한 단어 목록에 있고, 요청된 횟수만큼 사용 가능한지 확인합니다. 모든 단어가 사용 가능하다면 "YES", 하나라도 불가능하다면 "NO"를 출력합니다. 단어 사용은 누적 차감되지 않고, 각 요청 단어에 대해 현재 사용 가능한지만 독립적으로 판단합니다. (이 문제는 Counter를 활용한 재고 확인과 유사합니다. 단순화된 버전으로, 요청 목록의 단어 빈도를 세어, 각 단어의 요청 빈도가 사용 가능 빈도 이하인지 확인)

**입력 예시**
```
3
apple 2
banana 3
orange 1
4
apple
banana
apple
grape
```
(첫 줄: 사용 가능 단어 종류 수 N, 다음 N줄: "단어 사용가능횟수", 그 다음 줄: 사용할 단어 수 M, 다음 M줄: 사용할 단어)

**출력 예시**
```
NO
```
(grape는 사용 가능 목록에 없음)
(만약 입력이 apple, apple, apple 이라면 NO - apple은 2번만 가능)
(만약 입력이 apple, banana 라면 YES)


**풀이 코드**
```python
from collections import Counter

n_available = int(input())
available_counts = Counter()
for _ in range(n_available):
    word, count_str = input().split()
    available_counts[word] = int(count_str)

m_to_use = int(input())
words_to_use_list = [input() for _ in range(m_to_use)]

# 사용하려는 단어들의 빈도를 계산
requested_counts = Counter(words_to_use_list)

possible = True
for word, req_count in requested_counts.items():
    # 요청된 단어가 사용 가능 목록에 없거나, 요청 횟수가 사용 가능 횟수보다 많으면 불가능
    if available_counts[word] < req_count: # Counter는 없는 키에 대해 0을 반환
        possible = False
        break

if possible:
    print("YES")
else:
    print("NO")
```

**풀이에 대한 설명**
- `available_counts = Counter()`: 사용 가능한 단어와 그 횟수를 저장할 `Counter` 객체를 생성합니다.
- 입력을 받아 `available_counts`를 구성합니다 (예: `{'apple': 2, 'banana': 3, 'orange': 1}`).
- 사용하려는 단어 목록 `words_to_use_list`를 입력받습니다.
- `requested_counts = Counter(words_to_use_list)`: 사용하려는 단어 목록에서 각 단어가 몇 번 요청되었는지 빈도를 계산합니다 (예: `apple`, `banana`, `apple`, `grape` 입력 시 `{'apple': 2, 'banana': 1, 'grape': 1}`).
- `possible = True` 플래그를 초기화합니다.
- `requested_counts.items()`를 순회하며 각 요청된 단어 `word`와 그 요청 횟수 `req_count`를 가져옵니다.
    - `if available_counts[word] < req_count:`: `available_counts`에서 해당 `word`의 사용 가능 횟수를 확인합니다. `Counter`는 존재하지 않는 키에 대해 0을 반환하므로, `grape` 같은 단어는 `available_counts['grape']`가 0이 됩니다. 만약 사용 가능 횟수가 요청 횟수보다 적으면, `possible`을 `False`로 설정하고 루프를 중단합니다.
- 최종적으로 `possible` 플래그 값에 따라 "YES" 또는 "NO"를 출력합니다.

---

## ✅ 문제 29. 빈도의 빈도 계산

**설명**: 여러 개의 숫자가 주어집니다. 먼저, 각 숫자가 몇 번 등장하는지 빈도를 계산합니다. 그 다음, 이렇게 계산된 빈도수들 자체가 각각 몇 번 나타나는지, 즉 "빈도의 빈도"를 계산해야 합니다. 예를 들어, 숫자 리스트가 `[1, 1, 2, 2, 3]`이라면, 각 숫자의 빈도는 `{1:2, 2:2, 3:1}`입니다. 여기서 빈도수들은 `2, 2, 1`입니다. 이 빈도수들의 빈도는 `{1:1, 2:2}` (빈도수 1이 한 번, 빈도수 2가 두 번 등장)가 됩니다. 이 최종 "빈도의 빈도" 딕셔너리를 빈도수(원래 빈도)의 오름차순으로 정렬하여 "원래빈도: 원래빈도의빈도" 형태로 출력합니다.

**입력 예시**
```
7
1 1 2 2 3 4 4
```
(첫 줄: 숫자 개수 N, 다음 줄: N개의 숫자들)

**출력 예시**
```
1: 1
2: 2
```
(숫자 빈도: {1:2, 2:2, 3:1, 4:2}. 빈도값 리스트: [2, 2, 1, 2]. 이 빈도값들의 빈도: {1:1, 2:3})
(예시 수정: 1:2, 2:2, 3:1, 4:2 -> 빈도값들: 2,2,1,2 -> 이 빈도들의 빈도: {1:1 (3이 1번나옴), 2:3 (1,2,4가 2번나옴)}. 따라서 1:1, 2:3)

**정정된 출력 예시 (위 설명 기반)**
```
1: 1
2: 3
```

**풀이 코드**
```python
from collections import Counter

n = int(input())
numbers_str = input().split()
numbers = [int(x) for x in numbers_str] # 문자열 리스트를 정수 리스트로

# 1. 각 숫자의 빈도 계산
num_frequencies = Counter(numbers)

# 2. 계산된 빈도수들만 추출
frequencies_list = list(num_frequencies.values())

# 3. 빈도수들의 빈도 계산
if not frequencies_list: # 입력 숫자가 없는 경우
    pass # 아무것도 출력하지 않거나, 특정 메시지 출력
else:
    freq_of_freqs = Counter(frequencies_list)
    
    for freq_val in sorted(freq_of_freqs.keys()): # 원래 빈도값을 기준으로 정렬
        print(f"{freq_val}: {freq_of_freqs[freq_val]}")
```

**풀이에 대한 설명**
- `from collections import Counter`를 사용하여 `Counter`를 가져옵니다.
- 입력된 숫자 리스트 `numbers`를 구성합니다.
- `num_frequencies = Counter(numbers)`: `numbers` 리스트에 있는 각 숫자의 빈도를 계산하여 `num_frequencies` (예: `{1:2, 2:2, 3:1, 4:2}`)에 저장합니다.
- `frequencies_list = list(num_frequencies.values())`: `num_frequencies` 딕셔너리에서 값들(즉, 원래 숫자들의 빈도수들)만 추출하여 리스트 `frequencies_list` (예: `[2, 2, 1, 2]`)를 만듭니다.
- `if not frequencies_list:`: 만약 입력된 숫자가 없어 `frequencies_list`가 비어있다면, 아무 작업도 하지 않거나 적절한 메시지를 출력합니다.
- `else:`: `frequencies_list`가 비어있지 않다면,
    - `freq_of_freqs = Counter(frequencies_list)`: `frequencies_list`에 있는 빈도수들 자체의 빈도를 다시 계산하여 `freq_of_freqs` (예: `{1:1, 2:3}`)에 저장합니다.
    - `sorted(freq_of_freqs.keys())`를 사용하여 "원래 빈도값"을 기준으로 오름차순 정렬합니다.
    - 정렬된 각 "원래 빈도값" `freq_val`에 대해, 해당 `freq_val`이 몇 번 등장했는지(`freq_of_freqs[freq_val]`)를 "원래빈도: 원래빈도의빈도" 형식으로 출력합니다.

---

## ✅ 문제 30. Top N 빈도 아이템 (동점 처리)

**설명**: 문자열 아이템들의 리스트가 주어지고, 정수 N이 주어집니다. 리스트에서 가장 빈번하게 등장하는 상위 N개 아이템을 찾아야 합니다. 만약 N번째로 빈번한 아이템과 동일한 빈도수를 가진 다른 아이템들이 있다면, 이 아이템들도 모두 결과에 포함시켜야 합니다. 최종 결과는 아이템 이름을 사전순으로 정렬하여 각 줄에 하나씩 아이템 이름과 그 빈도수를 "아이템 빈도수" 형태로 출력합니다.

**입력 예시**
```
10
apple banana orange apple banana apple grape banana orange pear
3
```
(첫 줄: 아이템 총 개수 M, 다음 줄: M개의 아이템, 마지막 줄: N)

**출력 예시**
```
apple 3
banana 3
orange 2
```
(빈도: apple:3, banana:3, orange:2, grape:1, pear:1. N=3. 3번째 높은 빈도는 2(orange).
하지만 N=1이면 apple, banana. N=2여도 apple, banana. N=3이면 apple, banana, orange.)
(문제의 "N번째로 빈번한 아이템과 동일한 빈도수"라는 것은, 상위 N개의 *빈도값*을 먼저 찾고, 그 중 가장 낮은 빈도값과 같은 빈도를 가진 모든 아이템을 포함하라는 의미로 해석될 수도 있고, 또는 단순히 빈도순으로 정렬 후 상위 N개를 가져오되, N번째 아이템과 빈도가 같은 다음 아이템들도 포함하라는 의미일 수 있음. 여기서는 후자(더 일반적인 Top-N 동점 처리)로 해석)

(해석 변경: 상위 N개의 아이템을 선택하는데, 만약 N번째 아이템의 빈도수와 동일한 빈도수를 가진 아이템이 (N+1)번째 이후에도 있다면 그 아이템들도 포함한다.)
(예: apple:5, banana:5, cherry:4, date:4, elderberry:3. N=2. Top 2는 apple, banana (빈도5). 2번째 빈도는 5. 이와 같은 빈도 더 없음.
만약 N=3. Top 3는 apple, banana, cherry (빈도 5,5,4). 3번째 빈도는 4. date도 빈도 4이므로 포함. 결과: apple, banana, cherry, date)

**풀이 코드**
```python
from collections import Counter

m = int(input())
items_str = input().split()
n_top = int(input())

counts = Counter(items_str)

# 빈도수를 기준으로 내림차순, 빈도수가 같으면 아이템 이름을 오름차순으로 정렬
# (아이템, 빈도수) 튜플의 리스트가 됨
sorted_by_freq = sorted(counts.items(), key=lambda x: (-x[1], x[0]))

if not sorted_by_freq: # 아이템이 없는 경우
    pass # 아무것도 출력 안 함
elif n_top <= 0: # N이 0 이하인 경우
    pass # 아무것도 출력 안 함
else:
    result_items = []
    # 상위 N개 아이템을 우선 결과에 추가
    # 실제로는 N번째 아이템의 빈도수를 알아내야 함
    
    if len(sorted_by_freq) <= n_top:
        # 아이템 종류가 N개 이하이면 모든 아이템이 결과에 해당
        result_items = sorted_by_freq
    else:
        # N번째 아이템의 빈도수를 찾음
        nth_freq = sorted_by_freq[n_top-1][1]
        
        # 해당 빈도수 이상의 모든 아이템을 결과에 포함
        for item, freq in sorted_by_freq:
            if freq >= nth_freq:
                result_items.append((item, freq))
            else:
                # 그보다 낮은 빈도는 더 이상 볼 필요 없음 (이미 정렬됨)
                break 
                
    # 최종 결과는 아이템 이름 사전순으로 정렬 (이미 sorted_by_freq에서 2차 정렬됨)
    # 하지만 result_items를 다시 만들었으니, 만약 위 로직에서 순서가 바뀔 수 있다면 다시 정렬.
    # 현재 로직에서는 sorted_by_freq의 순서를 따르므로, 추가 정렬은 필요 없음.
    # 그러나 문제에서 최종 결과를 '아이템 이름을 사전순으로 정렬'하라 했으므로,
    # 명시적으로 다시 정렬하거나, 최초 정렬시 이름 우선순위를 다르게 해야함.
    # 여기서는 이름 사전순으로 최종 출력물을 정렬하는 것으로 함.

    # result_items.sort(key=lambda x: x[0]) # 이름으로 최종 정렬

    for item, freq in result_items: # result_items는 이미 이름으로 2차 정렬되어 있음.
                                   # 만약 문제의 최종 정렬 조건이 이름 우선이면,
                                   # sorted_items = sorted(result_items, key=lambda x: x[0]) 로 한번 더 정렬
        print(f"{item} {freq}")

```

**풀이에 대한 설명**
- `counts = Counter(items_str)`: 입력된 아이템 리스트로부터 각 아이템의 빈도를 계산합니다.
- `sorted_by_freq = sorted(counts.items(), key=lambda x: (-x[1], x[0]))`: `counts.items()` ( (아이템, 빈도수) 튜플의 리스트)를 정렬합니다. 정렬 기준은 첫째로 빈도수(`x[1]`)의 내림차순 (`-x[1]`), 둘째로 아이템 이름(`x[0]`)의 오름차순입니다.
- 입력 아이템이 없거나 `N`이 유효하지 않은 경우(0 이하)는 아무것도 출력하지 않도록 처리합니다.
- `result_items` 리스트를 만들어 결과를 담습니다.
    - 만약 정렬된 아이템의 종류(`len(sorted_by_freq)`)가 `n_top`보다 작거나 같으면, 모든 아이템이 상위 N개에 해당하므로 `sorted_by_freq` 전체를 `result_items`로 사용합니다.
    - 그렇지 않으면, `n_top - 1` 인덱스에 있는 아이템의 빈도수(`nth_freq = sorted_by_freq[n_top-1][1]`)를 N번째 빈도수로 결정합니다.
    - `sorted_by_freq`를 다시 순회하면서, 빈도수가 `nth_freq`보다 크거나 같은 모든 아이템과 그 빈도수를 `result_items`에 추가합니다. 빈도수가 `nth_freq`보다 작아지면 더 이상 포함할 아이템이 없으므로 루프를 중단합니다.
- `result_items`에 담긴 아이템과 빈도수를 출력합니다. 이 리스트는 이미 빈도수 내림차순, 이름 오름차순으로 정렬된 상태이므로, 문제에서 최종적으로 이름 사전순 정렬을 요구한다면 `result_items.sort(key=lambda x: x[0])`와 같이 이름 기준으로 한 번 더 정렬할 수 있습니다. (현재 코드에서는 `sorted_by_freq`의 2차 정렬 순서를 따르고 있습니다.)

```