# 🔄 deque(덱) 문법 드릴 문제집

Python `collections.deque`의 다양한 연산을 반복 숙달하기 위한 **10문제**입니다.
각 문제는 입력·출력 형식이 짧고 명확하며, 핵심 개념 사용에 집중합니다.

---

## ✅ 문제 1. 덱 기본 연산

**설명**
여러 개의 명령어가 주어집니다. 각 명령어에 따라 `deque`를 조작하고, 특정 명령어에 대해 결과를 출력해야 합니다.
- `push_front X`: 정수 X를 덱의 왼쪽에 추가합니다.
- `push_back X`: 정수 X를 덱의 오른쪽에 추가합니다.
- `pop_front`: 덱의 왼쪽에서 원소를 제거하고 그 수를 출력합니다. 덱이 비어있으면 -1을 출력합니다.
- `pop_back`: 덱의 오른쪽에서 원소를 제거하고 그 수를 출력합니다. 덱이 비어있으면 -1을 출력합니다.
- `size`: 덱의 크기를 출력합니다.
- `empty`: 덱이 비어있으면 1, 아니면 0을 출력합니다.
- `front`: 덱의 가장 왼쪽 원소를 출력합니다. 덱이 비어있으면 -1을 출력합니다.
- `back`: 덱의 가장 오른쪽 원소를 출력합니다. 덱이 비어있으면 -1을 출력합니다.

첫 줄에는 명령어의 수 N이 주어집니다. 이후 N개의 줄에 걸쳐 명령어가 주어집니다.

**입력 예시**
```
8
push_front 1
push_back 2
front
back
size
empty
pop_front
pop_back
```

**출력 예시**
```
1
2
2
0
1
2
```

**풀이 코드**
```python
from collections import deque
import sys

n = int(sys.stdin.readline())
dq = deque()
results = []

for _ in range(n):
    command_line = sys.stdin.readline().split()
    cmd = command_line[0]

    if cmd == "push_front":
        dq.appendleft(int(command_line[1]))
    elif cmd == "push_back":
        dq.append(int(command_line[1]))
    elif cmd == "pop_front":
        if dq:
            results.append(str(dq.popleft()))
        else:
            results.append("-1")
    elif cmd == "pop_back":
        if dq:
            results.append(str(dq.pop()))
        else:
            results.append("-1")
    elif cmd == "size":
        results.append(str(len(dq)))
    elif cmd == "empty":
        results.append("0" if dq else "1")
    elif cmd == "front":
        if dq:
            results.append(str(dq[0]))
        else:
            results.append("-1")
    elif cmd == "back":
        if dq:
            results.append(str(dq[-1]))
        else:
            results.append("-1")

print("\n".join(results))
```

**풀이에 대한 설명**
- `from collections import deque`: `deque`를 사용하기 위해 `collections` 모듈에서 가져옵니다.
- `dq = deque()`: 빈 `deque`를 생성합니다.
- `dq.appendleft(X)`: 덱의 왼쪽에 원소 `X`를 추가합니다.
- `dq.append(X)`: 덱의 오른쪽에 원소 `X`를 추가합니다 (리스트의 `append`와 유사).
- `dq.popleft()`: 덱의 왼쪽에서 원소를 제거하고 반환합니다. 비어있으면 `IndexError`가 발생하므로 확인이 필요합니다.
- `dq.pop()`: 덱의 오른쪽에서 원소를 제거하고 반환합니다. 비어있으면 `IndexError`가 발생하므로 확인이 필요합니다.
- `len(dq)`: 덱의 크기를 반환합니다.
- `if dq:` 또는 `if not dq:`: 덱이 비어있는지 확인합니다. 비어있으면 `False`로 평가됩니다.
- `dq[0]`: 덱의 가장 왼쪽 원소에 접근합니다. 비어있으면 `IndexError`가 발생합니다.
- `dq[-1]`: 덱의 가장 오른쪽 원소에 접근합니다. 비어있으면 `IndexError`가 발생합니다.
- `sys.stdin.readline()`을 사용하여 입력을 빠르게 처리하고, 결과를 리스트에 모아 한 번에 출력하여 효율을 높입니다.

---

## ✅ 문제 2. 카드 놓기 (회전 및 추가)

**설명**
1부터 N까지의 카드가 순서대로 있습니다. 가장 위의 카드를 바닥에 버리고, 그 다음 가장 위의 카드를 가장 아래로 옮기는 작업을 카드가 하나 남을 때까지 반복합니다. 마지막에 남는 카드의 번호를 출력하세요.

첫 줄에는 정수 N (1 ≤ N ≤ 1000)이 주어집니다.

**입력 예시**
```
6
```

**출력 예시**
```
4
```

**풀이 코드**
```python
from collections import deque

n = int(input())
dq = deque(range(1, n + 1))

while len(dq) > 1:
    dq.popleft() # 가장 위의 카드 버리기
    if dq: # 카드가 남아있다면
        dq.append(dq.popleft()) # 그 다음 가장 위의 카드를 가장 아래로 옮기기

print(dq[0])
```

**풀이에 대한 설명**
- `dq = deque(range(1, n + 1))`: 1부터 N까지의 숫자로 초기화된 `deque`를 생성합니다.
- `while len(dq) > 1:`: 덱에 카드가 하나만 남을 때까지 반복합니다.
    - `dq.popleft()`: 가장 왼쪽(위)의 카드를 제거하여 버립니다.
    - `if dq:`: 카드를 버린 후에도 덱에 카드가 남아 있는지 확인합니다. (카드가 하나만 남은 상태에서 `popleft()` 후 비게 될 수 있음)
    - `dq.append(dq.popleft())`: 만약 카드가 남아있다면, 현재 가장 왼쪽(위)의 카드를 `popleft()`로 꺼내어 `append()`를 통해 덱의 오른쪽(아래)으로 옮깁니다.
- 반복이 끝나면 덱에는 카드 하나만 남게 되고, `dq[0]`으로 해당 카드의 번호를 출력합니다.

---

## ✅ 문제 3. 원형 큐 시뮬레이션

