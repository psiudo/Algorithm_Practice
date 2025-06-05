# 🟦 Trie(트라이) 유형 문제집

접두사 탐색, 문자열 자동완성, 집합 표현 등  
Trie를 직접 구현하며 실전 적응력을 높이는 드릴용 10선

---

## ✅ 문제 1. 문자열 집합 포함 여부

**설명**: N개의 문자열을 Trie에 저장한 뒤, M개의 문자열이 그 집합에 존재하는지 1/0으로 출력하라.

**입력**
```
5 3
hello
hi
he
hero
her
hi
heroic
her
```

**출력**
```
1
0
1
```

**풀이**
```python
class TrieNode:
    def __init__(self):
        self.children = dict()
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True
    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end

n, m = map(int, input().split())
trie = Trie()
for _ in range(n):
    trie.insert(input())
for _ in range(m):
    print(1 if trie.search(input()) else 0)
```

---

## ✅ 문제 2. 접두사 여부 검사

**설명**: 단어가 사전에 접두사로만 존재하는지 검사 (단어 전체가 아니라 접두사로만 끝남)

**입력**
```
4
app
apple
apex
apply
3
ap
app
appl
```

**출력**
```
1
1
1
```

**풀이**
```python
class TrieNode:
    def __init__(self):
        self.children = {}
    def insert(self, word):
        node = self
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
    def startswith(self, prefix):
        node = self
        for ch in prefix:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True

root = TrieNode()
n = int(input())
for _ in range(n):
    root.insert(input())
q = int(input())
for _ in range(q):
    print(1 if root.startswith(input()) else 0)
```

---

## ✅ 문제 3. 자동완성 가능한 단어 수

**설명**: 사전에 등록된 단어 중 접두사를 입력했을 때 자동완성으로 가능한 단어 수를 출력하라.

**입력**
```
5
dog
dot
doll
dorm
door
2
do
dor
```

**출력**
```
4
1
```

**풀이**
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.count = 0
        self.is_end = False
    def insert(self, word):
        node = self
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
            node.count += 1
        node.is_end = True
    def count_prefix(self, prefix):
        node = self
        for ch in prefix:
            if ch not in node.children:
                return 0
            node = node.children[ch]
        return node.count

trie = TrieNode()
n = int(input())
for _ in range(n):
    trie.insert(input())
q = int(input())
for _ in range(q):
    print(trie.count_prefix(input()))
```

---

## ✅ 문제 4. 문자열 접두사 유일 여부 판별

**설명**: 입력된 문자열 중 접두사가 다른 단어에 포함되는 경우가 있으면 0, 없으면 1 출력

**입력**
```
3
dog
door
do
```

**출력**
```
0
```

**풀이**
```python
def check_consistency(words):
    trie = {}
    for word in words:
        node = trie
        for ch in word:
            if 'end' in node:
                return 0
            node = node.setdefault(ch, {})
        if node:
            return 0
        node['end'] = True
    return 1

n = int(input())
words = [input() for _ in range(n)]
print(check_consistency(words))
```

---

## ✅ 문제 5. 접두사 공통 길이 합

**설명**: 단어 N개가 주어질 때, 각 단어를 입력할 때까지 필요한 최소 글자 수의 합을 구하라.

**입력**
```
3
go
gone
guild
```

**출력**
```
7
```

**풀이**
```python
class Node:
    def __init__(self):
        self.children = dict()
        self.count = 0

class Trie:
    def __init__(self):
        self.root = Node()
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, Node())
            node.count += 1
    def search_depth(self, word):
        node = self.root
        for i, ch in enumerate(word):
            node = node.children[ch]
            if node.count == 1:
                return i + 1
        return len(word)

n = int(input())
words = [input() for _ in range(n)]
trie = Trie()
for word in words:
    trie.insert(word)
