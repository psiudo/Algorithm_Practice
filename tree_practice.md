## 🌳 트리 기본 드릴 문제집 (세트 1)

# 트리 문제의 원본은 Baekjoon https://www.acmicpc.net/ 에 있습니다.

---

### ✅ 문제 1. 이진 트리 노드 생성 및 정보 출력

**설명**
주어진 값으로 이진 트리의 노드를 생성하고, 해당 노드의 값, 왼쪽 자식 노드의 값, 오른쪽 자식 노드의 값을 출력하는 문제입니다. 자식 노드가 없을 경우 'None'으로 표시합니다. 이 문제에서는 노드 생성 연습에 집중하며, 초기 자식은 항상 `None`입니다. 첫 줄에 노드에 저장할 정수 값이 주어집니다.

**입력 예시**

```
10
```

**출력 예시**

```
Node Value: 10
Left Child: None
Right Child: None
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

node_val = int(input())
node = TreeNode(node_val)

print(f"Node Value: {node.value}")
print(f"Left Child: {node.left.value if node.left else 'None'}")
print(f"Right Child: {node.right.value if node.right else 'None'}")
```

**풀이에 대한 설명**
`TreeNode` 클래스를 정의하여 이진 트리의 노드를 표현합니다. 각 노드는 `value` (값), `left` (왼쪽 자식), `right` (오른쪽 자식) 속성을 가집니다. 입력받은 값으로 `TreeNode` 객체를 생성하고, 각 속성을 출력합니다. 이 단계에서는 자식 노드를 연결하지 않으므로 `left`와 `right`는 초기값 `None`을 유지합니다.

---

### ✅ 문제 2. 간단한 이진 트리 구성 및 루트 값 출력

**설명**
세 개의 정수 값 R, L, V가 주어집니다. R은 루트 노드의 값, L은 루트의 왼쪽 자식 노드의 값, V는 루트의 오른쪽 자식 노드의 값입니다. 이 값들로 간단한 이진 트리를 구성하고, 루트 노드의 값과 그 자식들의 값을 출력하세요. 만약 L 또는 V가 -1이면 해당 자식은 없는 것으로 간주합니다.
첫 줄에 R, L, V가 공백으로 구분되어 주어집니다.

**입력 예시**

```
10 5 15
```

**출력 예시**

```
Root: 10
Left Child of Root: 5
Right Child of Root: 15
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

r_val, l_val, v_val = map(int, input().split())

root = TreeNode(r_val)

if l_val != -1:
    root.left = TreeNode(l_val)
if v_val != -1:
    root.right = TreeNode(v_val)

print(f"Root: {root.value}")
if root.left:
    print(f"Left Child of Root: {root.left.value}")
else:
    print("Left Child of Root: None")

if root.right:
    print(f"Right Child of Root: {root.right.value}")
else:
    print("Right Child of Root: None")
```

**풀이에 대한 설명**
`TreeNode` 클래스를 사용하여 트리를 구성합니다. 입력받은 R 값으로 루트 노드를 생성합니다. L 또는 V 값이 -1이 아니면, 해당 값으로 자식 노드를 생성하여 루트 노드의 `left` 또는 `right`에 연결합니다. 최종적으로 루트 노드와 그 자식들의 값을 출력합니다.

---

### ✅ 문제 3. 이진 트리 전위 순회 (Preorder Traversal)

**설명**
N개의 노드를 가진 이진 트리가 주어집니다. 각 노드와 그 자식들의 관계가 문자열로 주어질 때, 이 트리를 구성하고 전위 순회(루트-왼쪽-오른쪽)한 결과를 출력하세요.
첫 줄에는 노드의 개수 N (1 <= N <= 26)이 주어집니다. 다음 N개의 줄에는 각 줄마다 노드 이름(알파벳 대문자), 왼쪽 자식 노드 이름, 오른쪽 자식 노드 이름이 공백으로 구분되어 주어집니다. 자식이 없는 경우는 '.'으로 표시됩니다. 항상 첫 번째로 입력되는 노드가 루트입니다.

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
A B D C E F G
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def preorder_traversal(node, result_list):
    if node is None or node.value == '.':
        return
    result_list.append(node.value)
    if node.left:
        preorder_traversal(nodes[node.left.value] if node.left.value in nodes else None, result_list)
    if node.right:
        preorder_traversal(nodes[node.right.value] if node.right.value in nodes else None, result_list)

n = int(input())
nodes = {} # 노드 이름(값)을 키로, TreeNode 객체를 값으로 저장

# 먼저 모든 노드 객체 생성
for _ in range(n):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
    # 임시로 자식 노드 이름만 저장해둠 (객체 연결은 나중에)
    nodes[data].temp_left = left_child
    nodes[data].temp_right = right_child

# 생성된 노드 객체들을 이용해 트리 연결
root_node_char = ''
is_first_node = True
for node_char in nodes:
    node_obj = nodes[node_char]
    if is_first_node: # 첫번째 입력된 노드를 루트로 가정
        root_node_char = node_char
        is_first_node = False

    if hasattr(node_obj, 'temp_left') and node_obj.temp_left != '.':
        if node_obj.temp_left in nodes: # 자식 노드가 실제로 존재하는지 확인
            node_obj.left = nodes[node_obj.temp_left]
    if hasattr(node_obj, 'temp_right') and node_obj.temp_right != '.':
        if node_obj.temp_right in nodes: # 자식 노드가 실제로 존재하는지 확인
            node_obj.right = nodes[node_obj.temp_right]

result = []
if root_node_char and root_node_char in nodes:
    preorder_traversal(nodes[root_node_char], result)
print(' '.join(result))
```

**풀이에 대한 설명**
`TreeNode` 클래스로 각 노드를 표현합니다. 입력으로 주어지는 노드 정보를 바탕으로 `nodes` 딕셔너리에 각 노드 객체를 저장합니다. 모든 노드 객체를 먼저 생성한 후, 각 노드의 `left` 및 `right` 자식 정보를 이용해 실제 `TreeNode` 객체들을 연결하여 트리를 완성합니다.
`preorder_traversal` 함수는 재귀적으로 트리를 순회합니다: 현재 노드 값을 리스트에 추가하고, 왼쪽 서브트리를 전위 순회한 뒤, 오른쪽 서브트리를 전위 순회합니다. 루트 노드는 입력에서 첫 번째로 정의된 노드로 가정합니다. `nodes[node.left.value]`와 같이 접근하기 전에 `node.left.value`가 `nodes` 딕셔너리에 있는지 확인해야 합니다 (이 코드에서는 자식 노드 객체를 직접 연결하므로 이 부분 수정됨). 순회 결과를 공백으로 구분하여 출력합니다.

---

### ✅ 문제 4. 이진 트리 중위 순회 (Inorder Traversal)

**설명**
문제 3과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리를 중위 순회(왼쪽-루트-오른쪽)한 결과를 출력하세요.

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
D B A E C G F
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def inorder_traversal(node, result_list):
    if node is None or node.value == '.':
        return
    if node.left:
        inorder_traversal(nodes[node.left.value] if node.left.value in nodes else None, result_list)
    result_list.append(node.value)
    if node.right:
        inorder_traversal(nodes[node.right.value] if node.right.value in nodes else None, result_list)

n = int(input())
nodes = {}

for _ in range(n):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
    nodes[data].temp_left = left_child
    nodes[data].temp_right = right_child

root_node_char = ''
is_first_node = True
for node_char in nodes:
    node_obj = nodes[node_char]
    if is_first_node:
        root_node_char = node_char
        is_first_node = False

    if hasattr(node_obj, 'temp_left') and node_obj.temp_left != '.':
        if node_obj.temp_left in nodes:
             node_obj.left = nodes[node_obj.temp_left]
    if hasattr(node_obj, 'temp_right') and node_obj.temp_right != '.':
        if node_obj.temp_right in nodes:
            node_obj.right = nodes[node_obj.temp_right]

result = []
if root_node_char and root_node_char in nodes:
    inorder_traversal(nodes[root_node_char], result)
print(' '.join(result))
```

**풀이에 대한 설명**
트리 구성 방식은 문제 3과 동일합니다. `inorder_traversal` 함수는 재귀적으로 트리를 순회합니다: 먼저 왼쪽 서브트리를 중위 순회하고, 현재 노드 값을 리스트에 추가한 뒤, 오른쪽 서브트리를 중위 순회합니다. 순회 결과를 공백으로 구분하여 출력합니다.

---

### ✅ 문제 5. 이진 트리 후위 순회 (Postorder Traversal)

**설명**
문제 3과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리를 후위 순회(왼쪽-오른쪽-루트)한 결과를 출력하세요.

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
D B E G F C A
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def postorder_traversal(node, result_list):
    if node is None or node.value == '.':
        return
    if node.left:
        postorder_traversal(nodes[node.left.value] if node.left.value in nodes else None, result_list)
    if node.right:
        postorder_traversal(nodes[node.right.value] if node.right.value in nodes else None, result_list)
    result_list.append(node.value)

n = int(input())
nodes = {}

for _ in range(n):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
    nodes[data].temp_left = left_child
    nodes[data].temp_right = right_child

root_node_char = ''
is_first_node = True
for node_char in nodes:
    node_obj = nodes[node_char]
    if is_first_node:
        root_node_char = node_char
        is_first_node = False

    if hasattr(node_obj, 'temp_left') and node_obj.temp_left != '.':
        if node_obj.temp_left in nodes:
            node_obj.left = nodes[node_obj.temp_left]
    if hasattr(node_obj, 'temp_right') and node_obj.temp_right != '.':
        if node_obj.temp_right in nodes:
            node_obj.right = nodes[node_obj.temp_right]

result = []
if root_node_char and root_node_char in nodes:
    postorder_traversal(nodes[root_node_char], result)
print(' '.join(result))
```

**풀이에 대한 설명**
트리 구성 방식은 문제 3과 동일합니다. `postorder_traversal` 함수는 재귀적으로 트리를 순회합니다: 먼저 왼쪽 서브트리를 후위 순회하고, 그 다음 오른쪽 서브트리를 후위 순회한 뒤, 마지막으로 현재 노드 값을 리스트에 추가합니다. 순회 결과를 공백으로 구분하여 출력합니다.

---

### ✅ 문제 6. 이진 트리의 노드 개수 세기

**설명**
문제 3과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리의 전체 노드 개수를 세어 출력하세요. ('.'으로 표시된 빈 자식 노드는 개수에 포함하지 않습니다.)

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
7
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def count_nodes(node):
    if node is None or node.value == '.': # '.' 값 자체를 노드로 취급하지 않음
        return 0

    # 현재 노드 1개 + 왼쪽 서브트리 노드 개수 + 오른쪽 서브트리 노드 개수
    count = 1
    if node.left and node.left.value != '.': # 실제 연결된 노드인지 확인
         count += count_nodes(nodes[node.left.value] if node.left.value in nodes else None)
    if node.right and node.right.value != '.': # 실제 연결된 노드인지 확인
         count += count_nodes(nodes[node.right.value] if node.right.value in nodes else None)
    return count

n_input = int(input()) # 입력 줄 수, 실제 노드 개수와는 다를 수 있음 ('.' 제외)
nodes = {}
actual_node_values = set()

for _ in range(n_input):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
        actual_node_values.add(data)

    nodes[data].temp_left_val = left_child
    nodes[data].temp_right_val = right_child

root_node_char = ''
parent_candidate = set(actual_node_values)
children_nodes = set()

# 트리 연결 및 루트 찾기
for node_char_val in actual_node_values:
    node_obj = nodes[node_char_val]

    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes:
        node_obj.left = nodes[left_val]
        children_nodes.add(left_val)

    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes:
        node_obj.right = nodes[right_val]
        children_nodes.add(right_val)

# 루트 노드는 다른 노드의 자식이 아닌 노드 중 하나
possible_roots = parent_candidate - children_nodes
if not possible_roots and n_input > 0 : # 모든 노드가 누군가의 자식인 경우 (cycle or disconnected but one node is root)
    # 이 문제에서는 첫 번째 입력된 실제 노드를 루트로 가정 (문제 3,4,5와 일관성)
    root_node_char = list(actual_node_values)[0] if actual_node_values else ''
elif possible_roots:
     root_node_char = list(possible_roots)[0]


if root_node_char and root_node_char in nodes:
    # print(count_nodes(nodes[root_node_char])) # 이 방식은 재귀적으로 실제 노드만 셈
    print(len(actual_node_values)) # 실제 노드 값들의 개수를 세는 것이 더 간단하고 문제 의도에 맞음
else:
    print(0)

```