**설명**
크기가 K인 원형 큐가 있습니다. N개의 숫자가 순서대로 주어질 때, 각 숫자를 큐에 넣고, 큐가 가득 차면 가장 오래된 원소를 제거하고 그 값을 출력합니다. 모든 숫자를 처리한 후 큐에 남아있는 원소들을 왼쪽부터 순서대로 공백으로 구분하여 출력합니다. 큐가 가득 차지 않은 상태에서 숫자를 넣을 때는 아무것도 출력하지 않습니다.

첫 줄에는 정수 N과 K (1 ≤ K ≤ N ≤ 100)가 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
1 2 3 4 5 6 7
```

**출력 예시**
```
1
2
3
4
5
6 7
```

**풀이 코드**
```python
from collections import deque

n, k = map(int, input().split())
numbers = list(map(int, input().split()))
dq = deque(maxlen=k) # 최대 크기가 k인 deque 생성
removed_elements = []

for num in numbers:
    if len(dq) == k: # 큐가 가득 찼다면
        removed_elements.append(str(dq[0])) # 가장 오래된 원소 (왼쪽 끝)를 기록
    dq.append(num) # 새 원소 추가 (가득 찼다면 가장 왼쪽 원소가 자동으로 제거됨)

# 제거된 원소들 출력
for val in removed_elements:
    print(val)

# 최종 큐 상태 출력
print(*(list(dq)))
```

**풀이에 대한 설명**
- `dq = deque(maxlen=k)`: `deque` 생성 시 `maxlen` 인자를 사용하면 덱의 최대 크기를 고정할 수 있습니다. 이 크기를 초과하여 원소가 추가되면 반대쪽 끝에서 자동으로 원소가 제거됩니다. `append` 사용 시 왼쪽 끝에서, `appendleft` 사용 시 오른쪽 끝에서 제거됩니다.
- `for num in numbers:`: 입력된 각 숫자를 순회합니다.
    - `if len(dq) == k:`: 현재 덱의 크기가 `maxlen`인 `k`와 같다면, 다음에 `append`가 일어날 때 가장 왼쪽 원소가 제거될 것이므로, 그 원소 `dq[0]`를 `removed_elements` 리스트에 미리 기록합니다.
    - `dq.append(num)`: 숫자를 덱의 오른쪽에 추가합니다. 이때 덱의 크기가 `maxlen`을 초과하면 가장 왼쪽 원소가 자동으로 제거됩니다.
- 제거된 원소들을 순서대로 출력하고, 마지막으로 덱에 남아있는 원소들을 공백으로 구분하여 출력합니다.

---

## ✅ 문제 4. 덱 회전시키기

**설명**
N개의 정수가 저장된 덱이 있습니다. M번의 회전 명령이 주어집니다. 각 명령은 양수 또는 음수입니다. 양수 `R`은 덱을 오른쪽으로 `R`칸 회전시키는 것을 의미하고 (오른쪽 끝 요소가 왼쪽 끝으로 이동), 음수 `L`은 덱을 왼쪽으로 `abs(L)`칸 회전시키는 것을 의미합니다 (왼쪽 끝 요소가 오른쪽 끝으로 이동). 모든 회전 명령을 수행한 후 덱의 최종 상태를 왼쪽부터 순서대로 공백으로 구분하여 출력합니다.

첫 줄에는 정수 N (1 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.
셋째 줄에는 회전 명령의 수 M (1 ≤ M ≤ 100)이 주어집니다.
넷째 줄에는 M개의 회전값 (정수)이 공백으로 구분되어 주어집니다.

**입력 예시**
```
5
1 2 3 4 5
2
2 -1
```

**출력 예시**
```
5 1 2 3 4
```
(초기: [1,2,3,4,5] -> 2번 오른쪽 회전: [4,5,1,2,3] -> 1번 왼쪽 회전: [5,1,2,3,4])

**풀이 코드**
```python
from collections import deque

n = int(input())
initial_elements = list(map(int, input().split()))
dq = deque(initial_elements)

m = int(input())
rotations = list(map(int, input().split()))

for r_val in rotations:
    dq.rotate(r_val) # 양수면 오른쪽, 음수면 왼쪽으로 회전

print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(initial_elements)`: 초기 원소들로 `deque`를 생성합니다.
- `dq.rotate(n)`: `deque`의 `rotate()` 메소드는 덱을 회전시킵니다.
    - `n`이 양수이면, 덱을 오른쪽으로 `n`칸 회전시킵니다. 즉, 오른쪽 끝의 `n`개 요소가 왼쪽 끝으로 이동합니다.
    - `n`이 음수이면, 덱을 왼쪽으로 `abs(n)`칸 회전시킵니다. 즉, 왼쪽 끝의 `abs(n)`개 요소가 오른쪽 끝으로 이동합니다.
- 각 회전값 `r_val`에 대해 `dq.rotate(r_val)`을 수행하여 덱을 회전시킵니다.
- 모든 회전 후, `list(dq)`를 언패킹하여 최종 덱의 상태를 출력합니다.

---

## ✅ 문제 5. 양쪽 끝 번갈아 제거

**설명**
N개의 정수가 저장된 덱이 있습니다. 덱의 왼쪽 끝과 오른쪽 끝에서 번갈아가며 원소를 제거하여 순서대로 출력합니다. (왼쪽 먼저, 그 다음 오른쪽, 그 다음 왼쪽 순). 덱에 원소가 하나만 남으면 그 원소를 마지막으로 제거하여 출력합니다. 덱이 빌 때까지 반복합니다.

첫 줄에는 정수 N (1 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
5
10 20 30 40 50
```

