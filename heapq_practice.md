#  heapq (최소 힙) 문법 드릴 문제집

Python `heapq` 모듈의 다양한 함수를 반복 숙달하기 위한 **10문제**입니다.
각 문제는 입력·출력 형식이 짧고 명확하며, 핵심 개념 사용에 집중합니다.

---

## ✅ 문제 1. 최소 힙 기본 연산

**설명**
여러 개의 명령어가 주어집니다. 각 명령어에 따라 최소 힙을 조작하고, 특정 명령어에 대해 결과를 출력해야 합니다. 힙은 비어있는 리스트로 시작합니다.
- `push X`: 정수 X를 힙에 추가합니다.
- `pop`: 힙에서 가장 작은 원소를 제거하고 그 수를 출력합니다. 힙이 비어있으면 -1을 출력합니다.
- `top`: 힙에서 가장 작은 원소를 출력합니다 (제거하지 않음). 힙이 비어있으면 -1을 출력합니다.
- `size`: 힙의 크기를 출력합니다.
- `empty`: 힙이 비어있으면 1, 아니면 0을 출력합니다.

첫 줄에는 명령어의 수 N이 주어집니다. 이후 N개의 줄에 걸쳐 명령어가 주어집니다.

**입력 예시**
```
7
push 5
push 2
top
pop
size
push 1
pop
```

**출력 예시**
```
2
2
1
1
```

**풀이 코드**
```python
import heapq
import sys

n = int(sys.stdin.readline())
heap = [] # heapq는 리스트를 사용하여 힙을 구현
results = []

for _ in range(n):
    command_line = sys.stdin.readline().split()
    cmd = command_line[0]

    if cmd == "push":
        heapq.heappush(heap, int(command_line[1]))
    elif cmd == "pop":
        if heap:
            results.append(str(heapq.heappop(heap)))
        else:
            results.append("-1")
    elif cmd == "top":
        if heap:
            results.append(str(heap[0])) # 최소값은 항상 인덱스 0에 위치
        else:
            results.append("-1")
    elif cmd == "size":
        results.append(str(len(heap)))
    elif cmd == "empty":
        results.append("0" if heap else "1")

print("\n".join(results))
```

**풀이에 대한 설명**
- `import heapq`: `heapq` 모듈을 가져옵니다.
- `heap = []`: `heapq` 모듈은 일반 파이썬 리스트를 최소 힙으로 다룹니다.
- `heapq.heappush(heap, item)`: `item`을 `heap` 리스트에 최소 힙의 속성을 유지하면서 추가합니다.
- `heapq.heappop(heap)`: `heap`에서 가장 작은 항목을 제거하고 반환합니다. 힙이 비어있으면 `IndexError`가 발생하므로 확인이 필요합니다.
- `heap[0]`: 최소 힙에서 가장 작은 원소는 항상 리스트의 첫 번째 원소 (인덱스 0)입니다. 힙이 비어있으면 `IndexError`가 발생합니다.
- `len(heap)`: 힙의 크기 (원소의 개수)를 반환합니다.
- `if heap:` 또는 `if not heap:`: 힙(리스트)이 비어있는지 확인합니다.
- `sys.stdin.readline()`을 사용하여 입력을 빠르게 처리하고, 결과를 리스트에 모아 한 번에 출력하여 효율을 높입니다.

---

## ✅ 문제 2. 가장 작은 K개의 숫자 합

**설명**
N개의 정수가 주어지고, 정수 K가 주어집니다. N개의 정수 중 가장 작은 K개의 숫자를 찾아 그 합을 출력하세요. `heapq`를 사용하여 효율적으로 찾을 수 있습니다.

첫 줄에는 정수 N과 K (1 ≤ K ≤ N ≤ 1000)가 공백으로 구분되어 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
5 2 8 1 9 4 6
```

**출력 예시**
```
7
```
(가장 작은 3개 숫자: 1, 2, 4. 합: 1+2+4 = 7)

**풀이 코드**
```python
import heapq

n, k = map(int, input().split())
numbers = list(map(int, input().split()))

heapq.heapify(numbers) # 리스트를 그 자리에서 힙으로 변환

sum_of_smallest_k = 0
for _ in range(k):
    if numbers: # 힙이 비어있지 않다면
        sum_of_smallest_k += heapq.heappop(numbers)
    else: # K번 pop 하기 전에 힙이 비는 경우는 문제 조건상 없음 (K <= N)
        break 

print(sum_of_smallest_k)
```

**풀이에 대한 설명**
- `heapq.heapify(list)`: 주어진 리스트 `list`를 그 자리에서(in-place) 최소 힙으로 변환합니다. 이 연산은 O(N) 시간에 수행됩니다.
- `heapq.heapify(numbers)`를 통해 입력받은 숫자 리스트를 최소 힙으로 만듭니다.
- `sum_of_smallest_k = 0`: 합계를 저장할 변수를 초기화합니다.
- `for _ in range(k):`: K번 반복합니다.
    - `sum_of_smallest_k += heapq.heappop(numbers)`: 힙에서 가장 작은 원소를 꺼내어(`heappop`) `sum_of_smallest_k`에 더합니다. 이 과정을 K번 반복하면 가장 작은 K개의 숫자를 더하게 됩니다.
- 최종 합계를 출력합니다.

---

## ✅ 문제 3. 최대 힙처럼 사용하기 (부호 반전)

**설명**
N개의 명령어가 주어집니다. 각 명령어는 `push X` 또는 `pop` 입니다. `push X`는 정수 X를 "최대 힙"에 추가하는 것을 의미하고, `pop`은 "최대 힙"에서 가장 큰 원소를 제거하고 그 수를 출력하는 것을 의미합니다. 힙이 비어있는데 `pop`이 호출되면 -1을 출력합니다. `heapq`는 최소 힙만 지원하므로, 값을 저장할 때 부호를 반전시켜 최대 힙의 동작을 흉내 내세요.

첫 줄에는 명령어의 수 N이 주어집니다. 이후 N개의 줄에 걸쳐 명령어가 주어집니다.

**입력 예시**
```
6
push 5
push 2
push 7
pop
pop
pop
```

**출력 예시**
```
7
5
2
```

**풀이 코드**
```python
import heapq
import sys

n = int(sys.stdin.readline())
max_heap = [] # 실제로는 최소 힙이지만, 값의 부호를 바꿔 저장
results = []

for _ in range(n):
    command_line = sys.stdin.readline().split()
    cmd = command_line[0]

    if cmd == "push":
        # 값을 음수로 만들어 최소 힙에 넣으면, 절댓값이 큰 수가 더 작은 음수가 됨
        heapq.heappush(max_heap, -int(command_line[1])) 
    elif cmd == "pop":
        if max_heap:
            # 최소 힙에서 가장 작은 값(원래 값 기준 가장 큰 값의 음수)을 꺼내고 부호 반전
            results.append(str(-heapq.heappop(max_heap)))
        else:
            results.append("-1")

print("\n".join(results))
```

**풀이에 대한 설명**
- `heapq`는 최소 힙만 지원합니다. 최대 힙을 구현하려면 저장하는 값의 부호를 반전시키는 트릭을 사용합니다.
    - 원소를 추가할 때: `heapq.heappush(max_heap, -value)`와 같이 `-value`를 저장합니다. 이렇게 하면 원래 값이 클수록 저장된 음수 값은 작아지므로, 최소 힙의 루트에는 원래 값 기준으로 가장 큰 값의 음수 형태가 위치하게 됩니다.
    - 원소를 꺼낼 때: `value = -heapq.heappop(max_heap)`와 같이 최소 힙에서 가장 작은 값(음수)을 꺼낸 후 다시 부호를 반전시키면, 원래 값 기준으로 가장 큰 값을 얻을 수 있습니다.
- `max_heap = []`: 실제로는 최소 힙으로 동작할 리스트입니다.
- `push X` 명령어: `heapq.heappush(max_heap, -int(command_line[1]))`로 값을 음수로 변환하여 추가합니다.
- `pop` 명령어: `max_heap`이 비어있지 않으면 `-heapq.heappop(max_heap)`을 통해 가장 큰 값을 (원래 부호로) 꺼내어 결과에 추가합니다.

---

## ✅ 문제 4. K번째로 큰 수 찾기

**설명**
N개의 정수가 주어지고, 정수 K가 주어집니다. N개의 정수 중 K번째로 큰 수를 출력하세요. `heapq`를 사용하여 효율적으로 찾을 수 있습니다.

첫 줄에는 정수 N과 K (1 ≤ K ≤ N ≤ 1000)가 공백으로 구분되어 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
5 2 8 1 9 4 6
```