**풀이에 대한 설명**
트리 구성 방식은 이전과 유사하나, 노드 개수를 셀 때는 '.'으로 표시된 빈 노드를 제외합니다. `count_nodes` 함수는 재귀적으로 현재 노드가 유효하면 1을 더하고, 왼쪽과 오른쪽 자식에 대해 재귀 호출하여 그 결과를 합산합니다.
다만, 이 문제의 입력 형식에서는 `N`이 주어지고 각 줄이 유효한 노드 정의이므로, 실제 생성된 `TreeNode` 객체의 개수 (즉, `nodes` 딕셔너리의 `actual_node_values` 집합의 크기)가 전체 노드 개수가 됩니다. `count_nodes` 재귀 함수를 사용하는 대신, `actual_node_values` 집합의 크기를 출력하는 것이 더 직접적입니다.

---

### ✅ 문제 7. 이진 트리의 높이(깊이) 계산하기

**설명**
문제 3과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리의 높이(또는 깊이)를 계산하여 출력하세요. 트리의 높이는 루트 노드에서 가장 멀리 있는 리프 노드까지의 경로에 있는 노드의 수로 정의합니다. (루트 노드만 있는 경우 높이는 1)

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
4
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def get_height(node):
    if node is None or node.value == '.':
        return 0

    # 왼쪽 서브트리의 높이와 오른쪽 서브트리의 높이 중 더 큰 값을 선택하고 +1 (현재 노드)
    left_height = 0
    if node.left and node.left.value != '.':
        left_height = get_height(nodes[node.left.value] if node.left.value in nodes else None)

    right_height = 0
    if node.right and node.right.value != '.':
        right_height = get_height(nodes[node.right.value] if node.right.value in nodes else None)

    return max(left_height, right_height) + 1

n = int(input())
nodes = {}
actual_node_values = set()

for _ in range(n):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
        actual_node_values.add(data)
    nodes[data].temp_left_val = left_child
    nodes[data].temp_right_val = right_child

root_node_char = ''
parent_candidate = set(actual_node_values)
children_nodes = set()