**출력 예시**
```
10
50
20
40
30
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements = list(map(int, input().split()))
dq = deque(elements)
removed_order = []

left_turn = True # 왼쪽부터 제거 시작
while dq:
    if left_turn:
        removed_order.append(str(dq.popleft()))
    else:
        removed_order.append(str(dq.pop()))
    left_turn = not left_turn # 다음 차례 변경

print("\n".join(removed_order))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `left_turn = True`: 현재 왼쪽에서 제거할 차례인지 나타내는 플래그입니다.
- `while dq:`: 덱이 비어있지 않은 동안 반복합니다.
    - `if left_turn:`: 왼쪽에서 제거할 차례이면 `dq.popleft()`를 사용하여 왼쪽 원소를 제거하고 `removed_order`에 추가합니다.
    - `else:`: 오른쪽에서 제거할 차례이면 `dq.pop()`을 사용하여 오른쪽 원소를 제거하고 `removed_order`에 추가합니다.
    - `left_turn = not left_turn`: 다음 제거 차례를 반대로 변경합니다.
- 제거된 순서대로 저장된 `removed_order` 리스트의 원소들을 줄바꿈으로 구분하여 출력합니다.

---

## ✅ 문제 6. 덱 뒤집기

**설명**
N개의 정수가 저장된 덱이 주어집니다. 이 덱의 순서를 완전히 뒤집은 후, 원소들을 왼쪽부터 순서대로 공백으로 구분하여 출력하세요.

첫 줄에는 정수 N (1 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
5
1 2 3 4 5
```

**출력 예시**
```
5 4 3 2 1
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements = list(map(int, input().split()))
dq = deque(elements)

dq.reverse() # 덱 내부의 원소 순서를 뒤집음

print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `dq.reverse()`: `deque`의 `reverse()` 메소드는 덱 내부의 원소들의 순서를 그 자리에서(in-place) 뒤집습니다. 리스트의 `reverse()`와 유사하게 동작합니다.
- 순서가 뒤집힌 `dq`를 리스트로 변환한 후 언패킹하여 원소들을 공백으로 구분해 출력합니다.

---

## ✅ 문제 7. 특정 값 개수 세기

**설명**
N개의 정수가 저장된 덱과 찾고자 하는 특정 정수 X가 주어집니다. 덱 안에서 정수 X가 몇 번 등장하는지 그 횟수를 세어 출력하세요.

첫 줄에는 정수 N (1 ≤ N ≤ 100)과 찾을 정수 X가 공백으로 구분되어 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 덱에 저장될 형태로 주어집니다.

**입력 예시**
```
7 3
1 3 2 3 4 3 5
```

**출력 예시**
```
3
```

**풀이 코드**
```python
from collections import deque

n, x = map(int, input().split())
elements = list(map(int, input().split()))
dq = deque(elements)

count = dq.count(x) # 덱에서 x의 등장 횟수를 셈

print(count)
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `dq.count(value)`: `deque`의 `count()` 메소드는 덱 내에서 특정 `value`와 동일한 원소가 몇 개인지 그 횟수를 반환합니다. 리스트의 `count()`와 유사하게 동작합니다.
- `count = dq.count(x)`를 통해 정수 `x`의 등장 횟수를 계산하고 출력합니다.

---

## ✅ 문제 8. 덱 확장하기

**설명**
초기 덱이 주어지고, 이어서 다른 여러 정수들이 주어집니다. 이 정수들을 기존 덱의 오른쪽 끝에 순서대로 모두 추가(확장)한 후, 최종 덱의 상태를 왼쪽부터 순서대로 공백으로 구분하여 출력하세요.

첫 줄에는 초기 덱의 원소 개수 N (0 ≤ N ≤ 50)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 초기 덱의 원소로 주어집니다. (N=0이면 빈 줄)
셋째 줄에는 추가할 원소의 개수 M (0 ≤ M ≤ 50)이 주어집니다.
넷째 줄에는 M개의 정수가 공백으로 구분되어 추가될 원소로 주어집니다. (M=0이면 빈 줄)

**입력 예시**
```
3
1 2 3
4
4 5 6 7
```

**출력 예시**
```
1 2 3 4 5 6 7
```

**풀이 코드**
```python
from collections import deque

n = int(input())
initial_elements_str = input()
if n > 0:
    initial_elements = list(map(int, initial_elements_str.split()))
else:
    initial_elements = []
dq = deque(initial_elements)

m = int(input())
additional_elements_str = input()
if m > 0:
    additional_elements = list(map(int, additional_elements_str.split()))
else:
    additional_elements = []

dq.extend(additional_elements) # 다른 iterable의 원소들을 덱의 오른쪽에 추가

print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(initial_elements)`: 초기 원소들로 `deque`를 생성합니다. N=0일 경우 빈 덱이 생성됩니다.
- `dq.extend(iterable)`: `deque`의 `extend()` 메소드는 주어진 `iterable`(예: 리스트, 다른 덱 등)의 모든 원소를 현재 덱의 오른쪽 끝에 순서대로 추가합니다. 리스트의 `extend()`와 유사하게 동작합니다.
- `additional_elements` 리스트를 `dq.extend()`를 통해 기존 덱 `dq`에 추가합니다.
- 최종 덱의 상태를 출력합니다.

---

## ✅ 문제 9. 슬라이딩 윈도우 최솟값 (단순 구현)

**설명**
N개의 정수와 윈도우 크기 K가 주어집니다. 숫자 시퀀스를 왼쪽에서 오른쪽으로 크기 K의 윈도우를 이동시키면서, 각 윈도우에 포함된 숫자들 중 최솟값을 찾아 출력합니다. (이 문제는 `deque`를 직접 활용한 최적화 기법을 사용하는 것이 일반적이나, 여기서는 `deque`를 사용하여 윈도우를 구성하고 각 윈도우의 최솟값을 찾는 단순한 방식으로 접근합니다.)

첫 줄에는 정수 N과 K (1 ≤ K ≤ N ≤ 100)가 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
8 3
1 5 2 6 3 7 4 8
```

**출력 예시**
```
1
2
2
3
3
4
```
(윈도우: [1,5,2]->1, [5,2,6]->2, [2,6,3]->2, [6,3,7]->3, [3,7,4]->3, [7,4,8]->4)

**풀이 코드**
```python
from collections import deque

n, k = map(int, input().split())
numbers = list(map(int, input().split()))
results = []

# 초기 윈도우 구성 (deque 사용은 필수는 아니나, 슬라이딩 표현을 위해 사용 가능)
window = deque() 
for i in range(n):
    window.append(numbers[i])
    if len(window) > k: # 윈도우 크기 유지
        window.popleft()
    
    if len(window) == k: # 윈도우가 가득 찼을 때 최솟값 계산
        results.append(str(min(window)))