**출력 예시**
```
6
```
(숫자들: 1,2,4,5,6,8,9. 큰 순서대로: 9(1번째), 8(2번째), 6(3번째). 따라서 3번째 큰 수는 6)

**풀이 코드**
```python
import heapq

n, k = map(int, input().split())
numbers = list(map(int, input().split()))

# 방법 1: 모든 수를 (음수로 변환하여) 최소 힙에 넣고 K번 pop
# max_heap_like = []
# for num in numbers:
#     heapq.heappush(max_heap_like, -num)

# kth_largest = 0
# for _ in range(k):
#     if max_heap_like:
#         kth_largest = -heapq.heappop(max_heap_like)
#     else:
#         break # K번 pop 전에 비는 경우 (문제 조건상 없음)

# print(kth_largest)

# 방법 2: 크기가 K인 최소 힙 유지 (더 효율적일 수 있음)
# 처음 K개의 원소로 최소 힙을 만듭니다.
# 이후 나머지 원소들에 대해, 현재 원소가 힙의 루트(최소값)보다 크면
# 루트를 제거하고 현재 원소를 힙에 추가합니다.
# 모든 원소를 처리한 후 힙의 루트가 K번째 큰 수가 됩니다.

min_heap_k_size = []
for num in numbers:
    if len(min_heap_k_size) < k:
        heapq.heappush(min_heap_k_size, num)
    else:
        # 현재 숫자가 힙의 가장 작은 값(min_heap_k_size[0])보다 크면,
        # 힙의 가장 작은 값을 제거하고 현재 숫자를 추가
        # 이렇게 하면 힙에는 항상 지금까지 본 숫자 중 가장 큰 K개가 유지됨 (최소힙이므로 루트는 그중 가장 작은값)
        if num > min_heap_k_size[0]:
            heapq.heapreplace(min_heap_k_size, num) # heappop 후 heappush 하는 것과 유사

# 모든 숫자를 처리한 후, min_heap_k_size에는 상위 K개의 숫자가 들어있고,
# 그 중 가장 작은 값(루트)이 K번째로 큰 수가 됨.
print(min_heap_k_size[0])

```

**풀이에 대한 설명**
- **방법 2 (크기 K의 최소 힙 유지)**:
    - `min_heap_k_size = []`: 크기가 최대 K인 최소 힙을 유지할 리스트입니다.
    - 입력된 각 숫자 `num`에 대해:
        - `if len(min_heap_k_size) < k:`: 힙의 크기가 아직 K보다 작으면, `heapq.heappush(min_heap_k_size, num)`로 현재 숫자를 힙에 추가합니다.
        - `else:` (힙의 크기가 K일 때):
            - `if num > min_heap_k_size[0]:`: 현재 숫자 `num`이 힙의 가장 작은 값(`min_heap_k_size[0]`)보다 크다면, 이 `num`은 K번째 큰 수의 후보가 될 수 있습니다.
            - `heapq.heapreplace(min_heap_k_size, num)`: 힙에서 가장 작은 원소(루트)를 제거하고 새로운 원소 `num`을 추가합니다. 이 연산은 `heappop()` 후 `heappush()`하는 것보다 효율적입니다. 이 과정을 통해 힙은 항상 지금까지 처리한 숫자들 중 가장 큰 K개의 숫자를 (최소 힙 형태로) 유지하게 됩니다.
    - 모든 숫자를 처리하고 나면, `min_heap_k_size`의 루트(`min_heap_k_size[0]`)에는 K개의 가장 큰 숫자들 중 가장 작은 값, 즉 전체에서 K번째로 큰 수가 담겨있게 됩니다.
- (주석 처리된 방법 1은 모든 숫자를 음수로 변환하여 최소 힙에 넣고 K번 `pop`하여 K번째 큰 수를 찾는 방식입니다. 공간 복잡도가 더 클 수 있습니다.)

---

## ✅ 문제 5. 여러 리스트 병합 정렬 (힙 사용)

**설명**
K개의 정렬된 리스트가 주어집니다. 이 모든 리스트를 병합하여 하나의 정렬된 리스트로 만들고, 그 결과를 공백으로 구분하여 출력하세요. `heapq`를 사용하여 효율적으로 병합할 수 있습니다. 각 리스트는 (값, 리스트_인덱스, 현재_값_인덱스_리스트내) 형태의 튜플을 힙에 저장하여 관리합니다. (문제 단순화를 위해, 여기서는 여러 리스트의 첫 원소들로 힙을 구성하고, 가장 작은 값을 꺼낼 때마다 해당 값이 나온 리스트의 다음 원소를 힙에 추가하는 방식으로 진행합니다.)

첫 줄에는 리스트의 개수 K가 주어집니다.
다음 K개의 줄에는 각 리스트의 원소들이 공백으로 구분되어 주어집니다. (각 리스트는 이미 오름차순 정렬되어 있음)

**입력 예시**
```
3
1 4 7
2 5 8
3 6 9
```

**출력 예시**
```
1 2 3 4 5 6 7 8 9
```

**풀이 코드**
```python
import heapq

k = int(input())
lists = []
for _ in range(k):
    lists.append(list(map(int, input().split())))

min_heap = []
# 각 리스트의 첫 번째 원소를 (값, 리스트_인덱스, 값_리스트내_인덱스) 형태로 힙에 추가
for i in range(k):
    if lists[i]: # 리스트가 비어있지 않다면
        # (값, 리스트_인덱스, 다음_원소_인덱스)
        heapq.heappush(min_heap, (lists[i][0], i, 0)) 

result = []
while min_heap:
    value, list_idx, element_idx_in_list = heapq.heappop(min_heap)
    result.append(value)
    
    # 꺼낸 값이 속했던 리스트에서 다음 원소를 가져와 힙에 추가
    next_element_idx = element_idx_in_list + 1
    if next_element_idx < len(lists[list_idx]): # 해당 리스트에 다음 원소가 있다면
        heapq.heappush(min_heap, (lists[list_idx][next_element_idx], list_idx, next_element_idx))

print(*result)
```

**풀이에 대한 설명**
- 이 문제는 K-way merge 알고리즘을 `heapq`를 사용하여 구현하는 것입니다.
- `min_heap = []`: 최소 힙을 준비합니다. 힙에는 `(값, 원래_리스트_인덱스, 해당_리스트내_값_인덱스)` 형태의 튜플을 저장합니다. 값으로 정렬하기 위해 튜플의 첫 번째 요소를 값으로 둡니다.
- 초기화: 각 K개의 리스트에서 첫 번째 원소가 있다면, `(원소값, 리스트_인덱스, 0)` (0은 리스트 내 첫 원소의 인덱스) 튜플을 `min_heap`에 `heappush` 합니다.
- 반복:
    - `while min_heap:`: 힙이 비어있지 않은 동안 반복합니다.
    - `value, list_idx, element_idx_in_list = heapq.heappop(min_heap)`: 힙에서 가장 작은 값(`value`)을 가진 튜플을 꺼냅니다. 이 `value`는 병합된 결과 리스트에 추가될 다음 숫자입니다.
    - `result.append(value)`: 꺼낸 `value`를 최종 결과 리스트 `result`에 추가합니다.
    - `next_element_idx = element_idx_in_list + 1`: 방금 꺼낸 `value`가 속해있던 원래 리스트 (`lists[list_idx]`)에서 그 다음 원소의 인덱스를 계산합니다.
    - `if next_element_idx < len(lists[list_idx]):`: 만약 해당 리스트에 다음 원소가 존재한다면, 그 다음 원소의 정보 `(lists[list_idx][next_element_idx], list_idx, next_element_idx)`를 다시 `min_heap`에 `heappush` 합니다.
