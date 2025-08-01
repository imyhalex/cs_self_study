# Tries (Preifix Tree)[[Link](https://neetcode.io/courses/advanced-algorithms/6)]

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = False # end of word

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        curr = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.word = True

    def search(self, word):
        curr = self.root
        for c in word:
            if c not in curr.children:
                return False
            curr = curr.children[c]
        return curr.word

    def startsWith(self, prefix):
        curr = self.root
        for c in prefix:
            if c not in curr.children:
                return False
            curr = curr.children[c]
        return True

```

## 208. Implement Trie (Prefix Tree)[[Link](https://leetcode.com/problems/implement-trie-prefix-tree/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/implement-prefix-tree?list=blind75)]

```python
class TireNode:
    def __init__(self):
        self.children = {}
        self.word = False

class Trie:

    def __init__(self):
        self.root = TireNode()

    def insert(self, word: str) -> None:
        curr = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = TireNode()
            curr = curr.children[c]
        curr.word = True

    def search(self, word: str) -> bool:
        curr = self.root
        for c in word:
            if c not in curr.children:
                return False
            curr = curr.children[c]
        return curr.word

    def startsWith(self, prefix: str) -> bool:
        curr = self.root
        for c in prefix:
            if c not in curr.children:
                return False
            curr = curr.children[c]
        return True


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

## 1804. Implement Trie II (Prefix Tree)[[Link](https://leetcode.com/problems/implement-trie-ii-prefix-tree/description/)]

- video expalination[[Link]()]

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = False
        self.count = 0
        self.prefix_count = 0

class Trie:

    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        curr = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
            curr.prefix_count += 1
        curr.word = True
        curr.count += 1

    def countWordsEqualTo(self, word: str) -> int:
        curr = self.root
        for c in word:
            if c not in curr.children:
                return 0
            curr = curr.children[c]
        return curr.count

    def countWordsStartingWith(self, prefix: str) -> int:
        curr = self.root
        for c in prefix:
            if c not in curr.children:
                return 0
            curr = curr.children[c]
        return curr.prefix_count

    def erase(self, word: str) -> None:
        curr = self.root
        track = []
        for c in word:
            if c not in curr.children: # means word not exist in prefix tree
                return
            track.append((curr, c)) # curr node, character pair
            curr = curr.children[c]
        
        if curr.count > 0:
            curr.count -= 1
            if curr.count == 0:
                curr.word = False
            
        for node, char in track:
            node.children[char].prefix_count -= 1


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.countWordsEqualTo(word)
# param_3 = obj.countWordsStartingWith(prefix)
# obj.erase(word)
```

## 211. Design Add and Search Words Data Structure[[Link](https://leetcode.com/problems/design-add-and-search-words-data-structure/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/design-word-search-data-structure?list=blind75)]

```python
# time: O(n) for both two methods; space: O(t + n)
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = False

class WordDictionary:

    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        curr = self.root
        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.word = True

    def search(self, word: str) -> bool:

        def dfs(j, node):
            curr = node

            for i in range(j, len(word)):
                c = word[i]
                if c == ".":
                    for child in curr.children.values():
                        if dfs(i + 1, child):
                            return True
                    return False
                else:
                    if c not in curr.children:
                        return False
                    curr = curr.children[c] # case: if the character does exsit, shift to next
            return curr.word

        return dfs(0, self.root)


# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```

## Word Search II[[Link](https://leetcode.com/problems/word-search-ii/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/search-for-word-ii?list=blind75)]

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.eow = False

    def add_word(self, word):
        curr = self
        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.eow = True  

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        root = TrieNode()
        for w in words:
            root.add_word(w) 
        
        rows, cols = len(board), len(board[0])
        res, visited = set(), set()
        def dfs(r, c, node, word):
            if (r < 0 or c < 0 or r == rows or c == cols or 
                (r, c) in visited or board[r][c] not in node.children):
                return 
            
            visited.add((r, c))
            node = node.children[board[r][c]]
            word += board[r][c]
            if node.eow:
                res.add(word)
            dfs(r + 1, c, node, word)
            dfs(r - 1, c, node, word)
            dfs(r, c + 1, node, word)
            dfs(r, c - 1, node, word)
            visited.remove((r, c))
        
        for r in range(rows):
            for c in range(cols):
                dfs(r, c, root, "")

        return list(res)
```

## *2707. Extra Characters in a String[[Link](https://leetcode.com/problems/extra-characters-in-a-string/description/)]

- video explaination[[Link](https://neetcode.io/problems/extra-characters-in-a-string?list=neetcode250)]

```python
# time: O(n ^ 3 + m * K); space: O(m + m * k)
class Solution:
    def minExtraChar(self, s: str, dictionary: List[str]) -> int:
        words = set(dictionary)
        dp = {}

        def dfs(i):
            if i == len(s):
                return 0
            if i in dp:
                return dp[i]

            # res = 1 + dfs(i + 1) # skip curr char + initial value
            res = 1 + dfs(i + 1)
            for j in range(i, len(s)):
                if s[i: j + 1] in words:
                    res = min(res, dfs(j + 1))
            dp[i] = res # store result in cache
            return res
        
        return dfs(0)
```