print("\n".join(results))
```

**풀이에 대한 설명**
- 이 풀이는 `deque`를 슬라이딩 윈도우 자체를 저장하는 용도로 사용합니다.
- `window = deque()`: 현재 윈도우를 나타낼 빈 `deque`를 생성합니다.
- `for i in range(n):`: 입력된 숫자들을 순회합니다.
    - `window.append(numbers[i])`: 현재 숫자를 윈도우(덱)의 오른쪽에 추가합니다.
    - `if len(window) > k:`: 윈도우에 숫자를 추가한 후 크기가 K를 초과하면, `window.popleft()`를 사용하여 가장 왼쪽(오래된) 숫자를 제거하여 윈도우 크기를 K로 유지합니다.
    - `if len(window) == k:`: 윈도우의 크기가 정확히 K가 되면 (즉, 유효한 윈도우가 구성되면), `min(window)`를 사용하여 현재 윈도우 내의 최솟값을 계산하고 `results` 리스트에 문자열 형태로 추가합니다.
- `results`에 저장된 각 윈도우의 최솟값들을 줄바꿈으로 구분하여 출력합니다.
- 참고: 실제로 슬라이딩 윈도우 최솟값/최댓값을 효율적으로 찾기 위해서는 `deque`를 이용한 단조 큐(monotonic queue) 알고리즘을 사용하지만, 이 문제에서는 `deque`의 기본 연산으로 윈도우를 구성하는 데 초점을 맞춥니다.

---

## ✅ 문제 10. 앞뒤로 번갈아 추가 후 특정 인덱스 조회

**설명**
N개의 명령어가 주어집니다. 각 명령어는 "L X" 또는 "R X" 형태입니다.
- "L X": 정수 X를 현재 덱의 가장 왼쪽에 추가합니다.
- "R X": 정수 X를 현재 덱의 가장 오른쪽에 추가합니다.
모든 명령어를 처리한 후, 덱의 M번째 원소 (0-indexed)를 출력합니다. M이 덱의 크기보다 크거나 같으면 -1을 출력합니다.

첫 줄에는 명령어의 수 N (1 ≤ N ≤ 100)과 조회할 인덱스 M (0 ≤ M < N)이 주어집니다.
다음 N개의 줄에 걸쳐 명령어가 주어집니다.

**입력 예시**
```
5 2
R 10
L 5
R 20
L 1
R 30
```

**출력 예시**
```
10
```
(덱 변화: [10] -> [5,10] -> [5,10,20] -> [1,5,10,20] -> [1,5,10,20,30]. 2번 인덱스는 10)

**풀이 코드**
```python
from collections import deque

n, m_idx = map(int, input().split())
dq = deque()

for _ in range(n):
    cmd, val_str = input().split()
    val = int(val_str)
    if cmd == "L":
        dq.appendleft(val)
    elif cmd == "R":
        dq.append(val)

if m_idx < len(dq):
    print(dq[m_idx])
else:
    print("-1")
```

**풀이에 대한 설명**
- `dq = deque()`: 빈 `deque`를 생성합니다.
- N개의 명령어를 순회하면서:
    - "L X" 명령어인 경우, `dq.appendleft(X)`를 사용하여 정수 X를 덱의 왼쪽에 추가합니다.
    - "R X" 명령어인 경우, `dq.append(X)`를 사용하여 정수 X를 덱의 오른쪽에 추가합니다.
- 모든 명령어를 처리한 후, `if m_idx < len(dq):` 조건을 확인하여 조회할 인덱스 `m_idx`가 현재 덱의 유효한 범위 내에 있는지 검사합니다.
    - 유효하다면 `dq[m_idx]`를 통해 해당 인덱스의 원소를 출력합니다. `deque`도 리스트처럼 인덱스로 접근 가능합니다.
    - 유효하지 않다면 (즉, M이 덱의 크기보다 크거나 같다면) -1을 출력합니다.


---

## ✅ 문제 11. 덱의 양 끝 값 교환

**설명**
N개의 정수가 저장된 덱이 있습니다. 만약 덱에 두 개 이상의 원소가 있다면, 가장 왼쪽 원소와 가장 오른쪽 원소의 위치를 서로 교환합니다. 이 작업을 수행한 후 덱의 최종 상태를 왼쪽부터 순서대로 공백으로 구분하여 출력합니다. 덱에 원소가 하나 이하이면 아무 작업도 하지 않고 원래 상태 그대로 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)

**입력 예시 1**
```
5
10 20 30 40 50
```

**출력 예시 1**
```
50 20 30 40 10
```

**입력 예시 2**
```
1
10
```

**출력 예시 2**
```
10
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
dq = deque(elements)

if len(dq) >= 2:
    # Python deque는 직접적인 양 끝 동시 할당(swap)을 지원하지 않음
    # 따라서 pop, appendleft/append를 사용하거나, 인덱스로 값을 교환
    # 또는 임시 변수 사용
    # dq[0], dq[-1] = dq[-1], dq[0] # 일반 리스트처럼 인덱스로 교환 가능
    
    # deque의 특성을 살린다면, pop & insert 방식 (덜 효율적일 수 있음)
    # temp_left = dq.popleft()
    # temp_right = dq.pop()
    # dq.appendleft(temp_right)
    # dq.append(temp_left)
    # 위 방식은 중간 원소가 있을 때 순서가 바뀔 수 있음.
    # 예를 들어 [1,2,3,4,5] -> popleft (1), pop (5) -> dq = [2,3,4]
    # -> appendleft(5) -> [5,2,3,4] -> append(1) -> [5,2,3,4,1]
    # 따라서 인덱스를 통한 직접 교환이 가장 명확함.
    
    left_val = dq[0]
    right_val = dq[-1]
    dq[0] = right_val
    dq[-1] = left_val
    