- 모든 과정이 끝나면 `result` 리스트에는 모든 원소가 정렬된 상태로 담겨있게 됩니다. 이를 공백으로 구분하여 출력합니다.

---

## ✅ 문제 6. 힙으로 만든 리스트의 특정 인덱스 값 변경 후 힙 속성 복원

**설명**
N개의 정수로 초기화된 최소 힙(리스트 형태)이 있습니다. 이 힙의 특정 인덱스 `idx`에 있는 값을 새로운 값 `new_val`로 변경합니다. 값 변경 후, 리스트가 최소 힙의 속성을 계속 유지하도록 복원해야 합니다. `heapq` 모듈에는 직접적인 "값 변경 후 복원" 함수가 없으므로, 가장 간단한 방법은 리스트 전체를 다시 `heapify` 하는 것입니다. (더 복잡한 방법으로는 sift-up 또는 sift-down 연산을 구현할 수 있으나, 여기서는 `heapify` 사용). 복원 후 힙의 루트 값을 출력하세요.

첫 줄에는 정수 N (1 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (이들은 이미 최소 힙 상태라고 가정하지 않아도 되며, `heapify`를 먼저 수행합니다)
셋째 줄에는 변경할 인덱스 `idx` (0 ≤ `idx` < N)와 새로운 값 `new_val`이 공백으로 구분되어 주어집니다.

**입력 예시**
```
5
3 5 1 7 9
2 0 
```
(초기 리스트 [3,5,1,7,9] -> heapify -> [1,5,3,7,9]. 인덱스 2의 값(3)을 0으로 변경 -> [1,5,0,7,9]. 다시 heapify -> [0,5,1,7,9]. 루트: 0)

**출력 예시**
```
0
```

**풀이 코드**
```python
import heapq

n = int(input())
elements = list(map(int, input().split()))
idx, new_val = map(int, input().split())

heapq.heapify(elements) # 먼저 초기 리스트를 힙으로 만듦

if 0 <= idx < n: # 유효한 인덱스인지 확인
    elements[idx] = new_val # 해당 인덱스의 값을 새 값으로 변경
    heapq.heapify(elements) # 값 변경 후 힙 속성을 복원하기 위해 다시 heapify
    
    if elements: # 힙이 비어있지 않다면
        print(elements[0]) # 복원된 힙의 루트 값 출력
    else:
        print("-1") # 힙이 비게 된 경우 (문제 조건상 이 경우는 드묾)
else:
    # 유효하지 않은 인덱스인 경우의 처리 (예: 원래 힙의 루트 출력 또는 에러 메시지)
    if elements:
        print(elements[0]) # 변경 없이 원래 (heapify된) 힙의 루트 출력
    else:
        print("-1")

```

**풀이에 대한 설명**
- `heapq.heapify(elements)`: 먼저 입력된 `elements` 리스트를 그 자리에서 최소 힙으로 변환합니다.
- `if 0 <= idx < n:`: 주어진 인덱스 `idx`가 유효한 범위 내에 있는지 확인합니다.
    - `elements[idx] = new_val`: 유효하다면, `elements` 리스트의 `idx` 위치에 있는 값을 `new_val`로 직접 변경합니다.
    - `heapq.heapify(elements)`: 리스트의 특정 원소 값이 변경되면 최소 힙의 속성이 깨질 수 있습니다. 가장 간단하게 힙 속성을 복원하는 방법은 리스트 전체에 대해 다시 `heapq.heapify()`를 호출하는 것입니다. 이 연산은 O(N)의 시간 복잡도를 가집니다.
    - (참고: 더 효율적인 방법은 변경된 값이 원래 값보다 작아졌는지 커졌는지에 따라 sift-up 또는 sift-down 연산을 해당 위치에서 수행하는 것이지만, `heapq` 모듈은 이러한 내부 연산을 직접 노출하지 않으므로 `heapify`가 간편한 대안입니다.)
- 복원된 힙이 비어있지 않다면, 힙의 루트 값 (`elements[0]`)을 출력합니다.

---

## ✅ 문제 7. N개의 가장 큰 항목 (nlargest)

**설명**
여러 개의 정수가 주어지고, 정수 N이 주어집니다. 이 정수들 중에서 가장 큰 N개의 항목을 찾아 리스트 형태로 출력합니다. 출력 순서는 큰 값부터 작은 값 순서(내림차순)입니다. `heapq.nlargest()` 함수를 사용하세요.

첫 줄에는 총 정수의 개수 M (1 ≤ N ≤ M ≤ 1000)과 N이 공백으로 구분되어 주어집니다.
둘째 줄에는 M개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
5 2 8 1 9 4 6
```

**출력 예시**
```
[9, 8, 6]
```

**풀이 코드**
```python
import heapq

m, n = map(int, input().split())
numbers = list(map(int, input().split()))

# numbers 리스트에서 가장 큰 n개의 항목을 찾아 리스트로 반환
largest_n_items = heapq.nlargest(n, numbers)

print(largest_n_items)
```

**풀이에 대한 설명**
- `heapq.nlargest(n, iterable, key=None)`: `iterable`에서 가장 큰 `n`개의 항목으로 이루어진 리스트를 반환합니다. 선택적 `key` 인자가 주어지면, 각 항목의 `key` 값을 기준으로 비교합니다.
- `largest_n_items = heapq.nlargest(n, numbers)`: `numbers` 리스트에서 가장 큰 `n`개의 숫자를 찾아 `largest_n_items` 리스트에 저장합니다. 이 리스트는 이미 큰 값부터 작은 값 순서(내림차순)로 정렬되어 반환됩니다.
- 결과를 그대로 출력합니다.

---

## ✅ 문제 8. N개의 가장 작은 항목 (nsmallest)

**설명**
여러 개의 정수가 주어지고, 정수 N이 주어집니다. 이 정수들 중에서 가장 작은 N개의 항목을 찾아 리스트 형태로 출력합니다. 출력 순서는 작은 값부터 큰 값 순서(오름차순)입니다. `heapq.nsmallest()` 함수를 사용하세요.

첫 줄에는 총 정수의 개수 M (1 ≤ N ≤ M ≤ 1000)과 N이 공백으로 구분되어 주어집니다.
둘째 줄에는 M개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
5 2 8 1 9 4 6
```

**출력 예시**
```
[1, 2, 4]
```

**풀이 코드**
```python
import heapq

m, n = map(int, input().split())
numbers = list(map(int, input().split()))

# numbers 리스트에서 가장 작은 n개의 항목을 찾아 리스트로 반환
smallest_n_items = heapq.nsmallest(n, numbers)

print(smallest_n_items)
```

**풀이에 대한 설명**
- `heapq.nsmallest(n, iterable, key=None)`: `iterable`에서 가장 작은 `n`개의 항목으로 이루어진 리스트를 반환합니다. 선택적 `key` 인자가 주어지면, 각 항목의 `key` 값을 기준으로 비교합니다.
- `smallest_n_items = heapq.nsmallest(n, numbers)`: `numbers` 리스트에서 가장 작은 `n`개의 숫자를 찾아 `smallest_n_items` 리스트에 저장합니다. 이 리스트는 이미 작은 값부터 큰 값 순서(오름차순)로 정렬되어 반환됩니다.
- 결과를 그대로 출력합니다.

---

## ✅ 문제 9. 힙에 원소 추가 후 즉시 가장 작은 값 제거 (heappushpop)

**설명**
초기 최소 힙과 새로운 정수 X가 주어집니다. 정수 X를 힙에 추가한 다음, 힙에서 가장 작은 원소를 제거하고 그 값을 출력합니다. 이 두 가지 작업을 하나의 연산으로 수행하는 `heapq.heappushpop()` 함수를 사용하세요. 만약 초기 힙이 비어있었다면, X는 추가된 후 바로 제거되므로 X가 출력됩니다.

첫 줄에는 초기 힙의 원소 개수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (이들은 이미 힙 상태라고 가정하지 않고, `heapify` 필요)
셋째 줄에는 추가할 정수 X가 주어집니다.

**입력 예시 1**
```
5
3 5 1 7 9
0
```
(힙 [1,5,3,7,9]에 0 push -> [0,1,3,7,9,5] -> 0 pop. 출력: 0)

**출력 예시 1**
```
0
```

**입력 예시 2**
```
5
3 5 1 7 9
4
```
(힙 [1,5,3,7,9]에 4 push -> [1,4,3,7,9,5] -> 1 pop. 출력: 1)

**출력 예시 2**
```
1
```

**풀이 코드**
```python
import heapq

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
heapq.heapify(elements) # 초기 리스트를 힙으로 만듦

new_item = int(input())

# 새 아이템을 힙에 push한 다음, 힙에서 가장 작은 아이템을 pop하여 반환
# 힙이 비어있다면, new_item을 push했다가 바로 pop하는 것과 같음
popped_item = heapq.heappushpop(elements, new_item)

print(popped_item)
# print("Final heap state:", elements) # 힙의 최종 상태 확인 (선택 사항)
```

**풀이에 대한 설명**
- `heapq.heapify(elements)`: 먼저 입력된 `elements` 리스트를 최소 힙으로 변환합니다.
- `new_item = int(input())`: 힙에 추가할 새 정수 `new_item`을 입력받습니다.
- `heapq.heappushpop(heap, item)`: 이 함수는 `item`을 `heap`에 먼저 `heappush`한 다음, `heap`에서 가장 작은 항목을 `heappop`하여 반환합니다. 이 두 연산은 `heappush()` 후 `heappop()` 하는 것보다 더 효율적으로 수행될 수 있습니다.
    - 만약 `item`이 힙의 기존 최소값보다 작거나 같다면, `item`이 푸시된 후 바로 팝되어 `item` 자신이 반환됩니다.
    - 만약 `item`이 힙의 기존 최소값보다 크다면, 기존 최소값이 팝되어 반환되고 `item`은 힙 내부에 자리잡게 됩니다.
- `popped_item = heapq.heappushpop(elements, new_item)`를 통해 연산을 수행하고, 반환된 (가장 작은) 값을 출력합니다.

---

## ✅ 문제 10. 힙에서 가장 작은 값 제거 후 새 원소 추가 (heapreplace)

**설명**
초기 최소 힙과 새로운 정수 X가 주어집니다. 만약 힙이 비어있지 않다면, 힙에서 가장 작은 원소를 먼저 제거하고 그 값을 출력한 후, 새로운 정수 X를 힙에 추가합니다. 이 두 가지 작업을 하나의 연산으로 수행하는 `heapq.heapreplace()` 함수를 사용하세요. 만약 초기 힙이 비어있다면 `heapreplace`는 `IndexError`를 발생시키므로, 이 경우 "-1"을 출력하고 X는 추가하지 않습니다. (또는 문제 조건을 단순화하여, 힙이 비어있으면 X를 그냥 추가하고 아무것도 출력하지 않도록 할 수도 있습니다. 여기서는 전자를 따릅니다.)

첫 줄에는 초기 힙의 원소 개수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. (이들은 이미 힙 상태라고 가정하지 않고, `heapify` 필요)
셋째 줄에는 추가할 정수 X가 주어집니다.

**입력 예시 1 (힙이 비어있지 않은 경우)**
```
5
3 5 1 7 9
10
```
(힙 [1,5,3,7,9] -> 1 pop (출력). 10 push -> [3,5,10,7,9]. 최종 힙은 [3,5,9,7,10] 등)

**출력 예시 1**
```
1
```

**입력 예시 2 (힙이 비어있는 경우)**
```
0

10
```

**출력 예시 2**
```
-1
```

**풀이 코드**
```python
import heapq

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = [] # N=0이면 빈 리스트
heapq.heapify(elements) # 초기 리스트를 힙으로 만듦

new_item = int(input())

if elements: # 힙이 비어있지 않은 경우에만 heapreplace 사용 가능
    # 힙에서 가장 작은 아이템을 pop하여 반환한 다음, 새 아이템을 push
    popped_item = heapq.heapreplace(elements, new_item)
    print(popped_item)
    # print("Final heap state:", elements) # 힙의 최종 상태 확인 (선택 사항)
else:
    # 힙이 비어있으면 heapreplace는 IndexError를 발생시킴
    print("-1")
    # 이 경우 new_item을 힙에 추가할지 여부는 문제 정의에 따라 다름
    # 여기서는 추가하지 않음. 만약 추가해야 한다면:
    # heapq.heappush(elements, new_item) 
    # print("Final heap state:", elements) 
```

**풀이에 대한 설명**
- `heapq.heapify(elements)`: 먼저 입력된 `elements` 리스트를 최소 힙으로 변환합니다.
- `new_item = int(input())`: 힙에 추가할 새 정수 `new_item`을 입력받습니다.
- `if elements:`: 힙(`elements` 리스트)이 비어있지 않은지 확인합니다.
    - `popped_item = heapq.heapreplace(heap, item)`: 힙이 비어있지 않을 때만 호출 가능합니다. 이 함수는 `heap`에서 가장 작은 항목을 먼저 `heappop`하여 반환한 다음, 새로운 `item`을 `heap`에 `heappush`합니다. 이 두 연산은 `heappop()` 후 `heappush()` 하는 것보다 더 효율적으로 수행될 수 있습니다.
    - 반환된 (원래 가장 작았던) `popped_item`을 출력합니다.
- `else:`: 만약 초기 힙이 비어있다면, `heapreplace`를 호출할 수 없으므로 (호출 시 `IndexError` 발생) "-1"을 출력합니다. 문제 조건에 따라 이 경우 `new_item`을 빈 힙에 추가할 수도 있지만, 현재 풀이에서는 추가하지 않고 "-1"만 출력합니다.



---

## ✅ 문제 11. 힙에서 특정 값 존재 여부 확인

**설명**
N개의 정수가 저장된 (최소) 힙(리스트 형태)과 찾고자 하는 특정 정수 X가 주어집니다. 힙 안에서 정수 X가 존재하는지 여부를 확인하여, 존재하면 "Found", 존재하지 않으면 "Not found"를 출력하세요. `heapq` 모듈은 직접적인 검색 함수를 제공하지 않으므로, 리스트의 `in` 연산자를 사용합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 힙에 저장될 형태로 주어집니다. (이들은 이미 힙 상태라고 가정하지 않고, `heapify` 필요)
셋째 줄에는 찾을 정수 X가 주어집니다.

**입력 예시**
```
7
5 2 8 1 9 4 6
4
```

**출력 예시**
```
Found
```

**풀이 코드**
```python
import heapq

n = int(input())
elements_str = input()
if n > 0:
    elements = list(map(int, elements_str.split()))
else:
    elements = []
heapq.heapify(elements) # 리스트를 힙으로 만듦

x_to_find = int(input())

if x_to_find in elements: # 리스트의 'in' 연산자로 존재 여부 확인
    print("Found")
else:
    print("Not found")
```

**풀이에 대한 설명**
- `heapq.heapify(elements)`: 입력된 리스트를 최소 힙으로 변환합니다.
- `x_to_find = int(input())`: 찾고자 하는 정수 `x_to_find`를 입력받습니다.
- `if x_to_find in elements:`: `heapq`는 리스트를 기반으로 동작하므로, 파이썬 리스트의 `in` 연산자를 사용하여 `x_to_find`가 `elements` 리스트(즉, 힙) 내에 존재하는지 확인할 수 있습니다. 이 연산은 평균적으로 O(N)의 시간 복잡도를 가집니다.
- 존재하면 "Found", 그렇지 않으면 "Not found"를 출력합니다.
- 참고: 힙 구조는 특정 값의 빠른 검색을 위해 설계된 것이 아니라, 최소/최대값의 빠른 접근 및 추출을 위해 설계되었습니다. 따라서 힙 내 특정 값 검색은 일반 리스트 검색과 유사한 성능을 보입니다.

---

## ✅ 문제 12. 힙의 모든 원소를 오름차순으로 정렬하여 출력

**설명**
N개의 정수가 주어집니다. 이 정수들을 최소 힙에 모두 넣은 다음, 힙에서 원소를 하나씩 꺼내어 오름차순으로 정렬된 결과를 출력하세요. (즉, 힙 정렬의 기본 원리)

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7
5 2 8 1 9 4 6
```

**출력 예시**
```
1 2 4 5 6 8 9
```

**풀이 코드**
```python
import heapq

n = int(input())
elements_str = input()
if n > 0:
    numbers = list(map(int, elements_str.split()))
else:
    numbers = []

heap = []
for num in numbers:
    heapq.heappush(heap, num)

sorted_list = []
while heap: # 힙이 빌 때까지 반복
    sorted_list.append(heapq.heappop(heap)) # 가장 작은 원소를 꺼내 결과 리스트에 추가

print(*sorted_list)
```

**풀이에 대한 설명**
- `heap = []`: 빈 리스트를 생성하여 최소 힙으로 사용합니다.
- `for num in numbers:`: 입력된 각 숫자 `num`에 대해 `heapq.heappush(heap, num)`를 호출하여 힙에 추가합니다. 모든 숫자가 추가되면 `heap`은 최소 힙의 구조를 가집니다.
- `sorted_list = []`: 정렬된 결과를 저장할 빈 리스트를 생성합니다.
- `while heap:`: 힙(`heap` 리스트)이 비어있지 않은 동안 반복합니다.
    - `sorted_list.append(heapq.heappop(heap))`: `heapq.heappop(heap)`은 현재 힙에서 가장 작은 원소를 제거하고 반환합니다. 이 원소를 `sorted_list`에 추가합니다.
- 모든 원소가 힙에서 제거되어 `sorted_list`에 추가되면, `sorted_list`는 오름차순으로 정렬된 상태가 됩니다. 이를 공백으로 구분하여 출력합니다. 이 과정이 힙 정렬(Heapsort)의 핵심 원리입니다.

---

## ✅ 문제 13. 두 개의 힙 병합하기

**설명**
두 개의 최소 힙(각각 리스트 형태로 주어짐)이 있습니다. 이 두 힙을 병합하여 하나의 새로운 최소 힙을 만들고, 새로운 힙의 루트 값을 출력하세요. 만약 병합된 힙이 비어있다면 -1을 출력합니다.

첫 줄에는 첫 번째 힙의 원소 개수 N1 (0 ≤ N1 ≤ 50)이 주어집니다.
둘째 줄에는 N1개의 정수가 공백으로 구분되어 주어집니다.
셋째 줄에는 두 번째 힙의 원소 개수 N2 (0 ≤ N2 ≤ 50)이 주어집니다.
넷째 줄에는 N2개의 정수가 공백으로 구분되어 주어집니다.
(입력되는 리스트들은 이미 힙 상태일 필요는 없으며, 각각 `heapify` 하거나, 합친 후 `heapify` 합니다.)

**입력 예시**
```
3
1 5 3
4
2 6 4 0
```
(힙1 후보: [1,3,5], 힙2 후보: [0,2,4,6])
(병합: [1,3,5,0,2,4,6] -> heapify -> [0,1,4,3,2,5,6]. 루트: 0)

**출력 예시**
```
0
```

**풀이 코드**
```python
import heapq

n1 = int(input())
elements_str1 = input()
if n1 > 0:
    list1 = list(map(int, elements_str1.split()))
else:
    list1 = []

n2 = int(input())
elements_str2 = input()
if n2 > 0:
    list2 = list(map(int, elements_str2.split()))
else:
    list2 = []

# 두 리스트를 합친다
merged_list = list1 + list2

if not merged_list: # 병합된 리스트가 비어있다면
    print("-1")
else:
    heapq.heapify(merged_list) # 합쳐진 리스트를 힙으로 변환
    print(merged_list[0]) # 힙의 루트 값 출력
```

**풀이에 대한 설명**
- `list1`과 `list2`를 각각 입력받아 구성합니다. 입력 시 각 리스트가 반드시 힙 상태일 필요는 없습니다.
- `merged_list = list1 + list2`: 두 리스트를 단순하게 연결하여 `merged_list`를 만듭니다.
- `if not merged_list:`: 병합된 리스트가 비어있는지 (즉, `list1`과 `list2`가 모두 비어있었는지) 확인합니다. 비어있으면 "-1"을 출력합니다.
- `else:`: 병합된 리스트에 원소가 있다면,
    - `heapq.heapify(merged_list)`: `merged_list` 전체를 그 자리에서 최소 힙으로 변환합니다.
    - `print(merged_list[0])`: 새롭게 구성된 최소 힙의 루트 값(가장 작은 원소)을 출력합니다.
- (다른 방법으로는, 각 리스트를 개별적으로 `heapify`한 후 문제 5번(여러 리스트 병합 정렬)과 유사한 방식으로 두 힙을 병합할 수도 있지만, 여기서는 리스트를 합친 후 한 번에 `heapify`하는 더 간단한 방법을 사용했습니다.)

---

## ✅ 문제 14. 힙에서 짝수만 필터링하여 새 힙 만들기

**설명**
N개의 정수가 저장된 초기 리스트가 주어집니다. 이 리스트의 원소들 중 짝수만을 골라내어 새로운 최소 힙을 구성합니다. 만약 짝수가 하나도 없다면, 새 힙은 비어있게 됩니다. 새롭게 구성된 짝수 힙의 루트 값을 출력하세요. 짝수 힙이 비어있다면 -1을 출력합니다.

첫 줄에는 정수 N (0 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7
1 5 2 8 9 4 6
```

**출력 예시**
```
2
```
(짝수: 2, 8, 4, 6. 이들로 힙 구성: [2,4,6,8]. 루트: 2)

**풀이 코드**
```python
import heapq

n = int(input())
elements_str = input()
if n > 0:
    numbers = list(map(int, elements_str.split()))
else:
    numbers = []

even_numbers_heap = []
for num in numbers:
    if num % 2 == 0: # 짝수인지 확인
        heapq.heappush(even_numbers_heap, num) # 짝수면 새 힙에 추가

if even_numbers_heap: # 짝수 힙이 비어있지 않다면
    print(even_numbers_heap[0]) # 루트 값 출력
else:
    print("-1")
```

**풀이에 대한 설명**
- `even_numbers_heap = []`: 짝수들만 저장하여 최소 힙을 구성할 빈 리스트를 생성합니다.
- 입력된 각 숫자 `num`에 대해:
    - `if num % 2 == 0:`: 숫자가 짝수인지 (2로 나누었을 때 나머지가 0인지) 확인합니다.
    - `heapq.heappush(even_numbers_heap, num)`: 만약 짝수라면, `even_numbers_heap`에 해당 숫자를 최소 힙의 속성을 유지하며 추가합니다.
- 모든 숫자를 처리한 후:
    - `if even_numbers_heap:`: `even_numbers_heap`이 비어있지 않은지 (즉, 짝수가 하나라도 있었는지) 확인합니다.
    - 비어있지 않다면, `even_numbers_heap[0]` (짝수 힙의 가장 작은 값, 즉 루트)를 출력합니다.
    - 비어있다면 (짝수가 하나도 없었다면), "-1"을 출력합니다.

---

## ✅ 문제 15. 힙에 중복 값 추가 시도 및 크기 변화 관찰

**설명**
초기 최소 힙이 주어지고, 이후 여러 개의 정수 X가 순서대로 주어집니다. 각 정수 X를 힙에 `heappush`합니다. 매번 `heappush`를 실행한 *직후*의 힙의 크기(원소 개수)를 출력하세요. 힙은 중복된 값을 허용합니다.

첫 줄에는 초기 힙의 원소 개수 N (0 ≤ N ≤ 50)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.
셋째 줄에는 추가할 정수의 개수 M (1 ≤ M ≤ 50)이 주어집니다.
다음 M개의 줄에는 각각 추가할 정수 X가 한 줄에 하나씩 주어집니다.

**입력 예시**
```
3
10 20 5
2
20
5
```
(초기 힙 후보: [10,20,5] -> heapify -> [5,20,10], 크기 3
20 push -> [5,20,10,20], 크기 4. 출력: 4
5 push -> [5,5,10,20,20], 크기 5. 출력: 5)

**출력 예시**
```
4
5
```

**풀이 코드**
```python
import heapq
import sys

n_initial = int(sys.stdin.readline())
initial_elements_str = sys.stdin.readline().strip()
heap = []
if n_initial > 0:
    initial_elements = list(map(int, initial_elements_str.split()))
    heapq.heapify(initial_elements)
    heap = initial_elements # heapify는 in-place 연산

m_additional = int(sys.stdin.readline())
results = []
for _ in range(m_additional):
    x_to_add = int(sys.stdin.readline())
    heapq.heappush(heap, x_to_add)
    results.append(str(len(heap))) # push 후 힙의 크기 기록

print("\n".join(results))
```

**풀이에 대한 설명**
- 초기 원소들로 리스트를 구성한 후, `heapq.heapify()`를 사용하여 최소 힙으로 만듭니다. `heap` 변수가 이 리스트를 참조합니다.
- 추가할 정수의 개수 `m_additional`만큼 반복합니다:
    - 각 정수 `x_to_add`를 입력받습니다.
    - `heapq.heappush(heap, x_to_add)`: `x_to_add`를 현재 `heap`에 추가합니다. 힙은 중복된 값을 허용하므로, 동일한 값이 이미 힙에 있더라도 문제없이 추가됩니다.
    - `results.append(str(len(heap)))`: `heappush` 연산 직후의 `heap`의 크기(원소 개수)를 `len(heap)`으로 얻어 문자열로 변환한 뒤 `results` 리스트에 추가합니다.
- 모든 추가 연산이 끝나면, `results` 리스트에 저장된 각 시점의 힙 크기들을 줄바꿈으로 구분하여 출력합니다.

---

## ✅ 문제 16. 가장 작은 두 원소 합치기 반복

**설명**
N개의 양의 정수가 주어집니다. 이 숫자들을 모두 최소 힙에 넣습니다. 힙에 두 개 이상의 원소가 남아있는 동안 다음 작업을 반복합니다:
1. 힙에서 가장 작은 두 원소를 꺼냅니다.
2. 두 원소의 합을 계산합니다.
3. 계산된 합을 다시 힙에 넣습니다.
힙에 원소가 하나만 남게 되면, 그 마지막 원소의 값을 출력합니다. 이 과정은 특정 알고리즘(예: 허프만 코드 생성 시 비용 계산)의 일부와 유사합니다.

첫 줄에는 정수 N (1 ≤ N ≤ 100)이 주어집니다.
둘째 줄에는 N개의 양의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
4
10 20 30 40
```
(힙: [10,20,30,40]
1. pop 10, 20. 합 30. push 30. 힙: [30,30,40]
2. pop 30, 30. 합 60. push 60. 힙: [40,60]
3. pop 40, 60. 합 100. push 100. 힙: [100]
결과: 100)

**출력 예시**
```
100
```

**풀이 코드**
```python
import heapq

n = int(input())
numbers_str = input()
heap = []
if n > 0:
    numbers = list(map(int, numbers_str.split()))
    heapq.heapify(numbers) # 입력된 숫자들로 최소 힙 구성
    heap = numbers

while len(heap) >= 2: # 힙에 원소가 2개 이상 있는 동안 반복
    val1 = heapq.heappop(heap) # 가장 작은 원소
    val2 = heapq.heappop(heap) # 두 번째로 작은 원소
    
    current_sum = val1 + val2
    heapq.heappush(heap, current_sum) # 두 원소의 합을 다시 힙에 추가

if heap: # 최종적으로 힙에 원소가 하나 남음
    print(heap[0])
else: # 입력 N=0 또는 N=1이었던 경우 (문제 조건상 N>=1)
      # N=1이면 루프 안돌고 바로 출력
    if n == 1 and numbers: print(numbers[0])
    elif n==0: print("-1") # 혹은 다른 적절한 처리
```

**풀이에 대한 설명**
- 입력된 숫자들로 리스트를 만든 후 `heapq.heapify()`를 호출하여 최소 힙 `heap`을 구성합니다.
- `while len(heap) >= 2:`: 힙에 원소가 2개 이상 남아있는 동안 루프를 반복합니다. (원소가 1개만 남으면 더 이상 두 개를 꺼낼 수 없으므로 루프가 종료됩니다.)
    - `val1 = heapq.heappop(heap)`: 힙에서 가장 작은 원소를 꺼냅니다.
    - `val2 = heapq.heappop(heap)`: 힙에서 (이제) 가장 작은 원소 (원래 두 번째로 작았던 원소)를 꺼냅니다.
    - `current_sum = val1 + val2`: 두 원소의 합을 계산합니다.
    - `heapq.heappush(heap, current_sum)`: 계산된 합 `current_sum`을 다시 힙에 추가합니다.
- 루프가 종료되면 힙에는 단 하나의 원소만 남게 됩니다. (만약 초기 N=0 또는 N=1이면 루프는 실행되지 않거나 한 번도 실행되지 않을 수 있습니다.)
- `if heap:` (힙이 비어있지 않다면), `heap[0]`을 통해 그 마지막 남은 원소의 값을 출력합니다. N=1인 경우 이 조건으로 처리됩니다. N=0인 경우를 대비해 추가적인 처리를 고려할 수 있습니다.

---

## ✅ 문제 17. 힙으로 우선순위 큐 구현 (튜플 사용)

**설명**
여러 개의 작업 요청이 (우선순위, 작업ID) 형태의 튜플로 주어집니다. 우선순위는 작은 숫자일수록 높습니다. 이 작업들을 우선순위 큐 (최소 힙 사용)에 넣고, 우선순위가 가장 높은 작업부터 순서대로 작업ID를 출력합니다. 만약 우선순위가 같다면, 작업ID가 작은 것이 먼저 처리됩니다.

첫 줄에는 작업 요청의 수 N (1 ≤ N ≤ 100)이 주어집니다.
다음 N개의 줄에는 각 작업의 우선순위 P와 작업ID I (모두 정수)가 공백으로 구분되어 주어집니다.

**입력 예시**
```
5
1 101
2 202
1 100
3 301
2 200
```

**출력 예시**
```
100
101
200
202
301
```
(힙에 저장되는 튜플 순서 (우선순위, 작업ID):
(1,100), (1,101), (2,200), (2,202), (3,301)
pop 순서대로 작업ID 출력)

**풀이 코드**
```python
import heapq
import sys

n = int(sys.stdin.readline())
priority_queue = [] # 최소 힙으로 사용할 리스트

for _ in range(n):
    priority, task_id = map(int, sys.stdin.readline().split())
    # 튜플 (우선순위, 작업ID)를 힙에 추가
    # 힙은 튜플의 첫 번째 요소로 먼저 비교, 같으면 두 번째 요소로 비교
    heapq.heappush(priority_queue, (priority, task_id))

results = []
while priority_queue:
    # 힙에서 가장 작은 튜플 (즉, 우선순위가 가장 높은 작업)을 꺼냄
    p, tid = heapq.heappop(priority_queue)
    results.append(str(tid)) # 작업ID만 결과에 추가

print("\n".join(results))
```

**풀이에 대한 설명**
- `priority_queue = []`: 최소 힙으로 사용될 리스트를 초기화합니다.
- 각 작업 요청에 대해 (우선순위 `priority`, 작업ID `task_id`)를 입력받습니다.
- `heapq.heappush(priority_queue, (priority, task_id))`: `(priority, task_id)` 형태의 튜플을 힙에 추가합니다. `heapq`는 튜플을 저장할 때 튜플의 첫 번째 요소(`priority`)를 기준으로 우선 정렬하고, 첫 번째 요소가 같으면 두 번째 요소(`task_id`)를 기준으로 정렬합니다. 따라서 우선순위가 낮은 숫자(높은 우선순위)가 먼저, 우선순위가 같으면 작업ID가 작은 것이 먼저 처리될 수 있도록 합니다.
- `while priority_queue:`: 힙이 빌 때까지 반복합니다.
    - `p, tid = heapq.heappop(priority_queue)`: 힙에서 가장 작은 튜플(가장 우선순위가 높은 작업)을 꺼내어 `p` (우선순위)와 `tid` (작업ID)에 할당합니다.
    - `results.append(str(tid))`: 작업ID `tid`를 결과 리스트에 추가합니다.
- 최종적으로 `results` 리스트에 저장된 작업ID들을 줄바꿈으로 구분하여 출력합니다.

---

## ✅ 문제 18. 힙에 이미 있는 값보다 작은 값만 추가하기

**설명**
초기 최소 힙이 주어지고 (비어있을 수 있음), 이후 여러 개의 정수 X가 순서대로 주어집니다. 각 정수 X에 대해, 만약 힙이 비어있거나 X가 현재 힙의 가장 작은 원소(루트)보다 작다면 X를 힙에 추가합니다. 그렇지 않으면 (X가 힙의 루트보다 크거나 같다면) X를 추가하지 않습니다. 매번 정수 X를 처리한 후, 현재 힙의 루트 값을 출력합니다. 힙이 비어있다면 -1을 출력합니다.

첫 줄에는 초기 힙의 원소 개수 N (0 ≤ N ≤ 50)이 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.
셋째 줄에는 처리할 정수의 개수 M (1 ≤ M ≤ 50)이 주어집니다.
다음 M개의 줄에는 각각 처리할 정수 X가 한 줄에 하나씩 주어집니다.

**입력 예시**
```
3
10 20 5
4
3
15
1
8
```
(초기 힙 후보: [10,20,5] -> heapify -> [5,20,10].
X=3: 3 < 5. push 3. 힙: [3,5,10,20]. 루트: 3. 출력: 3
X=15: 15 > 3. 추가 안함. 힙: [3,5,10,20]. 루트: 3. 출력: 3
X=1: 1 < 3. push 1. 힙: [1,3,10,20,5]. 루트: 1. 출력: 1
X=8: 8 > 1. 추가 안함. 힙: [1,3,10,20,5]. 루트: 1. 출력: 1)

**출력 예시**
```
3
3
1
1
```

**풀이 코드**
```python
import heapq
import sys

n_initial = int(sys.stdin.readline())
initial_elements_str = sys.stdin.readline().strip()
heap = []
if n_initial > 0:
    initial_elements = list(map(int, initial_elements_str.split()))
    heapq.heapify(initial_elements)
    heap = initial_elements

m_process = int(sys.stdin.readline())
results = []
for _ in range(m_process):
    x_to_process = int(sys.stdin.readline())
    
    should_push = False
    if not heap: # 힙이 비어있으면 무조건 추가
        should_push = True
    elif x_to_process < heap[0]: # X가 현재 힙의 루트보다 작으면 추가
        should_push = True
        
    if should_push:
        heapq.heappush(heap, x_to_process)
        
    if heap:
        results.append(str(heap[0]))
    else:
        results.append("-1")

print("\n".join(results))
```

**풀이에 대한 설명**
- 초기 힙 `heap`을 구성합니다.
- 처리할 각 정수 `x_to_process`에 대해:
    - `should_push = False` 플래그를 설정합니다.
    - `if not heap:`: 만약 힙이 현재 비어있다면, 어떤 값이든 추가할 수 있으므로 `should_push`를 `True`로 설정합니다.
    - `elif x_to_process < heap[0]:`: 힙이 비어있지 않고, `x_to_process`가 현재 힙의 가장 작은 원소(`heap[0]`)보다 작다면, `should_push`를 `True`로 설정합니다.
    - `if should_push:`: `should_push`가 `True`이면, `heapq.heappush(heap, x_to_process)`를 통해 `x_to_process`를 힙에 추가합니다.
    - `if heap:`: 현재 (갱신된) 힙이 비어있지 않다면, `heap[0]` (루트 값)을 `results`에 추가합니다.
    - `else:`: 힙이 비어있다면, "-1"을 `results`에 추가합니다.
- 모든 정수 처리가 끝나면, `results`에 저장된 각 시점의 루트 값(또는 -1)들을 줄바꿈으로 구분하여 출력합니다.

---

## ✅ 문제 19. 힙에서 상위 K개 원소만 남기기

**설명**
N개의 정수가 주어지고, 정수 K가 주어집니다. 처음에는 모든 N개의 정수를 최소 힙에 넣습니다. 그 다음, 만약 힙의 크기가 K보다 크다면, 힙의 크기가 정확히 K가 될 때까지 힙에서 가장 작은 원소들을 계속 제거합니다. 최종적으로 힙에 남아있는 K개 (또는 그 이하, 만약 초기 N < K 였다면)의 원소들을 오름차순으로 정렬하여 공백으로 구분해 출력합니다. (즉, N개의 숫자 중 가장 큰 K개의 숫자를 남기고 오름차순으로 출력하는 것과 유사)

첫 줄에는 정수 N과 K (1 ≤ K ≤ N ≤ 1000, 또는 K > N도 가능)가 공백으로 구분되어 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
5 2 8 1 9 4 6
```

**출력 예시**
```
6 8 9
```
(힙: [1,2,4,5,6,8,9]. K=3. 힙 크기 7 > 3.
pop 1. 힙: [2,4,5,6,8,9]
pop 2. 힙: [4,5,6,8,9]
pop 4. 힙: [5,6,8,9]
pop 5. 힙: [6,8,9]. 크기 3. 중단.
남은 힙 [6,8,9]를 정렬 (이미 힙이므로 루트가 최소)하여 출력. -> 실제로는 6,8,9 가 정렬된 결과로 나옴)

**풀이 코드**
```python
import heapq

n, k = map(int, input().split())
numbers_str = input()
heap = []
if n > 0:
    numbers = list(map(int, numbers_str.split()))
    # 모든 숫자를 일단 힙에 넣는다.
    # 이 문제의 의도는 "상위 K개만 남기기"이므로,
    # 최대 K 크기의 최소힙을 유지하며 큰 값들을 남기는 방식이 더 적합.
    # 즉, 문제 4(K번째 큰 수)의 로직과 유사하게 진행.
    
    for num in numbers:
        if len(heap) < k:
            heapq.heappush(heap, num)
        else:
            # 현재 숫자가 힙의 가장 작은 값(heap[0])보다 크면,
            # 힙의 가장 작은 값을 제거하고 현재 숫자를 추가
            if num > heap[0]:
                heapq.heapreplace(heap, num)
                
# 힙에 남아있는 K개 (또는 N개, 만약 N<K)의 원소는 가장 큰 K개임.
# 이들을 오름차순으로 정렬하여 출력.
# 힙에서 하나씩 꺼내면 오름차순으로 나옴.
result = []
while heap:
    result.append(heapq.heappop(heap))

if not result and n==0: # 초기 입력이 없고, K도 의미 없을때.
    pass # 아무것도 출력하지 않거나, 빈 줄 등.
elif not result and n>0 and k==0: # K=0이라 아무것도 안 남은 경우
    pass # 아무것도 출력하지 않거나, 빈 줄 등.
else:
    print(*result)

```

**풀이에 대한 설명**
- 이 문제는 주어진 N개의 숫자 중 가장 큰 K개의 숫자를 찾아 오름차순으로 출력하는 것과 동일합니다.
- `heap = []`: 최대 크기가 K인 최소 힙을 유지할 리스트입니다.
- 입력된 각 숫자 `num`에 대해:
    - `if len(heap) < k:`: 현재 힙의 크기가 K보다 작으면, `num`을 힙에 추가합니다.
    - `else:` (힙의 크기가 K일 때):
        - `if num > heap[0]:`: 현재 숫자 `num`이 힙의 가장 작은 값(`heap[0]`)보다 크다면, `num`은 K개의 가장 큰 숫자들 중 하나가 될 가능성이 있습니다.
        - `heapq.heapreplace(heap, num)`: 힙에서 가장 작은 값(루트)을 제거하고, `num`을 힙에 추가합니다. 이렇게 하면 힙은 항상 K개의 가장 큰 값들을 (최소 힙 형태로) 유지하게 됩니다.
- 모든 숫자를 처리한 후, `heap`에는 N개의 숫자 중 가장 큰 K개의 숫자들이 남아있습니다. (만약 N < K 였다면, N개의 모든 숫자가 남아있습니다.)
- `result = []` 리스트를 만들고, `while heap:` 루프를 통해 `heap`에서 원소를 하나씩 `heappop`하여 `result`에 추가합니다. `heappop`은 항상 가장 작은 원소를 반환하므로, `result` 리스트는 자연스럽게 오름차순으로 구성됩니다.
- 최종적으로 `result` 리스트를 공백으로 구분하여 출력합니다. 만약 `result`가 비어있다면 (예: N=0이거나, K=0으로 인해 아무것도 남지 않은 경우), 아무것도 출력하지 않거나 문제 조건에 따라 빈 줄을 출력할 수 있습니다.

---

## ✅ 문제 20. 주기적으로 힙의 중간값 (또는 유사값) 출력

**설명**
N개의 정수가 순서대로 주어집니다. 매 K번째 정수가 입력될 때마다, 지금까지 입력된 모든 숫자들을 최소 힙에 넣고, 힙의 크기를 M이라고 할 때, M이 홀수이면 중간값 (M//2 번째 인덱스 값), M이 짝수이면 왼쪽 중간값 ((M//2)-1 번째 인덱스 값)을 출력합니다. 힙의 특정 인덱스 값을 직접 가져오기 위해서는 힙을 정렬하거나 `nsmallest`를 사용해야 합니다. 여기서는 `nsmallest`를 사용하여 해당 위치의 값을 찾습니다.

첫 줄에는 정수 N과 K (1 ≤ K ≤ N ≤ 100)가 주어집니다.
둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**
```
7 3
5 2 8 1 9 4 6
```
(
1. 5 -> 힙 [5]
2. 2 -> 힙 [2,5]
3. 8 -> 힙 [2,5,8]. K번째(3). 크기 3(홀수). 중간값 인덱스 3//2=1. nsmallest(2, [2,5,8])[-1]은 5. 출력: 5
4. 1 -> 힙 [1,2,5,8]
5. 9 -> 힙 [1,2,5,8,9]
6. 4 -> 힙 [1,2,4,5,8,9]. K번째(6). 크기 6(짝수). 왼쪽중간 인덱스 (6//2)-1=2. nsmallest(3, [1,2,4,5,8,9])[-1]은 4. 출력: 4
7. 6 -> 힙 [1,2,4,5,6,8,9]
)

**출력 예시**
```
5
4
```

**풀이 코드**
```python
import heapq
import sys

n, k_interval = map(int, sys.stdin.readline().split())
numbers_str = sys.stdin.readline().strip()
all_numbers = []
if n > 0:
    all_numbers = list(map(int, numbers_str.split()))

current_heap = []
results = []

for i in range(n):
    heapq.heappush(current_heap, all_numbers[i])
    
    # (i+1)이 K의 배수일 때 (즉, 매 K번째 정수가 입력되었을 때)
    if (i + 1) % k_interval == 0:
        heap_size = len(current_heap)
        if heap_size > 0:
            if heap_size % 2 == 1: # 홀수 크기
                # 중간값 인덱스: heap_size // 2
                # (heap_size // 2) + 1 개의 가장 작은 값들 중 가장 큰 값
                median_idx_target = (heap_size // 2) + 1
                median_val = heapq.nsmallest(median_idx_target, current_heap)[-1]
                results.append(str(median_val))
            else: # 짝수 크기
                # 왼쪽 중간값 인덱스: (heap_size // 2) - 1
                # (heap_size // 2) 개의 가장 작은 값들 중 가장 큰 값
                left_median_idx_target = heap_size // 2
                left_median_val = heapq.nsmallest(left_median_idx_target, current_heap)[-1]
                results.append(str(left_median_val))
        # else: 힙이 비어있는 경우는 이 로직에 도달하기 어려움 (i>=0, k_interval>=1)

print("\n".join(results))
```

**풀이에 대한 설명**
- `current_heap = []`: 현재까지 입력된 숫자들을 저장할 최소 힙입니다.
- `results = []`: 매 K번째마다 계산된 중간값(또는 왼쪽 중간값)을 저장할 리스트입니다.
- 입력된 각 숫자 `all_numbers[i]`에 대해:
    - `heapq.heappush(current_heap, all_numbers[i])`: 현재 숫자를 `current_heap`에 추가합니다.
    - `if (i + 1) % k_interval == 0:`: 현재까지 처리한 숫자의 개수 `(i+1)`이 `k_interval`의 배수인지 확인합니다. (즉, K번째, 2K번째, ... 숫자 처리 시점)
        - `heap_size = len(current_heap)`: 현재 힙의 크기를 가져옵니다.
        - `if heap_size % 2 == 1:` (힙 크기가 홀수):
            - 중간값은 정렬했을 때 `heap_size // 2` 번째 인덱스에 위치합니다. 이는 `(heap_size // 2) + 1` 개의 가장 작은 값들 중 가장 큰 값과 같습니다.
            - `median_idx_target = (heap_size // 2) + 1`
            - `median_val = heapq.nsmallest(median_idx_target, current_heap)[-1]`: `heapq.nsmallest()`를 사용하여 힙에서 `median_idx_target`개의 가장 작은 값들을 리스트로 얻고, 그 리스트의 마지막 값(`[-1]`)이 중간값이 됩니다.
            - `results.append(str(median_val))`
        - `else:` (힙 크기가 짝수):
            - 왼쪽 중간값은 정렬했을 때 `(heap_size // 2) - 1` 번째 인덱스에 위치합니다. 이는 `heap_size // 2` 개의 가장 작은 값들 중 가장 큰 값과 같습니다.
            - `left_median_idx_target = heap_size // 2`
            - `left_median_val = heapq.nsmallest(left_median_idx_target, current_heap)[-1]`
            - `results.append(str(left_median_val))`
- 모든 숫자를 처리한 후, `results` 리스트에 저장된 값들을 줄바꿈으로 구분하여 출력합니다.
- 참고: 힙의 특정 인덱스 값을 직접 가져오는 것은 O(1)이 아닙니다. 힙은 루트 값만 빠르게 접근 가능하고, 다른 위치의 값은 힙 구조를 유지하는 선에서 정렬되어 있지 않기 때문입니다. `nsmallest`는 내부적으로 힙 연산을 사용하여 효율적으로 값을 찾지만, 매번 호출 시 비용이 발생합니다. 이 문제는 `heapq`의 활용을 연습하기 위한 것입니다.