print(sum(trie.search_depth(w) for w in words))
```

---

## ✅ 문제 6. 문자열 목록 중 접두사 아닌 것 세기

**설명**: 주어진 단어들 중, 다른 단어의 접두사가 아닌 단어의 개수를 출력하라.

**입력**
```
5
go
gone
goggles
gonewild
g
```

**출력**
```
1
```

**풀이**
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False
    def insert(self, word):
        node = self
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True
    def is_not_prefix(self, word):
        node = self
        for ch in word:
            node = node.children[ch]
        return not node.children  # 접두사인 경우 children이 존재

n = int(input())
words = [input() for _ in range(n)]
root = TrieNode()
for word in words:
    root.insert(word)
print(sum(root.is_not_prefix(w) for w in words))
```

---

## ✅ 문제 7. 최대 접두사 일치 길이

**설명**: 쿼리로 주어지는 문자열이 사전에 등록된 단어들과 얼마나 길게 접두사로 겹치는지 출력하라.

**입력**
```
3
cat
cap
camera
3
camp
cattle
dog
```

**출력**
```
3
3
0
```

**풀이**
```python
class Node:
    def __init__(self):
        self.children = {}

class Trie:
    def __init__(self):
        self.root = Node()
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, Node())
    def match_length(self, word):
        node = self.root
        length = 0
        for ch in word:
            if ch in node.children:
                node = node.children[ch]
                length += 1
            else:
                break
        return length

n = int(input())
trie = Trie()
for _ in range(n):
    trie.insert(input())
q = int(input())
for _ in range(q):
    print(trie.match_length(input()))
```

---

## ✅ 문제 8. Trie에 존재하는 단어 개수

**설명**: Trie에 삽입된 단어 중 특정 문자열이 정확히 몇 번 들어 있는지 출력하라.

**입력**
```
5
apple
apple
apply
app
apple
3
apple
app
appl
```

**출력**
```
3
1
0
```

**풀이**
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.end_count = 0
    def insert(self, word):
        node = self
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.end_count += 1
    def count_exact(self, word):
        node = self
        for ch in word:
            if ch not in node.children:
                return 0
            node = node.children[ch]
        return node.end_count

n = int(input())
root = TrieNode()
for _ in range(n):
    root.insert(input())
q = int(input())
for _ in range(q):
    print(root.count_exact(input()))
```

---

## ✅ 문제 9. 문자열 집합 중 특정 접미사 포함 여부 (역방향 트라이)

**설명**: Trie를 역으로 구성하여, 특정 접미사를 포함하는 단어가 있는지 검사하라.

**입력**
```
4
tion
action
fiction
station
3
ion
act
cat
```

**출력**
```
1
1
0
```

**풀이**
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False
    def insert(self, word):
        node = self
        for ch in reversed(word):
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True
    def has_suffix(self, suffix):
        node = self
        for ch in reversed(suffix):
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True

n = int(input())
root = TrieNode()
for _ in range(n):
    root.insert(input())
q = int(input())
for _ in range(q):
    print(1 if root.has_suffix(input()) else 0)
```

---

## ✅ 문제 10. 트라이로 문자열 정렬하기

**설명**: 트라이에 문자열을 삽입한 후, 사전순으로 정렬된 모든 문자열을 출력하라.

**입력**
```
5
bat
ball
baby
base
bag
```

**출력**
```
baby
bag
ball
base
bat
```

**풀이**
```python
class Node:
    def __init__(self):
        self.children = {}
        self.is_end = False
    def insert(self, word):
        node = self
        for ch in word:
            node = node.children.setdefault(ch, Node())
        node.is_end = True
    def dfs(self, path, result):
        if self.is_end:
            result.append("".join(path))
        for ch in sorted(self.children):
            self.children[ch].dfs(path + [ch], result)

n = int(input())
words = [input() for _ in range(n)]
root = Node()
for word in words:
    root.insert(word)
result = []
root.dfs([], result)
for w in result:
    print(w)
```