print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `if len(dq) >= 2:`: 덱에 원소가 2개 이상 있는지 확인합니다.
    - `left_val = dq[0]`: 가장 왼쪽 원소의 값을 임시 변수에 저장합니다.
    - `right_val = dq[-1]`: 가장 오른쪽 원소의 값을 임시 변수에 저장합니다.
    - `dq[0] = right_val`: 가장 왼쪽 원소의 위치에 원래 가장 오른쪽 원소의 값을 할당합니다.
    - `dq[-1] = left_val`: 가장 오른쪽 원소의 위치에 원래 가장 왼쪽 원소의 값을 할당합니다.
- `deque`는 인덱스를 통해 특정 위치의 원소 값을 직접 변경할 수 있습니다.
- 최종 덱의 상태를 공백으로 구분하여 출력합니다.

---

## ✅ 문제 12. 덱 중간에 원소 삽입

**설명**
N개의 정수가 저장된 덱이 있습니다. 정수 M (삽입할 위치, 0-indexed)과 삽입할 값 X가 주어집니다. 덱의 M번째 위치에 X를 삽입한 후, 최종 덱의 상태를 왼쪽부터 순서대로 공백으로 구분하여 출력합니다. M이 현재 덱의 크기보다 크면 가장 오른쪽에 추가합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)
셋째 줄에는 삽입할 위치 M과 삽입할 값 X가 공백으로 구분되어 주어집니다.

**입력 예시 1**
```
5
10 20 30 40 50
2 99
```

**출력 예시 1**
```
10 20 99 30 40 50
```

**입력 예시 2**
```
3
1 2 3
5 88
```

**출력 예시 2**
```
1 2 3 88
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
dq = deque(elements)

m, x = map(int, input().split())

# deque의 insert(index, value) 메소드 사용
if m >= len(dq): # 삽입 위치가 현재 크기 이상이면 맨 뒤에 추가
    dq.append(x)
else:
    dq.insert(m, x)

print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `m, x = map(int, input().split())`: 삽입할 위치 `m`과 값 `x`를 입력받습니다.
- `dq.insert(index, value)`: `deque`의 `insert()` 메소드는 지정된 `index` 위치에 `value`를 삽입합니다. 기존 `index` 위치부터의 원소들은 오른쪽으로 한 칸씩 밀려납니다.
- `if m >= len(dq):`: 만약 삽입할 위치 `m`이 현재 덱의 크기(`len(dq)`)보다 크거나 같으면, 이는 가장 오른쪽에 추가하는 것과 같으므로 `dq.append(x)`를 사용합니다.
- `else:`: 그렇지 않으면 (즉, `m`이 유효한 인덱스 범위 내이거나 0이라면), `dq.insert(m, x)`를 사용하여 해당 위치에 값을 삽입합니다.
- 최종 덱의 상태를 공백으로 구분하여 출력합니다.

---

## ✅ 문제 13. 덱에서 특정 원소 모두 제거

**설명**
N개의 정수가 저장된 덱과 제거할 값 X가 주어집니다. 덱에서 값 X와 일치하는 모든 원소를 제거한 후, 최종 덱의 상태를 왼쪽부터 순서대로 공백으로 구분하여 출력합니다. 만약 모든 원소가 제거되어 덱이 비게 되면 "EMPTY"를 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)
셋째 줄에는 제거할 값 X가 주어집니다.

**입력 예시**
```
7
10 20 30 20 40 20 50
20
```

**출력 예시**
```
10 30 40 50
```

**입력 예시 2**
```
3
10 10 10
10
```

**출력 예시 2**
```
EMPTY
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
dq = deque(elements)

val_to_remove = int(input())

# deque에서 특정 값을 모두 제거하는 직접적인 메소드는 없음
# 새로운 deque를 만들거나, remove를 반복 사용 (존재 여부 확인 필요)
new_dq = deque()
for item in dq:
    if item != val_to_remove:
        new_dq.append(item)
dq = new_dq # 원래 dq를 새 dq로 교체

# 또는 dq.remove(value)를 반복 사용하는 방법 (value가 없을 때 ValueError 발생)
# try:
#     while True:
#         dq.remove(val_to_remove)
# except ValueError:
#     pass # 더 이상 제거할 값이 없음

if not dq:
    print("EMPTY")
else:
    print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `val_to_remove = int(input())`: 제거할 값 `val_to_remove`를 입력받습니다.
- `deque`에는 특정 값을 가진 *모든* 원소를 한 번에 제거하는 직접적인 메소드가 없습니다. `dq.remove(value)`는 가장 먼저 발견되는 `value` 하나만 제거합니다.
- 따라서, 새로운 `deque`인 `new_dq`를 만듭니다.
- 기존 `dq`를 순회하면서 각 `item`에 대해, `item`이 `val_to_remove`와 다르면 `new_dq.append(item)`을 통해 `new_dq`에 추가합니다.
- 모든 원소 순회가 끝나면, `dq = new_dq`로 원래의 `dq`를 필터링된 `new_dq`로 대체합니다.
- 만약 최종 `dq`가 비어있으면 "EMPTY"를 출력하고, 그렇지 않으면 덱의 원소들을 공백으로 구분하여 출력합니다.
- (주석 처리된 `dq.remove()` 반복 사용 방법은 `ValueError` 예외 처리가 필요하며, 덱의 크기가 동적으로 변하므로 주의해야 합니다. 위 `new_dq` 방식이 더 안전하고 명확합니다.)

---

## ✅ 문제 14. 덱의 왼쪽 절반과 오른쪽 절반 교환

**설명**
N개의 정수가 저장된 덱이 있습니다. N은 항상 짝수라고 가정합니다. 덱을 정확히 반으로 나누어 왼쪽 절반의 원소들과 오른쪽 절반의 원소들의 순서를 통째로 바꿉니다. 예를 들어, [1,2,3,4,5,6] 이라면 [4,5,6,1,2,3] 으로 변경합니다. 변경 후 덱의 상태를 왼쪽부터 순서대로 공백으로 구분하여 출력합니다.

첫 줄에는 짝수 정수 N (0 ≤ N ≤ 100, N은 짝수)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)

**입력 예시**
```
6
10 20 30 70 80 90
```