for node_char_val in actual_node_values:
    node_obj = nodes[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes:
        node_obj.left = nodes[left_val]
        children_nodes.add(left_val)
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes:
        node_obj.right = nodes[right_val]
        children_nodes.add(right_val)

possible_roots = parent_candidate - children_nodes
if not possible_roots and n > 0:
    root_node_char = list(actual_node_values)[0] if actual_node_values else ''
elif possible_roots:
    root_node_char = list(possible_roots)[0]

if root_node_char and root_node_char in nodes:
    print(get_height(nodes[root_node_char]))
else:
    print(0)
```

**풀이에 대한 설명**
트리 구성은 이전 문제들과 유사합니다. `get_height` 함수는 재귀적으로 트리의 높이를 계산합니다. 만약 현재 노드가 `None`이거나 유효하지 않은 노드(값 '.')이면 높이는 0입니다. 그렇지 않으면, 왼쪽 서브트리의 높이와 오른쪽 서브트리의 높이를 각각 재귀적으로 계산하고, 둘 중 더 큰 값에 1 (현재 노드 자신)을 더한 값을 반환합니다. 루트 노드 찾기 로직은 문제 6과 유사하게 적용됩니다.

---

### ✅ 문제 8. 이진 트리에서 특정 값 찾기

**설명**
문제 3과 동일한 입력 형식으로 이진 트리가 주어지고, 추가로 찾고자 하는 노드의 값(알파벳 대문자)이 주어집니다. 트리 내에 해당 값을 가진 노드가 존재하는지 여부를 `True` 또는 `False`로 출력하세요.

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
E
```

**출력 예시**

```
True
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def find_value(node, target_value):
    if node is None or node.value == '.':
        return False
    if node.value == target_value:
        return True

    found_left = False
    if node.left and node.left.value != '.':
        found_left = find_value(nodes[node.left.value] if node.left.value in nodes else None, target_value)

    if found_left: # 왼쪽에서 찾았으면 오른쪽은 볼 필요 없음
        return True

    found_right = False
    if node.right and node.right.value != '.':
        found_right = find_value(nodes[node.right.value] if node.right.value in nodes else None, target_value)

    return found_right

n = int(input())
nodes = {}
actual_node_values = set()

for _ in range(n):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
        actual_node_values.add(data)
    nodes[data].temp_left_val = left_child
    nodes[data].temp_right_val = right_child

# 마지막 줄에서 찾을 값 입력
target = input()

root_node_char = ''
parent_candidate = set(actual_node_values)
children_nodes = set()

for node_char_val in actual_node_values:
    node_obj = nodes[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes:
        node_obj.left = nodes[left_val]
        children_nodes.add(left_val)
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes:
        node_obj.right = nodes[right_val]
        children_nodes.add(right_val)

possible_roots = parent_candidate - children_nodes
if not possible_roots and n > 0:
    root_node_char = list(actual_node_values)[0] if actual_node_values else ''
elif possible_roots:
    root_node_char = list(possible_roots)[0]


if root_node_char and root_node_char in nodes:
    print(find_value(nodes[root_node_char], target))
else:
    print(False) # 트리가 비었거나 루트를 못 찾으면 False
```

**풀이에 대한 설명**
트리 구성은 이전과 같습니다. `find_value` 함수는 재귀적으로 특정 값을 찾습니다. 현재 노드가 `None`이거나 유효하지 않으면 `False`를 반환합니다. 현재 노드의 값이 찾고자 하는 값과 같으면 `True`를 반환합니다. 그렇지 않으면 왼쪽 서브트리에서 값을 찾아보고, 찾지 못하면 오른쪽 서브트리에서 값을 찾아봅니다. 둘 중 한 곳에서라도 찾으면 `True`를 반환하고, 양쪽 모두에서 찾지 못하면 `False`를 반환합니다.

---

### ✅ 문제 9. 이진 트리의 리프 노드 개수 세기

**설명**
문제 3과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리의 리프 노드(자식 노드가 없는 노드)의 개수를 세어 출력하세요.

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
3
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def count_leaf_nodes(node):
    if node is None or node.value == '.':
        return 0

    is_leaf = True
    if node.left and node.left.value != '.':
        is_leaf = False
    if node.right and node.right.value != '.':
        is_leaf = False

    if is_leaf:
        return 1

    leaf_count = 0
    if node.left and node.left.value != '.':
         leaf_count += count_leaf_nodes(nodes[node.left.value] if node.left.value in nodes else None)
    if node.right and node.right.value != '.':
         leaf_count += count_leaf_nodes(nodes[node.right.value] if node.right.value in nodes else None)
    return leaf_count

n = int(input())
nodes = {}
actual_node_values = set()

for _ in range(n):
    data, left_child, right_child = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
        actual_node_values.add(data)
    nodes[data].temp_left_val = left_child
    nodes[data].temp_right_val = right_child

root_node_char = ''
parent_candidate = set(actual_node_values)
children_nodes = set()

for node_char_val in actual_node_values:
    node_obj = nodes[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes:
        node_obj.left = nodes[left_val]
        children_nodes.add(left_val)
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes:
        node_obj.right = nodes[right_val]
        children_nodes.add(right_val)

possible_roots = parent_candidate - children_nodes
if not possible_roots and n > 0:
    root_node_char = list(actual_node_values)[0] if actual_node_values else ''
elif possible_roots:
    root_node_char = list(possible_roots)[0]


if root_node_char and root_node_char in nodes:
    print(count_leaf_nodes(nodes[root_node_char]))
else:
    print(0)
```

**풀이에 대한 설명**
트리 구성은 이전과 같습니다. `count_leaf_nodes` 함수는 재귀적으로 리프 노드의 개수를 셉니다.
현재 노드가 `None`이거나 유효하지 않으면 0을 반환합니다.
현재 노드의 왼쪽 자식과 오른쪽 자식이 모두 없거나 유효하지 않은 노드(값이 '.')이면 현재 노드는 리프 노드이므로 1을 반환합니다.
그렇지 않으면 (리프 노드가 아니면), 왼쪽 서브트리의 리프 노드 개수와 오른쪽 서브트리의 리프 노드 개수를 재귀적으로 구해 합산한 결과를 반환합니다.

---

### ✅ 문제 10. 이진 검색 트리(BST) 기본 삽입 및 루트 값 확인

**설명**
여러 개의 정수가 주어질 때, 이 정수들을 순서대로 이진 검색 트리(BST)에 삽입합니다. 이진 검색 트리는 각 노드에 대해 왼쪽 서브트리의 모든 값은 현재 노드의 값보다 작고, 오른쪽 서브트리의 모든 값은 현재 노드의 값보다 큰 속성을 가집니다. 모든 값을 삽입한 후 최종 트리의 루트 노드 값을 출력하세요. 만약 입력된 값이 없다면 'Empty'를 출력합니다.
첫 줄에는 삽입할 정수의 개수 N (0 <= N <= 100)이 주어집니다. 둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다.

**입력 예시**

```
5
50 30 70 20 40
```

**출력 예시**

```
50
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def insert_bst(root, value):
    if root is None:
        return TreeNode(value)

    current = root
    while True:
        if value < current.value:
            if current.left is None:
                current.left = TreeNode(value)
                break
            current = current.left
        elif value > current.value: # BST에서는 중복 값을 허용하지 않거나 특정 규칙을 따름 (여기서는 중복값 무시 또는 오른쪽으로)
                                    # 이 문제에서는 중복값이 없다고 가정하거나, 크면 오른쪽으로 보냄.
            if current.right is None:
                current.right = TreeNode(value)
                break
            current = current.right
        else: # value == current.value, 중복 값 처리 (여기서는 삽입하지 않음)
            break
    return root # 항상 원래 루트를 반환


num_count = int(input())
if num_count == 0:
    print("Empty")
else:
    values = list(map(int, input().split()))

    bst_root = None
    if values: # 값이 하나라도 있으면 첫 번째 값을 루트로 시작
        bst_root = TreeNode(values[0])
        for i in range(1, len(values)):
            insert_bst(bst_root, values[i]) # 루트는 변경되지 않음

    if bst_root:
        print(bst_root.value)
    else: # 이 경우는 num_count > 0 이지만 values가 빈 경우 (문제 조건상 발생 안함)
        print("Empty")
```

**풀이에 대한 설명**
이진 검색 트리(BST)의 노드는 `TreeNode` 클래스로 표현합니다. `insert_bst` 함수는 주어진 `value`를 BST에 삽입합니다.

- 만약 루트가 `None`이면, 새 노드를 생성하여 루트로 만듭니다.
- 그렇지 않으면, 현재 노드에서 시작하여 `value`와 비교합니다. - `value`가 현재 노드의 값보다 작으면 왼쪽 자식으로 이동합니다. 왼쪽 자식이 비어있으면 새 노드를 왼쪽에 삽입합니다. - `value`가 현재 노드의 값보다 크면 오른쪽 자식으로 이동합니다. 오른쪽 자식이 비어있으면 새 노드를 오른쪽에 삽입합니다. - `value`가 현재 노드의 값과 같으면 (중복된 값), 이 문제에서는 별도의 처리를 하지 않고 삽입을 중단합니다 (또는 특정 규칙에 따라 처리 가능).
  입력받은 정수들을 순서대로 BST에 삽입한 후, 최종 트리의 루트 노드 값을 출력합니다. 만약 입력된 정수가 없으면 "Empty"를 출력합니다. `insert_bst` 함수는 루트 노드를 반환하며, 반복적인 삽입 과정에서 루트는 변경되지 않습니다 (첫 번째 삽입된 값이 루트가 됨).

### ✅ 문제 11. 이진 트리 레벨 순서 순회 (Level Order Traversal)

**설명**
N개의 노드를 가진 이진 트리가 주어집니다. 각 노드와 그 자식들의 관계가 문자열로 주어질 때, 이 트리를 구성하고 레벨 순서 순회(너비 우선 탐색, BFS)한 결과를 출력하세요. 루트 노드부터 시작하여 각 레벨별로 왼쪽에서 오른쪽 순서로 방문합니다.
첫 줄에는 노드의 개수 N (1 <= N <= 26)이 주어집니다. 다음 N개의 줄에는 각 줄마다 노드 이름(알파벳 대문자), 왼쪽 자식 노드 이름, 오른쪽 자식 노드 이름이 공백으로 구분되어 주어집니다. 자식이 없는 경우는 '.'으로 표시됩니다. 루트는 다른 노드의 자식이 아닌 노드로 간주하거나, 명시적으로 첫 번째 입력된 노드로 가정할 수 있습니다. (여기서는 첫 번째 입력된 실제 노드를 루트로 가정합니다.)

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
```

**출력 예시**

```
A B C D E F G
```

**풀이 코드**

```python
from collections import deque

class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def level_order_traversal(root_node, nodes_dict, result_list):
    if root_node is None or root_node.value == '.':
        return

    queue = deque([root_node])

    while queue:
        current_node = queue.popleft()
        if current_node is None or current_node.value == '.': # Should not happen if properly added
            continue

        result_list.append(current_node.value)

        if current_node.left and current_node.left.value != '.':
            # Ensure the child object exists in nodes_dict before adding to queue
            if current_node.left.value in nodes_dict:
                 queue.append(nodes_dict[current_node.left.value])
                 # Actually, current_node.left should already be the object if linked properly
                 # queue.append(current_node.left) would be better if fully linked.

        if current_node.right and current_node.right.value != '.':
            if current_node.right.value in nodes_dict:
                queue.append(nodes_dict[current_node.right.value])
                # queue.append(current_node.right)

# --- 트리 구성 로직 (문제 3과 유사) ---
n = int(input())
nodes = {} # 노드 이름(값)을 키로, TreeNode 객체를 값으로 저장
node_input_order = [] # 입력 순서대로 노드 이름 저장

# 먼저 모든 노드 객체 생성
for i in range(n):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes:
        nodes[data] = TreeNode(data)
    if i == 0: # 첫 번째 입력된 노드를 루트 후보로 우선 저장
        root_node_char_candidate = data

    # 임시로 자식 노드 이름만 저장
    nodes[data].temp_left_val = left_child_val
    nodes[data].temp_right_val = right_child_val

# 생성된 노드 객체들을 이용해 트리 연결
for node_char_val in nodes:
    node_obj = nodes[node_char_val]

    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes: # 자식 노드가 정의되어 있고, '.'이 아니면 연결
        node_obj.left = nodes[left_val]

    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes: # 자식 노드가 정의되어 있고, '.'이 아니면 연결
        node_obj.right = nodes[right_val]

result = []
# 첫번째 입력된 노드를 루트로 사용
root_node = nodes.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes else None

if root_node:
    # BFS를 위해 큐에 실제 TreeNode 객체를 넣어야 함
    # level_order_traversal 함수 수정:
    q = deque()
    if root_node:
        q.append(root_node)

    while q:
        curr = q.popleft()
        result.append(curr.value)
        if curr.left:
            q.append(curr.left)
        if curr.right:
            q.append(curr.right)

print(' '.join(result))
```

**풀이에 대한 설명**
트리 구성은 이전 문제들과 유사합니다. `level_order_traversal` 함수 대신, 직접 BFS 로직을 구현합니다. `collections.deque`를 사용하여 큐(queue)를 만들고, 루트 노드를 큐에 추가합니다. 큐가 빌 때까지 다음 과정을 반복합니다:

1. 큐에서 노드를 하나 꺼냅니다.
2. 해당 노드의 값을 결과 리스트에 추가합니다.
3. 해당 노드의 왼쪽 자식이 존재하면 큐에 추가합니다.
4. 해당 노드의 오른쪽 자식이 존재하면 큐에 추가합니다.
   이 과정을 통해 레벨 순서대로 노드가 방문되며, 최종 결과를 공백으로 구분하여 출력합니다. 풀이 코드에서는 트리 구성 후 BFS 로직을 직접 적용했습니다.

---

### ✅ 문제 12. 완전 이진 트리(Complete Binary Tree)인지 확인하기

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리가 완전 이진 트리인지 여부를 `True` 또는 `False`로 출력하세요. 완전 이진 트리는 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 모든 노드는 가능한 한 왼쪽에 정렬되어 있습니다.

**입력 예시**

```
7
A B C
B D E
C F .
D . .
E . .
F . .
```

(위 예시는 A를 루트로 할 때 완전 이진 트리임: A(B,C), B(D,E), C(F))

**출력 예시**

```
True
```

**풀이 코드**

```python
from collections import deque

class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

def is_complete_binary_tree(root):
    if not root:
        return True # 빈 트리는 완전 이진 트리로 간주 가능

    queue = deque([root])
    found_none_child = False # None 자식을 만난 적이 있는지 표시하는 플래그

    while queue:
        current_node = queue.popleft()

        if current_node.left:
            if found_none_child: # None 자식 이후에 실제 자식이 나오면 완전 이진 트리 아님
                return False
            queue.append(current_node.left)
        else: # 왼쪽 자식이 None이면
            found_none_child = True

        if current_node.right:
            if found_none_child: # None 자식 (왼쪽이 없거나, 이전 노드의 오른쪽이 없었음) 이후에 실제 자식이 나오면 완전 이진 트리 아님
                return False
            queue.append(current_node.right)
        else: # 오른쪽 자식이 None이면
            found_none_child = True

    return True

root = nodes_dict.get(root_node_char) if root_node_char else None

if not root_node_char or not root: # 노드가 없거나 루트를 못찾으면
    if n_nodes_defined == 0: # 정의된 노드가 0개면 완전 이진 트리
         print(True)
    else: # 정의된 노드가 있는데 루트가 없으면 (이상한 케이스)
         print(False)
else:
    print(is_complete_binary_tree(root))
```

**풀이에 대한 설명**
완전 이진 트리를 확인하는 한 가지 방법은 레벨 순서 순회(BFS)를 이용하는 것입니다.

1. 큐를 사용하여 BFS를 수행합니다.
2. 순회 중 `None`인 자식 노드를 만나면 `found_none_child` 플래그를 `True`로 설정합니다.
3. 만약 `found_none_child`가 `True`인 상태에서 실제 자식 노드(`None`이 아닌 노드)를 만나면, 이는 완전 이진 트리의 조건을 위배하므로 `False`를 반환합니다. (즉, 중간에 빈 공간이 있다는 의미)
4. 순회가 끝날 때까지 위 조건에 걸리지 않으면 `True`를 반환합니다.
   루트 노드는 입력의 첫 번째 노드로 가정합니다.

---

### ✅ 문제 13. 포화 이진 트리(Perfect Binary Tree)인지 확인하기

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리가 포화 이진 트리(모든 내부 노드가 두 개의 자식을 갖고 모든 리프 노드가 같은 레벨에 있는 트리)인지 여부를 `True` 또는 `False`로 출력하세요.

**입력 예시**

```
7
A B C
B D E
C F G
D . .
E . .
F . .
G . .
```

**출력 예시**

```
True
```

**풀이 코드**

```python
import math

class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char = None
actual_node_count = 0

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
        actual_node_count +=1 # 실제 노드 수 카운트
    if i == 0:
        root_node_char = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict: # 실제 노드들에 대해서만 자식 연결
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

def get_depth(node): # 리프까지의 최대 깊이 (높이와 유사)
    if not node:
        return 0
    return 1 + max(get_depth(node.left), get_depth(node.right))

def is_perfect_recursive(node, depth, level=0):
    if not node: # 빈 노드는 포화 트리의 일부가 아님 (호출되지 않아야 함)
        return True

    # 현재 노드가 리프 노드인지 확인
    if not node.left and not node.right:
        return depth == level + 1 # 리프 노드의 깊이가 전체 깊이와 같은지

    # 내부 노드인데 자식이 하나만 있으면 포화 트리가 아님
    if not node.left or not node.right:
        return False

    # 왼쪽과 오른쪽 서브트리에 대해 재귀적으로 확인
    return is_perfect_recursive(node.left, depth, level + 1) and \
           is_perfect_recursive(node.right, depth, level + 1)


root = nodes_dict.get(root_node_char) if root_node_char else None

if not root_node_char or not root:
    if n_nodes_defined == 0: # 정의된 노드가 0개면 (빈 트리)
        print(True) # 빈 트리는 포화 이진 트리로 간주하기도 함. 문제에 따라 다를 수 있음.
    else:
        print(False)
else:
    # 방법 1: 높이와 노드 수 관계 (2^h - 1)
    # 실제 트리 노드 수를 세는 것이 더 정확함 (입력의 N이 아닌)

    # 모든 노드가 0개 또는 2개의 자식을 가져야하고 모든 리프가 같은 레벨
    # 먼저 깊이(height)를 구함
    depth_val = get_depth(root) # 루트 레벨 0 기준이면 depth, 루트 레벨 1 기준이면 height
                                # 여기서는 get_depth가 노드 개수를 반환하므로 height와 같음

    # 노드 수 계산 (BFS/DFS 등으로 실제 노드 수 세기)
    # 여기서는 actual_node_count 를 사용

    # 포화 이진 트리의 노드 수는 2^(height) - 1 이어야 함
    expected_nodes = (2**depth_val) - 1

    if actual_node_count == expected_nodes:
        # 추가적으로 모든 리프가 같은 깊이이고 모든 내부 노드가 자식 2개인지 확인
        # is_perfect_recursive 함수 사용
        print(is_perfect_recursive(root, depth_val))
    else:
        print(False)

```

**풀이에 대한 설명**
포화 이진 트리(Perfect Binary Tree)는 다음 두 가지 조건을 만족합니다:

1.  모든 내부 노드는 정확히 두 개의 자식 노드를 가집니다.
2.  모든 리프 노드는 동일한 깊이(레벨)에 있습니다.

이를 확인하는 한 가지 방법은 트리의 높이 `h`와 실제 노드 수 `count`를 구하는 것입니다. 포화 이진 트리라면 `count`는 <span class="math-inline">2^h \- 1</span>을 만족해야 합니다.
추가적으로, 모든 리프가 실제로 같은 깊이에 있는지, 모든 내부 노드가 자식 2개를 가지는지 재귀적으로 확인할 수 있습니다 (`is_perfect_recursive` 함수).
`get_depth` 함수는 트리의 깊이(루트에서 가장 먼 리프까지의 노드 수)를 계산합니다. `actual_node_count`는 입력에서 정의된 실제 노드의 수입니다.

---

### ✅ 문제 14. 두 이진 트리가 같은지 확인하기 (Same Tree)

**설명**
두 개의 이진 트리가 주어집니다. 이 두 트리가 구조적으로 동일하고, 각 대응하는 노드의 값도 같은지 확인하여 `True` 또는 `False`를 출력하세요.
입력은 두 개의 트리 각각에 대해 노드의 개수와 노드 정보(문제 11과 동일한 형식)가 주어집니다.

**입력 예시**

```
3
A B C
B D .
C . .
3
X Y Z
Y W .
Z . .
```

(위 예시는 A-X, B-Y, C-Z, D-W가 값만 다르고 구조는 같다고 가정. 값이 같아야 True)

**입력 예시 2 (True인 경우)**

```
3
A B C
B D .
C . E
3
A B C
B D .
C . E
```

**출력 예시 2**

```
True
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def build_tree_from_input():
    n_str = input()
    if not n_str: # 빈 줄 등으로 N을 못 읽으면 빈 트리 처리
        return None, {}
    n = int(n_str)
    if n == 0:
        return None, {}

    nodes_dict = {}
    root_node_char_candidate = None

    for i in range(n):
        line = input()
        if not line: continue # 빈 줄 스킵
        data, left_child_val, right_child_val = line.split()

        if data not in nodes_dict:
            nodes_dict[data] = TreeNode(data)
        if i == 0:
            root_node_char_candidate = data

        nodes_dict[data].temp_left_val = left_child_val
        nodes_dict[data].temp_right_val = right_child_val

    for node_char_val in nodes_dict:
        node_obj = nodes_dict[node_char_val]
        left_val = node_obj.temp_left_val
        if left_val != '.' and left_val in nodes_dict:
            node_obj.left = nodes_dict[left_val]
        right_val = node_obj.temp_right_val
        if right_val != '.' and right_val in nodes_dict:
            node_obj.right = nodes_dict[right_val]

    return nodes_dict.get(root_node_char_candidate) if root_node_char_candidate else None, nodes_dict


def is_same_tree(p_node, q_node):
    # 둘 다 None이면 같은 것으로 간주
    if not p_node and not q_node:
        return True
    # 하나만 None이거나 값이 다르면 다른 트리
    if not p_node or not q_node or p_node.value != q_node.value:
        return False

    # 재귀적으로 왼쪽 자식들과 오른쪽 자식들이 같은지 확인
    return is_same_tree(p_node.left, q_node.left) and \
           is_same_tree(p_node.right, q_node.right)

# 첫 번째 트리 입력 및 빌드
root1, nodes1 = build_tree_from_input()

# 두 번째 트리 입력 및 빌드
root2, nodes2 = build_tree_from_input()

print(is_same_tree(root1, root2))
```

**풀이에 대한 설명**
`build_tree_from_input` 함수는 주어진 입력 형식으로부터 하나의 이진 트리를 구성하고 루트 노드를 반환합니다. 이 함수를 두 번 호출하여 두 개의 트리를 만듭니다.
`is_same_tree` 함수는 두 개의 노드 `p_node`와 `q_node`를 인자로 받아, 이들을 루트로 하는 서브트리가 동일한지 재귀적으로 확인합니다.

1.  두 노드가 모두 `None`이면, 동일하므로 `True`를 반환합니다.
2.  한쪽 노드만 `None`이거나, 두 노드의 값이 다르면, 동일하지 않으므로 `False`를 반환합니다.
3.  두 노드가 모두 존재하고 값도 같으면, `p_node`의 왼쪽 자식과 `q_node`의 왼쪽 자식에 대해 재귀 호출하고, `p_node`의 오른쪽 자식과 `q_node`의 오른쪽 자식에 대해 재귀 호출합니다. 두 재귀 호출 결과가 모두 `True`여야 두 트리가 동일한 것으로 간주합니다.

---

### ✅ 문제 15. 이진 트리의 최소 깊이(Minimum Depth) 계산하기

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리의 최소 깊이를 계산하여 출력하세요. 최소 깊이는 루트 노드에서 가장 가까운 리프 노드까지의 경로에 있는 노드의 수로 정의합니다. (루트 노드만 있는 경우 최소 깊이는 1)

**입력 예시**

```
5
A B C
B D .
C . .
D . .
E . .
```

(E는 연결 안됨. A-C가 가장 짧은 리프 경로)

**출력 예시**

```
2
```

**풀이 코드**

```python
from collections import deque

class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

def min_depth_bfs(root):
    if not root:
        return 0

    queue = deque([(root, 1)]) # (노드, 현재 깊이)

    while queue:
        current_node, depth = queue.popleft()

        # 현재 노드가 리프 노드이면, 현재 깊이가 최소 깊이
        if not current_node.left and not current_node.right:
            return depth

        if current_node.left:
            queue.append((current_node.left, depth + 1))
        if current_node.right:
            queue.append((current_node.right, depth + 1))
    return 0 # 비정상적인 경우 (예: 리프 없는 트리인데 호출됨)

root = nodes_dict.get(root_node_char) if root_node_char else None

if not root_node_char or not root:
     print(0)
else:
    print(min_depth_bfs(root))
```

**풀이에 대한 설명**
최소 깊이는 너비 우선 탐색(BFS)을 사용하여 효율적으로 찾을 수 있습니다.

1. 큐에 `(노드, 현재_깊이)` 쌍을 저장합니다. 루트 노드와 깊이 1로 시작합니다.
2. 큐에서 노드를 하나 꺼냅니다.
3. 만약 꺼낸 노드가 리프 노드(왼쪽 자식과 오른쪽 자식이 모두 없는 노드)이면, 해당 노드의 깊이가 현재까지 발견된 가장 가까운 리프까지의 깊이이므로 이 값을 반환합니다. BFS는 레벨 순으로 탐색하므로 처음 만나는 리프 노드가 최소 깊이를 가집니다.
4. 리프 노드가 아니면, 유효한 자식 노드들을 (자식 노드, 현재\_깊이 + 1)의 형태로 큐에 추가합니다.
   루트가 없으면 0을 반환합니다.

---

### ✅ 문제 16. 균형 이진 트리(Balanced Binary Tree)인지 확인하기 (간단한 정의)

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리가 균형 이진 트리인지 여부를 `True` 또는 `False`로 출력하세요. 균형 이진 트리는 모든 노드에 대해 해당 노드의 왼쪽 서브트리의 높이와 오른쪽 서브트리의 높이 차이가 1 이하인 트리입니다.

**입력 예시**

```
7
A B C
B D E
C F G
D . .
E . .
F . .
G . .
```

**출력 예시**

```
True
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

# (높이, 균형여부)를 반환하는 헬퍼 함수
def get_height_and_check_balance(node):
    if not node:
        return 0, True # 빈 노드는 높이 0, 균형 상태

    left_height, is_left_balanced = get_height_and_check_balance(node.left)
    if not is_left_balanced:
        return -1, False # 왼쪽 서브트리가 불균형이면 전체도 불균형

    right_height, is_right_balanced = get_height_and_check_balance(node.right)
    if not is_right_balanced:
        return -1, False # 오른쪽 서브트리가 불균형이면 전체도 불균형

    # 현재 노드에서의 균형 확인
    if abs(left_height - right_height) > 1:
        return -1, False # 현재 노드가 불균형

    # 현재 노드가 균형이면, 현재 노드를 루트로 하는 트리의 높이 반환
    return 1 + max(left_height, right_height), True

def is_balanced(root):
    if not root:
        return True
    height, balanced = get_height_and_check_balance(root)
    return balanced

root = nodes_dict.get(root_node_char) if root_node_char else None

if not root_node_char or not root:
    if n_nodes_defined == 0:
        print(True) # 빈 트리는 균형으로 간주
    else:
        print(False)
else:
    print(is_balanced(root))

```

**풀이에 대한 설명**
`get_height_and_check_balance` 함수는 각 노드에 대해 (서브트리의 높이, 해당 서브트리가 균형인지 여부)를 반환합니다.

1.  노드가 `None`이면 (0, `True`)를 반환합니다.
2.  왼쪽 서브트리와 오른쪽 서브트리에 대해 재귀적으로 호출하여 각각의 높이와 균형 상태를 얻습니다.
3.  만약 자식 서브트리 중 하나라도 불균형이면, 현재 노드를 루트로 하는 트리도 불균형이므로 (-1, `False`)를 반환합니다 (높이 -1은 불균형 신호로 사용).
4.  자식 서브트리들이 모두 균형이면, 현재 노드에서 왼쪽과 오른쪽 서브트리의 높이 차가 1을 초과하는지 확인합니다. 초과하면 불균형이므로 (-1, `False`)를 반환합니다.
5.  모든 조건을 만족하면, 현재 노드를 루트로 하는 트리는 균형이며, 높이는 `1 + max(왼쪽 높이, 오른쪽 높이)`이고, (`계산된 높이`, `True`)를 반환합니다.
    `is_balanced` 함수는 이 헬퍼 함수의 결과 중 균형 여부만 반환합니다.

---

### ✅ 문제 17. 주어진 경로가 이진 트리에 존재하는지 확인하기

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어지고, 추가로 문자(노드 값)의 시퀀스로 표현된 경로가 주어집니다. 이 경로가 루트부터 시작하여 트리 내에 순서대로 존재하는지 여부를 `True` 또는 `False`로 출력하세요.
경로는 첫 줄에 공백 없이 문자열로 주어집니다.

**입력 예시**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
ABDF
```

(경로는 ABDF가 아닌 ABD임. 예시 수정 필요)
**입력 예시 수정**

```
7
A B C
B D .
C E F
D . .
E . .
F . G
G . .
ABD
```

**출력 예시 수정**

```
True
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

path_str = input() # 경로 문자열

def check_path(current_node, path_chars, index):
    # 경로의 현재 문자가 노드의 값과 일치하는지 확인
    if not current_node or current_node.value != path_chars[index]:
        return False

    # 경로의 마지막 문자에 도달했고, 현재 노드 값과 일치하면 경로 존재
    if index == len(path_chars) - 1:
        return True

    # 다음 경로 문자를 위해 왼쪽 자식 탐색
    if current_node.left:
        if check_path(current_node.left, path_chars, index + 1):
            return True

    # 다음 경로 문자를 위해 오른쪽 자식 탐색
    if current_node.right:
        if check_path(current_node.right, path_chars, index + 1):
            return True

    return False # 이 노드에서는 경로를 찾지 못함

root = nodes_dict.get(root_node_char) if root_node_char else None

if not root_node_char or not root or not path_str: # 루트가 없거나 경로가 비었으면
    print(False)
else:
    # 경로의 첫 문자는 루트 노드의 값과 일치해야 함
    print(check_path(root, list(path_str), 0))

```

**풀이에 대한 설명**
`check_path` 함수는 현재 노드(`current_node`), 전체 경로 문자 리스트(`path_chars`), 그리고 경로에서 현재 확인해야 할 문자의 인덱스(`index`)를 인자로 받습니다.

1.  만약 `current_node`가 `None`이거나, `current_node`의 값이 `path_chars[index]`와 다르면 경로는 존재하지 않으므로 `False`를 반환합니다.
2.  만약 `index`가 경로의 마지막 인덱스이고 (즉, 경로의 모든 문자를 확인했고), 현재 노드의 값이 경로의 마지막 문자와 일치하면, 경로가 존재하므로 `True`를 반환합니다.
3.  그렇지 않으면 (경로가 아직 남았으면), 다음 경로 문자(`path_chars[index+1]`)에 대해 왼쪽 자식과 오른쪽 자식으로 재귀 호출을 수행합니다. 둘 중 하나라도 `True`를 반환하면 경로가 존재하는 것입니다.
    초기 호출은 루트 노드, 전체 경로, 인덱스 0으로 시작합니다.

---

### ✅ 문제 18. 이진 트리에서 두 노드의 최소 공통 조상(LCA) 찾기 (간단한 경우)

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어지고, 추가로 두 개의 서로 다른 노드 값(N1, N2)이 주어집니다. 이 두 노드의 최소 공통 조상(LCA) 노드의 값을 출력하세요. 두 노드는 항상 트리에 존재한다고 가정합니다.

**입력 예시**

```
7
A B C
B D E
C F G
D . .
E . .
F . .
G . .
D G
```

**출력 예시**

```
A
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

node1_val, node2_val = input().split()

def find_lca(current_node, val1, val2):
    if not current_node:
        return None

    # 현재 노드가 찾으려는 두 값 중 하나이면, 현재 노드가 (잠재적) LCA
    if current_node.value == val1 or current_node.value == val2:
        return current_node

    # 왼쪽 서브트리에서 LCA 탐색
    left_lca = find_lca(current_node.left, val1, val2)
    # 오른쪽 서브트리에서 LCA 탐색
    right_lca = find_lca(current_node.right, val1, val2)

    # 만약 왼쪽과 오른쪽 서브트리 모두에서 각각 val1, val2를 찾았다면 (즉, non-None 반환)
    # 현재 노드가 LCA
    if left_lca and right_lca:
        return current_node

    # 그렇지 않으면, non-None인 쪽을 반환 (한쪽에 두 노드가 다 있거나, 하나만 있는 경우)
    return left_lca if left_lca is not None else right_lca


root = nodes_dict.get(root_node_char) if root_node_char else None

if root and node1_val in nodes_dict and node2_val in nodes_dict:
    lca_node = find_lca(root, node1_val, node2_val)
    if lca_node:
        print(lca_node.value)
    else:
        print("LCA not found") # 이론상으론 문제 조건에 의해 항상 찾아져야 함
else:
    print("Invalid input or tree")

```

**풀이에 대한 설명**
최소 공통 조상(LCA)을 찾는 표준적인 재귀 알고리즘을 사용합니다.
`find_lca(current_node, val1, val2)` 함수:

1.  `current_node`가 `None`이면 `None`을 반환합니다.
2.  `current_node`의 값이 `val1` 또는 `val2`와 같으면, `current_node` 자체를 반환합니다 (이것이 LCA일 수 있거나, 더 상위 조상의 자식 중 하나일 수 있음).
3.  왼쪽 서브트리와 오른쪽 서브트리에 대해 각각 `find_lca`를 재귀적으로 호출합니다.
4.  만약 왼쪽 호출(`left_lca`)과 오른쪽 호출(`right_lca`)이 모두 `None`이 아닌 유효한 노드를 반환하면, 이는 `val1`과 `val2`가 각각 왼쪽과 오른쪽 서브트리에 나뉘어 존재한다는 의미이므로, `current_node`가 LCA입니다. `current_node`를 반환합니다.
5.  그렇지 않고, `left_lca`와 `right_lca` 중 하나만 유효한 노드를 반환하면 (다른 하나는 `None`), 그 유효한 노드를 반환합니다. 이는 두 노드가 모두 해당 서브트리 아래에 존재하거나, 해당 노드 자체가 `val1` 또는 `val2` 중 하나임을 의미합니다.
6.  둘 다 `None`이면 `None`을 반환합니다.
    초기 호출은 트리의 루트 노드와 찾고자 하는 두 값으로 시작합니다.

---

### ✅ 문제 19. 일반 트리(N-ary Tree) 노드 생성 및 자식 추가

**설명**
일반 트리(각 노드가 여러 개의 자식을 가질 수 있는 트리)의 노드를 나타내는 클래스를 만들고, 주어진 값으로 노드를 생성한 후 여러 자식 노드를 추가하는 예제입니다. 노드의 값과 자식들의 값을 출력합니다.
첫 줄에는 부모 노드의 정수 값이 주어집니다. 둘째 줄에는 추가할 자식 노드의 개수 M (0 <= M <= 10)이 주어집니다. 셋째 줄에는 M개의 자식 노드 값이 공백으로 구분되어 주어집니다.

**입력 예시**

```
10
3
5 6 7
```

**출력 예시**

```
Parent Node: 10
Children: [5, 6, 7]
```

**풀이 코드**

```python
class NaryTreeNode:
    def __init__(self, value):
        self.value = value
        self.children = [] # 자식 노드들을 저장할 리스트

    def add_child(self, child_node):
        self.children.append(child_node)

parent_val = int(input())
parent_node = NaryTreeNode(parent_val)

num_children = int(input())
if num_children > 0:
    children_values = list(map(int, input().split()))
    for child_val in children_values:
        child_node = NaryTreeNode(child_val) # 자식도 NaryTreeNode 객체여야 함
        parent_node.add_child(child_node)

print(f"Parent Node: {parent_node.value}")
children_output = [child.value for child in parent_node.children]
print(f"Children: {children_output}")

```

**풀이에 대한 설명**
`NaryTreeNode` 클래스는 일반 트리의 노드를 표현합니다. 각 노드는 `value`와 자식 노드들의 리스트인 `children`을 가집니다. `add_child` 메소드는 자식 노드 객체를 `children` 리스트에 추가합니다.
입력으로 부모 노드의 값, 자식의 수, 그리고 자식들의 값을 받아 부모 노드 객체를 생성하고, 각 자식 값에 대해 `NaryTreeNode` 객체를 만들어 부모의 `children` 리스트에 추가합니다. 마지막으로 부모 노드의 값과 자식 노드들의 값 리스트를 출력합니다.

---

### ✅ 문제 20. 일반 트리의 깊이 우선 탐색 (DFS)

**설명**
간단한 일반 트리(N-ary Tree)가 주어집니다. 트리는 부모-자식 관계의 리스트로 표현됩니다. 이 트리를 구성하고 루트부터 시작하여 깊이 우선 탐색(DFS)을 수행한 결과를 출력하세요. 자식들은 입력된 순서(또는 특정 정렬 순서)대로 방문합니다. (여기서는 입력된 순서대로 방문)
첫 줄에는 총 노드의 개수 N과 루트 노드의 값(정수)이 주어집니다. 다음 N-1개의 줄에는 각 줄마다 부모 노드 값과 자식 노드 값이 공백으로 구분되어 주어집니다. (모든 노드는 0부터 N-1까지의 정수로 표현된다고 가정하고, 루트가 0이라고 가정합시다.)

**입력 예시 수정** (문자 대신 숫자로, 루트 명시)

```
7 0
0 1
0 2
1 3
1 4
2 5
2 6
```

**출력 예시 수정** (0 1 3 4 2 5 6)

```
0 1 3 4 2 5 6
```

**풀이 코드**

```python
class NaryTreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

    def add_child_obj(self, child_node_obj): # 객체를 직접 추가
        self.children.append(child_node_obj)

def dfs_nary(node, result_list):
    if node is None:
        return

    result_list.append(node.value)
    for child in node.children:
        dfs_nary(child, result_list)

num_total_nodes, root_val = map(int, input().split())
nodes_nary = {} # 값(숫자)을 키로, NaryTreeNode 객체를 값으로 저장

# 모든 노드 객체 미리 생성
for i in range(num_total_nodes):
    nodes_nary[i] = NaryTreeNode(i) # 값이 0부터 N-1까지라고 가정

# 간선 정보로 트리 구성
for _ in range(num_total_nodes - 1): # N-1개의 간선
    parent_v, child_v = map(int, input().split())
    if parent_v in nodes_nary and child_v in nodes_nary:
        nodes_nary[parent_v].add_child_obj(nodes_nary[child_v])
        # 자식들을 특정 순서로 정렬하고 싶다면 여기서 정렬
        # nodes_nary[parent_v].children.sort(key=lambda x: x.value)


result = []
if root_val in nodes_nary:
    dfs_nary(nodes_nary[root_val], result)
print(' '.join(map(str, result)))

```

**풀이에 대한 설명**
일반 트리의 노드는 `NaryTreeNode` 클래스로 표현되며, 각 노드는 자식들의 리스트를 가집니다.
입력으로 노드 수와 루트 노드 값이 주어지고, 이후 부모-자식 관계가 주어집니다.

1.  모든 노드에 대해 `NaryTreeNode` 객체를 미리 생성하여 `nodes_nary` 딕셔너리에 저장합니다. (노드 값은 0부터 N-1까지의 정수라고 가정)
2.  주어진 부모-자식 관계를 읽어, 부모 노드의 `children` 리스트에 자식 노드 객체를 추가하여 트리를 구성합니다.
    `dfs_nary` 함수는 DFS를 수행합니다:
3.  현재 노드의 값을 결과 리스트에 추가합니다.
4.  현재 노드의 각 자식에 대해 재귀적으로 `dfs_nary`를 호출합니다. 자식들은 `children` 리스트에 저장된 순서대로 방문합니다.
    최종적으로 DFS 방문 순서대로 노드 값들을 공백으로 구분하여 출력합니다.

---

### ✅ 문제 21. 이진 트리의 지름(Diameter) 구하기

**설명**
N개의 노드를 가진 이진 트리가 주어집니다. 트리의 지름은 트리 내의 두 노드 간 가장 긴 경로의 길이(노드 수 기준)입니다. 이 경로가 반드시 루트를 통과할 필요는 없습니다. 트리의 지름을 계산하여 출력하세요.
입력 형식은 문제 11과 동일합니다. (첫 번째 입력된 실제 노드를 루트로 가정)

**입력 예시**

```
7
A B C
B D E
C . F
D . .
E . .
F . G
G . .
```

(경로 예: D-B-A-C-F-G 또는 E-B-A-C-F-G. 길이 6)

**출력 예시**

```
6
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char_candidate = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char_candidate = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

class DiameterCalculator:
    def __init__(self):
        self.max_diameter = 0 # 노드 수 기준이므로 0으로 시작 (노드 1개면 지름 1)

    def get_height_and_update_diameter(self, node):
        if not node:
            return 0

        left_height = self.get_height_and_update_diameter(node.left)
        right_height = self.get_height_and_update_diameter(node.right)

        # 현재 노드를 루트로 하는 서브트리에서 지름 업데이트
        # 지름 = 왼쪽 서브트리 높이 + 오른쪽 서브트리 높이 + 1 (현재 노드)
        # 이 경로가 현재 노드를 "구부러지는 지점"으로 하는 가장 긴 경로
        current_path_through_node = left_height + right_height + 1
        self.max_diameter = max(self.max_diameter, current_path_through_node)

        # 현재 노드의 높이 반환 (부모 노드로 전달)
        return 1 + max(left_height, right_height)

root = nodes_dict.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes_dict else None

if not root:
    print(0)
else:
    calculator = DiameterCalculator()
    calculator.get_height_and_update_diameter(root)
    # 만약 노드가 하나뿐이면 max_diameter는 1이 되어야 함.
    # get_height_and_update_diameter가 0개의 자식을 가진 노드에 대해 1을 반환하므로,
    # 루트만 있는 경우 left_h=0, right_h=0, current_path=1. max_diameter=1.
    print(calculator.max_diameter)

```

**풀이에 대한 설명**
트리의 지름을 계산하기 위해 각 노드를 기준으로 다음을 고려합니다:

1.  해당 노드의 왼쪽 서브트리의 높이 (`left_height`).
2.  해당 노드의 오른쪽 서브트리의 높이 (`right_height`).
3.  만약 가장 긴 경로가 현재 노드를 통과한다면, 그 길이는 `left_height + right_height + 1` (현재 노드 포함, 노드 수 기준) 입니다.
    `DiameterCalculator` 클래스의 `get_height_and_update_diameter` 함수는 재귀적으로 각 노드의 높이를 계산하면서, 동시에 `self.max_diameter`를 위 3번의 값과 비교하여 최댓값으로 계속 업데이트합니다. 함수 자체는 해당 노드를 루트로 하는 서브트리의 높이를 반환하여 부모 노드의 계산에 사용됩니다. 초기 `max_diameter`는 0으로 설정하고, 단일 노드 트리의 경우 지름은 1이 됩니다.

---

### ✅ 문제 22. 이진 트리를 문자열로 직렬화(Serialize)하기

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리를 전위 순회(Preorder) 기반으로 직렬화하여 하나의 문자열로 만드세요. 노드 값은 쉼표(,)로 구분하고, 자식이 없는 경우(None)는 특별한 문자 '#'으로 표시합니다.
예를 들어, 루트가 'A', 왼쪽 자식이 'B', 오른쪽 자식이 'C'인 트리는 "A,B,#,#,C,#,#"와 같이 직렬화될 수 있습니다.

**입력 예시**

```
3
A B C
B . .
C . .
```

**출력 예시**

```
A,B,#,#,C,#,#
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char_candidate = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char_candidate = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

def serialize_preorder(node, result_parts):
    if not node:
        result_parts.append("#")
        return

    result_parts.append(str(node.value)) # 노드 값을 문자열로 추가
    serialize_preorder(node.left, result_parts)
    serialize_preorder(node.right, result_parts)

root = nodes_dict.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes_dict else None
serialized_parts = []

if not root: # 빈 트리 또는 루트 못 찾음
    if n_nodes_defined == 0 : # 노드 정의가 아예 없었으면 # 하나만.
        serialized_parts.append("#")
    # else: 루트를 못찾았지만 노드가 정의된 경우. 이 경우는 직렬화가 애매함.
    #       문제에서는 루트가 있다고 가정하므로 빈 문자열 또는 #로 처리.
    #       여기선 입력이 없으면 #을 출력하도록 함.
else:
    serialize_preorder(root, serialized_parts)

print(",".join(serialized_parts))
```

**풀이에 대한 설명**
`serialize_preorder` 함수는 트리를 전위 순회 방식으로 직렬화합니다.

1.  현재 노드가 `None`이면, 결과 리스트에 '#' (None을 나타내는 마커)을 추가하고 반환합니다.
2.  현재 노드가 `None`이 아니면, 노드의 값을 문자열로 변환하여 결과 리스트에 추가합니다.
3.  왼쪽 자식에 대해 재귀적으로 `serialize_preorder`를 호출합니다.
4.  오른쪽 자식에 대해 재귀적으로 `serialize_preorder`를 호출합니다.
    모든 재귀 호출이 끝나면, 결과 리스트에 저장된 부분들을 쉼표(,)로 연결하여 최종 직렬화된 문자열을 만듭니다. 빈 트리의 경우 '#'을 출력합니다.

---

### ✅ 문제 23. 직렬화된 문자열로부터 이진 트리 역직렬화(Deserialize)하기

**설명**
문제 22에서 생성된 방식(전위 순회 기반, '#'은 None, 쉼표로 구분)으로 직렬화된 문자열이 주어집니다. 이 문자열로부터 원래의 이진 트리를 복원하고, 복원된 트리의 루트 노드 값을 출력하세요. 만약 문자열이 '#'이거나 비어있으면 "Empty"를 출력합니다.

**입력 예시**

```
A,B,#,#,C,#,#
```

**출력 예시**

```
A
```

**풀이 코드**

```python
from collections import deque

class TreeNode:
    def __init__(self, value):
        self.value = value # 값은 문자열로 저장될 수 있음
        self.left = None
        self.right = None

def deserialize_preorder(data_queue):
    if not data_queue:
        return None

    val = data_queue.popleft()

    if val == "#":
        return None

    node = TreeNode(val) # 값은 문자열 그대로 사용
    node.left = deserialize_preorder(data_queue)
    node.right = deserialize_preorder(data_queue)
    return node

serialized_str = input()

if not serialized_str or serialized_str == "#":
    print("Empty")
else:
    parts = serialized_str.split(',')
    data_queue = deque(parts) # 큐를 사용하여 값들을 순서대로 처리

    root = deserialize_preorder(data_queue)

    if root:
        print(root.value)
    else: # 이 경우는 deserialize_preorder가 None을 반환 (예: "#"만 있는 경우)
        print("Empty")
```

**풀이에 대한 설명**
`deserialize_preorder` 함수는 직렬화된 문자열로부터 트리를 복원합니다. 문자열을 쉼표로 분리하여 값들을 큐에 넣고 사용합니다.

1.  큐에서 값을 하나 꺼냅니다.
2.  값이 '#'이면 `None`을 반환합니다 (해당 위치에 노드가 없음을 의미).
3.  값이 '#'이 아니면, 해당 값으로 `TreeNode`를 생성합니다.
4.  새로 생성된 노드의 왼쪽 자식은 큐의 다음 값들을 사용하여 재귀적으로 `deserialize_preorder`를 호출하여 설정합니다.
5.  새로 생성된 노드의 오른쪽 자식도 마찬가지로 재귀적으로 설정합니다.
6.  생성된 노드를 반환합니다.
    최종적으로 복원된 트리의 루트 노드 값을 출력합니다. 입력 문자열이 비어있거나 '#' 뿐이면 "Empty"를 출력합니다.

---

### ✅ 문제 24. 이진 검색 트리(BST)에서 특정 값보다 작은 가장 큰 값 찾기 (Inorder Predecessor)

**설명**
여러 개의 정수가 주어져 이진 검색 트리(BST)를 구성합니다. 그 후 특정 정수 `target` 값이 주어질 때, BST 내에서 `target` 값보다 작으면서 가장 큰 값을 가진 노드의 값을 출력하세요. 만약 그러한 값이 없으면 "None"을 출력합니다.
첫 줄에는 BST를 구성할 정수의 개수 N (0 <= N <= 100)이 주어집니다. 둘째 줄에는 N개의 정수가 공백으로 구분되어 주어집니다. 셋째 줄에는 `target` 정수 값이 주어집니다. (BST에 `target`과 동일한 값이 있을 수 있습니다)

**입력 예시**

```
7
50 30 70 20 40 60 80
45
```

**출력 예시**

```
40
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def insert_bst(root, value):
    if root is None:
        return TreeNode(value)
    current = root
    while True:
        if value < current.value:
            if current.left is None:
                current.left = TreeNode(value); break
            current = current.left
        elif value > current.value:
            if current.right is None:
                current.right = TreeNode(value); break
            current = current.right
        else: break # 중복 값 무시
    return root

def find_predecessor(root, target_val):
    predecessor = None
    current = root

    while current:
        if current.value < target_val:
            # 현재 노드 값이 target보다 작으면, predecessor 후보.
            # 더 큰 값을 찾기 위해 오른쪽으로 이동 시도.
            predecessor = current.value
            current = current.right
        else: # current.value >= target_val
            # target보다 크거나 같으면, 더 작은 값을 찾아 왼쪽으로 이동.
            current = current.left

    return predecessor

# BST 구성
num_count = int(input())
bst_root = None
if num_count > 0:
    values = list(map(int, input().split()))
    if values:
        bst_root = TreeNode(values[0])
        for i in range(1, len(values)):
            insert_bst(bst_root, values[i])
target = int(input())

if bst_root is None:
    print("None")
else:
    result = find_predecessor(bst_root, target)
    if result is not None:
        print(result)
    else:
        print("None")
```

**풀이에 대한 설명**
`find_predecessor` 함수는 BST에서 `target_val`의 직전값(predecessor)을 찾습니다.

1. `predecessor` 변수를 `None`으로 초기화합니다.
2. 루트에서 시작하여 `current` 노드를 이동시킵니다.
3. 만약 `current.value`가 `target_val`보다 작으면, `current.value`는 잠재적인 직전값입니다. `predecessor`를 `current.value`로 업데이트하고, 더 `target_val`에 가까운 (더 큰) 직전값을 찾기 위해 오른쪽 서브트리로 이동합니다 (`current = current.right`).
4. 만약 `current.value`가 `target_val`보다 크거나 같으면, 직전값은 반드시 왼쪽 서브트리에 있으므로 왼쪽으로 이동합니다 (`current = current.left`).
5. `current`가 `None`이 될 때까지 반복하면, `predecessor`에는 `target_val`보다 작은 가장 큰 값이 저장됩니다.

---

### ✅ 문제 25. 이진 검색 트리(BST)에서 특정 값보다 큰 가장 작은 값 찾기 (Inorder Successor)

**설명**
여러 개의 정수가 주어져 이진 검색 트리(BST)를 구성합니다. 그 후 특정 정수 `target` 값이 주어질 때, BST 내에서 `target` 값보다 크면서 가장 작은 값을 가진 노드의 값을 출력하세요. 만약 그러한 값이 없으면 "None"을 출력합니다.
입력 형식은 문제 24와 동일합니다.

**입력 예시**

```
7
50 30 70 20 40 60 80
55
```

**출력 예시**

```
60
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def insert_bst(root, value): # 문제 24와 동일
    if root is None: return TreeNode(value)
    current = root
    while True:
        if value < current.value:
            if current.left is None: current.left = TreeNode(value); break
            current = current.left
        elif value > current.value:
            if current.right is None: current.right = TreeNode(value); break
            current = current.right
        else: break
    return root

def find_successor(root, target_val):
    successor = None
    current = root

    while current:
        if current.value > target_val:
            # 현재 노드 값이 target보다 크면, successor 후보.
            # 더 작은 값을 찾기 위해 왼쪽으로 이동 시도.
            successor = current.value
            current = current.left
        else: # current.value <= target_val
            # target보다 작거나 같으면, successor는 반드시 오른쪽 서브트리에 있음.
            current = current.right

    return successor

# BST 구성
num_count = int(input())
bst_root = None
if num_count > 0:
    values = list(map(int, input().split()))
    if values:
        bst_root = TreeNode(values[0])
        for i in range(1, len(values)):
            insert_bst(bst_root, values[i])
target = int(input())

if bst_root is None:
    print("None")
else:
    result = find_successor(bst_root, target)
    if result is not None:
        print(result)
    else:
        print("None")

```

**풀이에 대한 설명**
`find_successor` 함수는 BST에서 `target_val`의 직후값(successor)을 찾습니다.

1. `successor` 변수를 `None`으로 초기화합니다.
2. 루트에서 시작하여 `current` 노드를 이동시킵니다.
3. 만약 `current.value`가 `target_val`보다 크면, `current.value`는 잠재적인 직후값입니다. `successor`를 `current.value`로 업데이트하고, 더 `target_val`에 가까운 (더 작은) 직후값을 찾기 위해 왼쪽 서브트리로 이동합니다 (`current = current.left`).
4. 만약 `current.value`가 `target_val`보다 작거나 같으면, 직후값은 반드시 오른쪽 서브트리에 있으므로 오른쪽으로 이동합니다 (`current = current.right`).
5. `current`가 `None`이 될 때까지 반복하면, `successor`에는 `target_val`보다 큰 가장 작은 값이 저장됩니다.

---

### ✅ 문제 26. 이진 트리의 모든 경로 출력하기 (루트에서 리프까지)

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 루트에서 시작하여 모든 리프 노드에 도달하는 경로들을 각각 출력하세요. 각 경로는 노드 값들을 "->"로 연결하여 문자열 형태로 출력하며, 각 경로는 한 줄에 하나씩 출력합니다.

**입력 예시**

```
5
A B C
B D E
C . .
D . .
E . .
```

**출력 예시**

```
A->B->D
A->B->E
A->C
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char_candidate = None

for i in range(n_nodes_defined):
    data, left_child_val, right_child_val = input().split()
    if data not in nodes_dict:
        nodes_dict[data] = TreeNode(data)
    if i == 0:
        root_node_char_candidate = data

    nodes_dict[data].temp_left_val = left_child_val
    nodes_dict[data].temp_right_val = right_child_val

for node_char_val in nodes_dict:
    node_obj = nodes_dict[node_char_val]
    left_val = node_obj.temp_left_val
    if left_val != '.' and left_val in nodes_dict:
        node_obj.left = nodes_dict[left_val]
    right_val = node_obj.temp_right_val
    if right_val != '.' and right_val in nodes_dict:
        node_obj.right = nodes_dict[right_val]
# --- 트리 구성 로직 끝 ---

def find_all_paths(node, current_path_nodes, all_paths_list):
    if not node:
        return

    current_path_nodes.append(str(node.value)) # 현재 노드를 경로에 추가

    # 리프 노드에 도달했으면, 현재 경로를 all_paths_list에 추가
    if not node.left and not node.right:
        all_paths_list.append("->".join(current_path_nodes))
    else:
        # 리프가 아니면 왼쪽, 오른쪽 자식으로 계속 탐색
        if node.left:
            find_all_paths(node.left, current_path_nodes, all_paths_list)
        if node.right:
            find_all_paths(node.right, current_path_nodes, all_paths_list)

    current_path_nodes.pop() # 백트래킹: 현재 노드에서 빠져나오기 전에 경로에서 제거

root = nodes_dict.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes_dict else None
all_found_paths = []

if root:
    find_all_paths(root, [], all_found_paths)
    for path_str in all_found_paths:
        print(path_str)
elif n_nodes_defined > 0 and not root: # 노드는 정의되었으나 루트를 못 찾은 경우
    pass # 아무것도 출력 안 함
elif n_nodes_defined == 0: # 노드가 아예 없는 경우
    pass # 아무것도 출력 안 함

```

**풀이에 대한 설명**
`find_all_paths` 함수는 깊이 우선 탐색(DFS)을 변형하여 모든 루트-리프 경로를 찾습니다.

1. `current_path_nodes` 리스트는 현재까지 탐색한 경로의 노드 값들을 저장합니다.
2. 함수가 호출되면 현재 노드의 값을 `current_path_nodes`에 추가합니다.
3. 만약 현재 노드가 리프 노드(왼쪽과 오른쪽 자식이 모두 없음)이면, `current_path_nodes`에 저장된 값들을 "->"로 연결하여 `all_paths_list`에 추가합니다.
4. 현재 노드가 리프 노드가 아니면, 왼쪽 자식과 오른쪽 자식에 대해 재귀적으로 `find_all_paths`를 호출합니다.
5. 재귀 호출에서 돌아온 후에는 `current_path_nodes.pop()`을 호출하여 현재 노드를 경로에서 제거합니다(백트래킹). 이는 다른 경로를 탐색할 때 이전 경로의 영향이 없도록 하기 위함입니다.
   최종적으로 `all_found_paths`에 저장된 모든 경로 문자열을 출력합니다.

---

### ✅ 문제 27. 이진 트리가 대칭인지 확인하기 (Symmetric Tree)

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어질 때, 이 트리가 자신의 중심(루트)을 기준으로 거울 이미지처럼 대칭인지 여부를 `True` 또는 `False`로 출력하세요.

**입력 예시**

```
7
A B B
B C D
B D C
C . .
D . .
```

(위 트리는 A를 루트로, 왼쪽 B(C,D), 오른쪽 B(D,C) 형태여야 대칭)

**예시 (대칭인 경우)**

```
7
A B1 B2
B1 C1 D1
B2 D2 C2
C1 . .
D1 . .
D2 . .
C2 . .
```

(값이 같다고 가정하고 B1, B2 / C1, C2 / D1, D2 가 대칭 위치에 같은 값을 가짐)
**수정된 입력 예시 (대칭, 값 동일 가정)**

```
7
A B B
B C D
B D C
C . .
D . .
# 실제로 입력에서 같은 'B'를 두번 정의할 수 없음. 노드 이름은 유일해야 함.
# 따라서 다른 이름으로 하되, 값이 대칭 위치에서 같도록.
# A L R / L L1 R1 / R L2 R2 where L.val == R.val, L1.val == R2.val, R1.val == L2.val
# 입력 예시를 더 명확하게:
```

**실제 가능한 대칭 입력 예시**

```
7
A L R
L C1 D1
R D2 C2
C1 . .
D1 . .
D2 . .
C2 . .
```

(이 트리가 대칭이려면 A의 왼쪽 자식 L의 값과 오른쪽 자식 R의 값이 같고,
L의 왼쪽 자식 C1의 값과 R의 오른쪽 자식 C2의 값이 같고,
L의 오른쪽 자식 D1의 값과 R의 왼쪽 자식 D2의 값이 같아야 함.)
**문제 단순화를 위해, 값은 무시하고 구조만 대칭인지 확인하는 것으로 가정하고, 값이 같은지는 is_mirror에서 추가 체크.**
**가장 흔한 대칭 트리 문제의 입력 예시 (값도 고려):**

```
7
1 2 2
2 3 4
2 4 3
3 . .
4 . .
```

(위 예시는 숫자로 노드값을 표현, 1을 루트로, (2,3,4)와 (2,4,3)이 대칭)

**출력 예시 (위 숫자 예시에 대해)**

```
True
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 11과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char_candidate = None

# 이 문제에서는 노드 값을 숫자로 가정하고 입력받겠습니다.
# 입력 예시도 숫자로 변경했으므로.
for i in range(n_nodes_defined):
    line_parts = input().split()
    data_val_str = line_parts[0]
    left_child_val_str = line_parts[1]
    right_child_val_str = line_parts[2]

    # 노드 값은 비교를 위해 숫자로 변환 (또는 문자열 그대로 사용하고 is_mirror에서 비교)
    # 여기서는 문자열로 두고, is_mirror에서 값 비교
    if data_val_str not in nodes_dict:
        nodes_dict[data_val_str] = TreeNode(data_val_str)
    if i == 0:
        root_node_char_candidate = data_val_str # 루트 후보

    nodes_dict[data_val_str].temp_left_val = left_child_val_str
    nodes_dict[data_val_str].temp_right_val = right_child_val_str

for node_data_str in nodes_dict:
    node_obj = nodes_dict[node_data_str]
    left_val_str = node_obj.temp_left_val
    if left_val_str != '.' and left_val_str in nodes_dict:
        node_obj.left = nodes_dict[left_val_str]
    right_val_str = node_obj.temp_right_val
    if right_val_str != '.' and right_val_str in nodes_dict:
        node_obj.right = nodes_dict[right_val_str]
# --- 트리 구성 로직 끝 ---

def is_mirror(t1_node, t2_node):
    # 두 노드가 모두 비어있으면 대칭
    if not t1_node and not t2_node:
        return True
    # 한쪽만 비어있거나 값이 다르면 비대칭
    if not t1_node or not t2_node or t1_node.value != t2_node.value:
        return False

    # t1의 왼쪽 자식과 t2의 오른쪽 자식이 대칭인지,
    # t1의 오른쪽 자식과 t2의 왼쪽 자식이 대칭인지 확인
    return is_mirror(t1_node.left, t2_node.right) and \
           is_mirror(t1_node.right, t2_node.left)

def is_symmetric(root_node):
    if not root_node:
        return True # 빈 트리는 대칭으로 간주
    return is_mirror(root_node.left, root_node.right)


root = nodes_dict.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes_dict else None

if not root:
    if n_nodes_defined == 0: # 노드가 아예 없는 경우
        print(True)
    else: # 루트는 없으나 노드가 정의된 경우
        print(False)
else:
    print(is_symmetric(root))

```

**풀이에 대한 설명**
트리가 대칭인지 확인하려면, 루트의 왼쪽 서브트리와 오른쪽 서브트리가 서로 거울 이미지 관계인지 확인해야 합니다.
`is_mirror(t1_node, t2_node)` 함수는 두 개의 노드 `t1`과 `t2`를 받아 이들이 서로 거울 이미지인지 재귀적으로 검사합니다:

1.  `t1`과 `t2`가 모두 `None`이면, `True` (대칭).
2.  `t1`이나 `t2` 중 하나만 `None`이거나, 두 노드의 값이 다르면, `False` (비대칭).
3.  그렇지 않으면 (`t1`과 `t2` 모두 존재하고 값이 같으면), `t1`의 왼쪽 자식과 `t2`의 오른쪽 자식이 서로 거울 이미지인지, 그리고 `t1`의 오른쪽 자식과 `t2`의 왼쪽 자식이 서로 거울 이미지인지 재귀적으로 확인합니다. 두 조건 모두 만족해야 `True`입니다.
    `is_symmetric` 함수는 루트 노드가 주어지면, 루트의 왼쪽 자식과 오른쪽 자식에 대해 `is_mirror`를 호출하여 대칭 여부를 판단합니다. 빈 트리는 대칭으로 간주합니다.

---

### ✅ 문제 28. 주어진 합을 갖는 경로가 이진 트리에 존재하는지 확인하기 (루트에서 리프까지)

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어지고 (노드 값은 정수로 가정), 추가로 목표 합(target_sum) 정수가 주어집니다. 루트에서 시작하여 리프 노드까지 이어지는 경로 중에서 노드 값들의 합이 `target_sum`과 같은 경로가 존재하는지 여부를 `True` 또는 `False`로 출력하세요.

**입력 예시 (노드 값을 숫자로 변경)**

```
7
5 4 8
4 11 .
8 13 4
11 7 2
13 . .
4 . 1
7 . .
2 . .
22
```

(경로 5->4->11->2 합 = 22)

**출력 예시**

```
True
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value): # 값은 정수로 저장
        self.value = int(value)
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 27 숫자 노드값 버전과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char_candidate = None # 실제로는 숫자 문자열이 됨

for i in range(n_nodes_defined):
    line_parts = input().split()
    data_val_str, left_child_val_str, right_child_val_str = line_parts

    if data_val_str not in nodes_dict:
        nodes_dict[data_val_str] = TreeNode(data_val_str) # 정수 변환은 TreeNode 생성자에서
    if i == 0:
        root_node_char_candidate = data_val_str

    nodes_dict[data_val_str].temp_left_val = left_child_val_str
    nodes_dict[data_val_str].temp_right_val = right_child_val_str

for node_data_str in nodes_dict:
    node_obj = nodes_dict[node_data_str]
    left_val_str = node_obj.temp_left_val
    if left_val_str != '.' and left_val_str in nodes_dict:
        node_obj.left = nodes_dict[left_val_str]
    right_val_str = node_obj.temp_right_val
    if right_val_str != '.' and right_val_str in nodes_dict:
        node_obj.right = nodes_dict[right_val_str]
# --- 트리 구성 로직 끝 ---

target_sum = int(input())

def has_path_sum(node, current_sum, required_sum):
    if not node:
        return False

    current_sum += node.value # 현재 노드 값을 더함

    # 현재 노드가 리프 노드이고, current_sum이 required_sum과 같으면 True
    if not node.left and not node.right: # 리프 노드
        return current_sum == required_sum

    # 리프가 아니면 왼쪽 또는 오른쪽 서브트리에서 경로 탐색
    found_in_left = False
    if node.left:
        found_in_left = has_path_sum(node.left, current_sum, required_sum)

    if found_in_left: # 왼쪽에서 찾았으면 더 볼 필요 없음
        return True

    found_in_right = False
    if node.right:
        found_in_right = has_path_sum(node.right, current_sum, required_sum)

    return found_in_right


root = nodes_dict.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes_dict else None

if not root:
    if target_sum == 0 and n_nodes_defined == 0: # 빈 트리에 합 0은 False (경로 없음)
        print(False)
    else:
        print(False)
else:
    print(has_path_sum(root, 0, target_sum))
```

**풀이에 대한 설명**
`has_path_sum` 함수는 재귀적으로 루트-리프 경로 합을 확인합니다.

1.  현재 노드가 `None`이면 `False`를 반환합니다.
2.  `current_sum`에 현재 노드의 값을 더합니다.
3.  현재 노드가 리프 노드(왼쪽과 오른쪽 자식이 모두 없음)이면, `current_sum`이 `required_sum`과 같은지 비교하여 결과를 반환합니다.
4.  현재 노드가 리프 노드가 아니면, 왼쪽 자식에 대해 `has_path_sum(node.left, current_sum, required_sum)`을 호출하고, 오른쪽 자식에 대해 `has_path_sum(node.right, current_sum, required_sum)`을 호출합니다. 두 호출 중 하나라도 `True`를 반환하면 `True`를 반환합니다.
    초기 호출은 루트 노드, 현재 합 0, 목표 합 `target_sum`으로 시작합니다.

---

### ✅ 문제 29. 이진 검색 트리(BST)의 유효성 검사

**설명**
문제 11과 동일한 입력 형식으로 이진 트리가 주어집니다 (노드 값은 정수로 가정). 이 트리가 유효한 이진 검색 트리(BST)인지 여부를 `True` 또는 `False`로 출력하세요.
BST 조건:

- 모든 노드의 왼쪽 서브트리에 있는 값들은 현재 노드의 값보다 작아야 합니다.
- 모든 노드의 오른쪽 서브트리에 있는 값들은 현재 노드의 값보다 커야 합니다.
- 왼쪽과 오른쪽 서브트리 또한 각각 BST여야 합니다. (중복 값은 없다고 가정)

**입력 예시 (유효한 BST)**

```
5
50 30 70
30 20 40
70 60 80
20 . .
40 . .
60 . .
80 . .
```

(입력 라인 수 수정: 7)

**출력 예시**

```
True
```

**풀이 코드**

```python
import math

class TreeNode:
    def __init__(self, value): # 값은 정수로 저장
        self.value = int(value)
        self.left = None
        self.right = None

# --- 트리 구성 로직 (문제 28과 유사) ---
n_nodes_defined = int(input())
nodes_dict = {}
root_node_char_candidate = None

for i in range(n_nodes_defined):
    line_parts = input().split()
    data_val_str, left_child_val_str, right_child_val_str = line_parts

    if data_val_str not in nodes_dict:
        nodes_dict[data_val_str] = TreeNode(data_val_str)
    if i == 0:
        root_node_char_candidate = data_val_str

    nodes_dict[data_val_str].temp_left_val = left_child_val_str
    nodes_dict[data_val_str].temp_right_val = right_child_val_str

for node_data_str in nodes_dict:
    node_obj = nodes_dict[node_data_str]
    left_val_str = node_obj.temp_left_val
    if left_val_str != '.' and left_val_str in nodes_dict:
        node_obj.left = nodes_dict[left_val_str]
    right_val_str = node_obj.temp_right_val
    if right_val_str != '.' and right_val_str in nodes_dict:
        node_obj.right = nodes_dict[right_val_str]
# --- 트리 구성 로직 끝 ---

def is_valid_bst_recursive(node, min_val, max_val):
    if not node:
        return True # 빈 노드는 BST 조건을 만족

    # 현재 노드의 값이 유효한 범위 내에 있는지 확인
    if not (min_val < node.value < max_val):
        return False

    # 왼쪽 서브트리는 (min_val, 현재 노드 값) 범위 내에서 유효해야 함
    # 오른쪽 서브트리는 (현재 노드 값, max_val) 범위 내에서 유효해야 함
    return is_valid_bst_recursive(node.left, min_val, node.value) and \
           is_valid_bst_recursive(node.right, node.value, max_val)


root = nodes_dict.get(root_node_char_candidate) if 'root_node_char_candidate' in locals() and root_node_char_candidate in nodes_dict else None

if not root:
    if n_nodes_defined == 0: # 노드가 아예 없는 경우
        print(True) # 빈 트리는 BST로 간주
    else: # 루트는 없으나 노드가 정의된 경우
        print(False)
else:
    # 초기 범위는 음의 무한대에서 양의 무한대
    print(is_valid_bst_recursive(root, -math.inf, math.inf))
```

**풀이에 대한 설명**
`is_valid_bst_recursive` 함수는 현재 노드와 함께 유효한 값의 범위 (`min_val`, `max_val`)를 인자로 받습니다.

1.  현재 노드가 `None`이면, 유효한 BST의 일부이므로 `True`를 반환합니다.
2.  현재 노드의 값이 `min_val`과 `max_val` 사이에 있지 않으면 (즉, `min_val < node.value < max_val`을 만족하지 않으면), BST 조건 위배이므로 `False`를 반환합니다.
3.  왼쪽 자식에 대해 재귀 호출할 때는 최대값을 현재 노드의 값으로 제한합니다 (`is_valid_bst_recursive(node.left, min_val, node.value)`).
4.  오른쪽 자식에 대해 재귀 호출할 때는 최소값을 현재 노드의 값으로 제한합니다 (`is_valid_bst_recursive(node.right, node.value, max_val)`).
5.  두 재귀 호출이 모두 `True`를 반환해야 유효한 BST입니다.
    초기 호출은 루트 노드와 매우 작은 값(-무한대), 매우 큰 값(+무한대)을 범위로 시작합니다.

---

### ✅ 문제 30. 정렬된 배열/리스트로부터 높이 균형 이진 검색 트리(Height-Balanced BST) 만들기

**설명**
오름차순으로 정렬된 중복 없는 정수 배열이 주어집니다. 이 배열의 원소들을 사용하여 높이 균형을 이루는 이진 검색 트리(BST)를 구성하고, 생성된 트리의 루트 노드 값을 출력하세요. 만약 배열이 비어있으면 "Empty"를 출력합니다.
첫 줄에는 배열의 원소 개수 N (0 <= N <= 100)이 주어집니다. 둘째 줄에는 N개의 정수가 공백으로 구분되어 오름차순으로 주어집니다.

**입력 예시**

```
7
10 20 30 40 50 60 70
```

**출력 예시**

```
40
```

**풀이 코드**

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def sorted_array_to_bst(nums_array):
    if not nums_array:
        return None

    # 배열의 중간 원소를 루트로 선택
    mid_index = len(nums_array) // 2
    root = TreeNode(nums_array[mid_index])

    # 왼쪽 부분 배열로 왼쪽 서브트리 구성
    root.left = sorted_array_to_bst(nums_array[:mid_index])

    # 오른쪽 부분 배열로 오른쪽 서브트리 구성
    root.right = sorted_array_to_bst(nums_array[mid_index+1:])

    return root

num_count = int(input())
if num_count == 0:
    print("Empty")
else:
    sorted_nums = list(map(int, input().split()))

    bst_root = sorted_array_to_bst(sorted_nums)

    if bst_root:
        print(bst_root.value)
    else: # num_count > 0 이지만 sorted_nums가 빈 경우 (문제 조건상 거의 없음)
        print("Empty")

```

**풀이에 대한 설명**
정렬된 배열로부터 높이 균형 BST를 만들기 위해서는 배열의 중간 원소를 루트로 선택하는 전략을 사용합니다.
`sorted_array_to_bst` 함수:

1.  입력 배열 `nums_array`가 비어있으면 `None`을 반환합니다 (서브트리가 없음을 의미).
2.  배열의 중간 인덱스 `mid_index`를 계산합니다.
3.  `nums_array[mid_index]`를 값으로 하는 `TreeNode`를 생성하여 현재 서브트리의 루트로 삼습니다.
4.  루트의 왼쪽 자식은 배열의 왼쪽 부분 (`nums_array[:mid_index]`)에 대해 `sorted_array_to_bst`를 재귀적으로 호출하여 설정합니다.
5.  루트의 오른쪽 자식은 배열의 오른쪽 부분 (`nums_array[mid_index+1:]`)에 대해 `sorted_array_to_bst`를 재귀적으로 호출하여 설정합니다.
6.  생성된 루트 노드를 반환합니다.
    이 과정을 통해 재귀적으로 높이 균형을 이루는 BST가 구성됩니다. 최종적으로 생성된 전체 트리의 루트 노드 값을 출력합니다.