**출력 예시**
```
70 80 90 10 20 30
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
dq = deque(elements)

if n > 0: # n이 0이 아닐 때만 (짝수이므로 n >= 2 부터 의미 있음)
    mid = n // 2
    
    # deque를 직접 슬라이싱하여 새 deque를 만들 수는 없음
    # rotate를 활용하거나, pop/append 반복으로 구현
    
    # 방법 1: rotate 활용
    # 오른쪽 절반을 앞으로 가져오려면, 왼쪽으로 mid 만큼 회전
    dq.rotate(-mid) # 왼쪽으로 mid 만큼 회전시키면 오른쪽 절반이 앞으로 옴
    
    # 방법 2: pop과 append를 활용 (새 deque 생성)
    # left_half = deque()
    # right_half = deque()
    # for _ in range(mid):
    #     left_half.append(dq.popleft())
    # for _ in range(mid): # 남은 것이 오른쪽 절반
    #     right_half.append(dq.popleft())
    # dq.extend(right_half)
    # dq.extend(left_half)

print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `if n > 0:`: 덱에 원소가 있을 때만 작업을 수행합니다. (N은 짝수이므로, 최소 2개부터 의미가 있습니다.)
    - `mid = n // 2`: 덱의 중간 지점을 계산합니다.
    - `dq.rotate(-mid)`: `deque`의 `rotate()` 메소드를 사용합니다. `-mid` 만큼 (즉, 왼쪽으로 `mid` 칸) 회전시키면, 원래 덱의 오른쪽 절반에 있던 원소들이 덱의 앞부분으로 오고, 원래 왼쪽 절반의 원소들이 뒷부분으로 가게 되어 효과적으로 두 절반이 교환됩니다.
- 최종 덱의 상태를 공백으로 구분하여 출력합니다.
- (주석 처리된 방법 2는 `popleft()`와 `extend()`를 사용하여 두 개의 임시 덱을 만들고 다시 합치는 방식입니다. `rotate()`가 더 간결하고 효율적입니다.)

---

## ✅ 문제 15. 덱의 모든 원소 합계 구하기

**설명**
N개의 정수가 저장된 덱이 주어집니다. 덱에 있는 모든 정수의 합계를 계산하여 출력하세요. 덱이 비어있으면 0을 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)

**입력 예시**
```
5
10 20 -5 15 30
```

**출력 예시**
```
70
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
dq = deque(elements)

# sum() 함수는 iterable의 모든 원소의 합을 구함
total_sum = sum(dq)

print(total_sum)
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- `total_sum = sum(dq)`: Python의 내장 함수 `sum()`은 `iterable` (리스트, 튜플, 덱 등)을 인자로 받아 그 안의 모든 숫자 원소들의 합계를 반환합니다. `deque`도 `iterable`이므로 `sum()` 함수를 직접 사용할 수 있습니다.
- 덱이 비어있다면 `dq`는 빈 `iterable`이 되고, `sum([])`은 0을 반환하므로 문제 조건에 맞습니다.
- 계산된 `total_sum`을 출력합니다.

---

## ✅ 문제 16. 덱의 처음과 끝 K개 원소 제거

**설명**
N개의 정수가 저장된 덱과 정수 K가 주어집니다. 덱의 가장 왼쪽에서 K개의 원소를 제거하고, 가장 오른쪽에서도 K개의 원소를 제거합니다. 만약 이 과정에서 덱의 크기보다 많은 원소를 제거하려고 하면 (예: 총 5개인데 양쪽에서 3개씩 제거), 가능한 만큼만 제거하고 덱은 비게 됩니다. 최종적으로 남은 덱의 원소들을 왼쪽부터 순서대로 공백으로 구분하여 출력합니다. 덱이 비게 되면 "EMPTY"를 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)과 K (0 ≤ K ≤ N)가 공백으로 구분되어 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)

**입력 예시**
```
10 3
1 2 3 4 5 6 7 8 9 10
```

**출력 예시**
```
4 5 6 7
```

**입력 예시 2**
```
5 3
1 2 3 4 5
```

**출력 예시 2**
```
EMPTY
```

**풀이 코드**
```python
from collections import deque

n, k = map(int, input().split())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
dq = deque(elements)

# 왼쪽에서 K개 제거
for _ in range(k):
    if dq: # 덱이 비어있지 않으면 제거
        dq.popleft()
    else: # 덱이 비면 중단
        break

# 오른쪽에서 K개 제거
for _ in range(k):
    if dq: # 덱이 비어있지 않으면 제거
        dq.pop()
    else: # 덱이 비면 중단
        break

if not dq:
    print("EMPTY")
else:
    print(*list(dq))
```

**풀이에 대한 설명**
- `dq = deque(elements)`: 초기 원소들로 `deque`를 생성합니다.
- 왼쪽에서 K개 제거:
    - `for _ in range(k):` 루프를 K번 반복합니다.
    - `if dq:`: 덱이 비어있는지 확인합니다. 비어있지 않으면 `dq.popleft()`를 호출하여 가장 왼쪽 원소를 제거합니다.
    - `else: break`: 만약 K번을 다 돌기 전에 덱이 비게 되면, 더 이상 제거할 수 없으므로 루프를 중단합니다.
- 오른쪽에서 K개 제거:
    - 유사하게 `for _ in range(k):` 루프를 K번 반복합니다.
    - `if dq:`: 덱이 비어있는지 확인합니다. 비어있지 않으면 `dq.pop()`를 호출하여 가장 오른쪽 원소를 제거합니다.
    - `else: break`: 만약 덱이 비게 되면 루프를 중단합니다.
- 최종적으로 `dq`가 비어있으면 "EMPTY"를 출력하고, 그렇지 않으면 남은 원소들을 공백으로 구분하여 출력합니다.

---

## ✅ 문제 17. 덱을 이용한 중복 제거 (순서 유지)

**설명**
N개의 정수가 순서대로 주어집니다. 이 숫자들에서 중복을 제거하되, 처음 등장한 순서를 유지하여 출력해야 합니다. `deque`와 `set`을 함께 활용하여 이를 구현하세요. 최종 결과를 공백으로 구분하여 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
8
10 20 10 30 20 20 40 10
```

**출력 예시**
```
10 20 30 40
```

**풀이 코드**
```python
from collections import deque

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []

# deque는 입력 순서를 그대로 가짐
input_dq = deque(elements) 
result_dq = deque()
seen_set = set()

for item in input_dq:
    if item not in seen_set:
        result_dq.append(item)
        seen_set.add(item)

print(*list(result_dq))
```

**풀이에 대한 설명**
- `input_dq = deque(elements)`: 입력된 모든 원소를 순서대로 `deque`에 저장합니다.
- `result_dq = deque()`: 중복이 제거되고 순서가 유지된 결과를 저장할 빈 `deque`를 생성합니다.
- `seen_set = set()`: 이미 등장한 원소들을 기록하기 위한 `set`을 생성합니다. `set`은 원소 탐색(존재 여부 확인)이 매우 빠릅니다.
- `for item in input_dq:`: 입력 `deque`의 각 원소를 순서대로 순회합니다.
    - `if item not in seen_set:`: 현재 `item`이 `seen_set`에 없다면 (즉, 이전에 등장한 적이 없다면),
        - `result_dq.append(item)`: `result_dq`에 현재 `item`을 추가합니다.
        - `seen_set.add(item)`: `seen_set`에 현재 `item`을 추가하여 다음 번에 중복 체크될 수 있도록 합니다.
- 최종적으로 `result_dq`에 저장된 (중복이 제거되고 순서가 유지된) 원소들을 공백으로 구분하여 출력합니다.

---

## ✅ 문제 18. 덱의 최대 길이 제한 후 넘치는 요소 기록

**설명**
덱의 최대 길이 K가 주어지고, N개의 정수가 순서대로 주어집니다. 각 정수를 덱의 오른쪽에 추가합니다. 만약 덱의 길이가 K를 초과하게 되면, 가장 왼쪽(오래된) 원소가 자동으로 제거됩니다. 이렇게 자동으로 제거되는 모든 원소들을 순서대로 기록하여 출력합니다. 모든 N개의 정수를 처리한 후, 마지막으로 기록된 제거 원소들을 각 줄에 하나씩 출력합니다. 제거된 원소가 없으면 아무것도 출력하지 않습니다.

첫 줄에는 정수 N (1 ≤ N ≤ 100)과 최대 길이 K (1 ≤ K ≤ N)가 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
1 2 3 4 5 6 7
```

**출력 예시**
```
1
2
3
4
```
(덱 상태 변화 및 제거:
1 추가 -> [1]
2 추가 -> [1,2]
3 추가 -> [1,2,3] (K=3, 가득 참)
4 추가 -> [2,3,4] (1 제거됨) -> 기록: 1
5 추가 -> [3,4,5] (2 제거됨) -> 기록: 2
6 추가 -> [4,5,6] (3 제거됨) -> 기록: 3
7 추가 -> [5,6,7] (4 제거됨) -> 기록: 4
)

**풀이 코드**
```python
from collections import deque

n, k = map(int, input().split())
numbers = list(map(int, input().split()))

dq = deque(maxlen=k) # maxlen을 설정하면 크기 초과 시 자동 제거
removed_log = []

for num in numbers:
    # maxlen이 설정된 deque에 append 시, 꽉 찼으면 반대쪽(왼쪽)에서 자동 pop
    # 그 pop 되는 값을 미리 알아내야 함.
    if len(dq) == k: # 현재 덱이 꽉 차 있다면, 다음 append 시 맨 왼쪽이 제거됨
        removed_log.append(str(dq[0])) # 제거될 원소를 미리 기록
    dq.append(num)

for entry in removed_log:
    print(entry)

```

**풀이에 대한 설명**
- `dq = deque(maxlen=k)`: 최대 길이가 `k`로 고정된 `deque`를 생성합니다. 이 덱에 원소를 `append()` (오른쪽 추가)할 때, 만약 덱의 길이가 `maxlen`에 도달한 상태라면, 가장 왼쪽(`popleft()` 위치) 원소가 자동으로 제거된 후 새 원소가 추가됩니다.
- `removed_log = []`: 자동으로 제거되는 원소들을 순서대로 기록할 리스트입니다.
- `for num in numbers:`: 입력된 각 숫자 `num`을 순회합니다.
    - `if len(dq) == k:`: 현재 `dq`의 길이가 `maxlen`인 `k`와 같다면 (즉, 덱이 꽉 차 있다면), 다음 `dq.append(num)`이 호출될 때 가장 왼쪽에 있는 원소 `dq[0]`가 제거될 것입니다. 이 제거될 원소를 `removed_log`에 미리 추가합니다.
    - `dq.append(num)`: 현재 숫자 `num`을 덱의 오른쪽에 추가합니다. 이때 덱이 꽉 차 있었다면 가장 왼쪽 원소가 자동으로 제거됩니다.
- 모든 숫자를 처리한 후, `removed_log`에 기록된 (자동으로 제거된) 원소들을 순서대로 각 줄에 하나씩 출력합니다.

---

## ✅ 문제 19. 덱을 두 개로 분리하기

**설명**
N개의 정수가 저장된 덱이 주어집니다. 이 덱을 인덱스 M (0-indexed)을 기준으로 두 개의 덱으로 분리합니다. 첫 번째 덱에는 원래 덱의 0번부터 M-1번 인덱스까지의 원소들이, 두 번째 덱에는 M번부터 마지막 인덱스까지의 원소들이 순서대로 포함됩니다. M이 0이면 첫 번째 덱은 비고, M이 N 이상이면 두 번째 덱이 비게 됩니다. 분리된 두 덱의 원소들을 각각 한 줄씩 (첫 번째 덱 먼저, 그 다음 두 번째 덱) 공백으로 구분하여 출력합니다. 빈 덱은 "EMPTY"로 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)과 분리 기준 인덱스 M (0 ≤ M ≤ N)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (N=0이면 빈 줄)

**입력 예시 1**
```
7 3
10 20 30 40 50 60 70
```

**출력 예시 1**
```
10 20 30
40 50 60 70
```

**입력 예시 2**
```
5 0
1 2 3 4 5
```

**출력 예시 2**
```
EMPTY
1 2 3 4 5
```

**입력 예시 3**
```
5 5
1 2 3 4 5
```

**출력 예시 3**
```
1 2 3 4 5
EMPTY
```

**풀이 코드**
```python
from collections import deque

n, m_split_idx = map(int, input().split())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []

original_dq = deque(elements)
dq1 = deque()
dq2 = deque()

# 분리 기준 인덱스 m_split_idx는 두 번째 덱의 시작 인덱스
# 첫 번째 덱은 0 ~ m_split_idx - 1
# 두 번째 덱은 m_split_idx ~ 끝

# 원본 덱을 순회하며 분리
for i in range(len(original_dq)):
    if i < m_split_idx:
        dq1.append(original_dq[i])
    else:
        dq2.append(original_dq[i])

# 또는 popleft를 사용해 분리 (원본 덱이 변경됨)
# for _ in range(m_split_idx):
#     if original_dq:
#         dq1.append(original_dq.popleft())
# dq2.extend(original_dq) # 남은 것들이 두 번째 덱


if not dq1:
    print("EMPTY")
else:
    print(*list(dq1))

if not dq2:
    print("EMPTY")
else:
    print(*list(dq2))
```

**풀이에 대한 설명**
- `original_dq = deque(elements)`: 초기 원소들로 원본 `deque`를 생성합니다.
- `dq1 = deque()`와 `dq2 = deque()`: 분리된 원소들을 저장할 두 개의 빈 `deque`를 생성합니다.
- 분리 기준 인덱스 `m_split_idx`를 사용하여 원본 `deque`의 원소들을 분배합니다.
    - `for i in range(len(original_dq)):`: 원본 `deque`의 인덱스를 기준으로 순회합니다.
        - `if i < m_split_idx:`: 현재 인덱스 `i`가 `m_split_idx`보다 작으면, 해당 원소 `original_dq[i]`를 `dq1`에 추가합니다.
        - `else:`: 그렇지 않으면 (즉, `i`가 `m_split_idx` 이상이면), `dq2`에 추가합니다.
- (주석 처리된 `popleft` 사용 방식은 원본 `deque`를 변경하면서 두 개의 덱을 구성하는 다른 방법입니다.)
- 각 분리된 `deque` (`dq1`, `dq2`)에 대해, 비어있으면 "EMPTY"를 출력하고, 그렇지 않으면 원소들을 공백으로 구분하여 출력합니다.

---

## ✅ 문제 20. 덱을 사용한 괄호 검사

**설명**
주어진 문자열에 포함된 괄호들 `( )`, `[ ]`, `{ }`의 짝이 올바르게 맞는지 검사합니다. 올바르면 "YES"를, 올바르지 않으면 "NO"를 출력합니다. `deque`를 스택처럼 활용하여 검사하세요.

첫 줄에는 검사할 문자열이 주어집니다. (문자열 길이는 1 이상 100 이하)

**입력 예시 1**
```
({[]})
```

**출력 예시 1**
```
YES
```

**입력 예시 2**
```
([)]
```

**출력 예시 2**
```
NO
```

**입력 예시 3**
```
(((())))
```

**출력 예시 3**
```
YES
```

**입력 예시 4**
```
())
```

**출력 예시 4**
```
NO
```

**풀이 코드**
```python
from collections import deque

s = input()
stack = deque() # deque를 스택으로 사용 (append와 pop으로 오른쪽 끝 사용)
mapping = {")": "(", "]": "[", "}": "{"}
is_balanced = True

for char_val in s:
    if char_val in "([{": # 여는 괄호면 스택에 추가
        stack.append(char_val)
    elif char_val in ")]}": # 닫는 괄호면
        if not stack: # 스택이 비어있는데 닫는 괄호가 나오면 불일치
            is_balanced = False
            break
        
        top_element = stack.pop()
        if mapping[char_val] != top_element: # 짝이 맞지 않으면 불일치
            is_balanced = False
            break
    # 괄호가 아닌 다른 문자는 무시 (문제 조건에 따라 다를 수 있음)

# 모든 문자 처리 후 스택에 여는 괄호가 남아있으면 불일치
if is_balanced and stack: 
    is_balanced = False

if is_balanced:
    print("YES")
else:
    print("NO")
```

**풀이에 대한 설명**
- `stack = deque()`: `deque`를 스택 용도로 생성합니다. `append()` (오른쪽 추가)를 `push` 연산으로, `pop()` (오른쪽 제거)를 `pop` 연산으로 사용합니다.
- `mapping = {")": "(", "]": "[", "}": "{"}`: 닫는 괄호에 대응하는 여는 괄호를 매핑하는 딕셔너리입니다.
- `is_balanced = True`: 괄호 짝이 맞는지 나타내는 플래그입니다.
- 입력 문자열 `s`의 각 문자 `char_val`을 순회합니다:
    - `if char_val in "([{"`: 현재 문자가 여는 괄호이면, `stack.append(char_val)`을 통해 스택에 추가합니다.
    - `elif char_val in ")]}"`: 현재 문자가 닫는 괄호이면,
        - `if not stack:`: 스택이 비어있는지 확인합니다. 비어있는데 닫는 괄호가 나오면 짝이 맞지 않으므로 `is_balanced = False`로 설정하고 루프를 중단합니다.
        - `top_element = stack.pop()`: 스택에서 가장 위에 있는 (가장 최근에 추가된 여는) 괄호를 꺼냅니다.
        - `if mapping[char_val] != top_element:`: 현재 닫는 괄호 `char_val`에 대응하는 여는 괄호 (`mapping[char_val]`)가 스택에서 꺼낸 `top_element`와 다르면 짝이 맞지 않으므로 `is_balanced = False`로 설정하고 루프를 중단합니다.
- 모든 문자를 처리한 후, `if is_balanced and stack:` 조건을 확인합니다. 만약 `is_balanced`가 여전히 `True`인데 스택에 여는 괄호가 남아있다면 (예: "((("), 이는 짝이 맞지 않는 것이므로 `is_balanced`를 `False`로 설정합니다.
- 최종 `is_balanced` 값에 따라 "YES" 또는 "NO"를 출력합니다.

