# Backtracking Problems

## Subsets

- Explain[[Link](https://neetcode.io/courses/advanced-algorithms/11)]
- Time: O(n * 2^n), Space O(n)

### Distinct Element Subset
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        subsets, currset = [], []
        self.get_subsets(0, nums, subsets, currset)
        return subsets
    
    def get_subsets(self, idx, nums, sub, curr):
        if idx >= len(nums):
            sub.append(curr.copy())
            return

        curr.append(nums[idx])
        self.get_subsets(idx + 1, nums, sub, curr)
        curr.pop()
        self.get_subsets(idx + 1, nums, sub, curr)
```

### Subsets - non-distinct elements
```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        subsets, currset = [], []
        self.get_subsets(0, nums, subsets, currset)
        return subsets
    
    def get_subsets(self, idx, nums, sub, curr):
        if idx >= len(nums):
            sub.append(curr.copy())
            return

        curr.append(nums[idx])
        self.get_subsets(idx + 1, nums, sub, curr)
        curr.pop()

        while idx + 1 < len(nums) and nums[idx] == nums[idx + 1]:
            idx += 1
        self.get_subsets(idx + 1, nums, sub, curr)
```

## Combination[[Link](https://neetcode.io/courses/advanced-algorithms/12)]
- Time Complexity: O(k * 2^n)

### 1. Trivial Approach
```python
def combination(n, k):
    combs, curr = [], []
    get_combs(1, curr, combs, n, k)
    return combs

def get_combs(i, curr, combs, n, k):
    if len(curr) == k:
        combs.append(curr.copy())
        return
    if i > n:
        return
    
    curr.appen(i)
    get_combs(i + 1, curr, combs, n, k)
    curr.pop()

    get_combs(i + 1, curr, combs, n, k)
```

### 2. Optimized Approcah
- Time Complexity: O(k * C(n, k))
```python
def combination(n, k):
    combs, curr = [], []
    get_combs(1, curr, combs, n, k)
    return combs

def get_combs(i, curr, combs, n, k):
    if len(curr) == k:
        combs.append(curr.copy())
        return
    if i > n:
        return
    
    for j in range(i, n + 1):
        curr.append(j)
        get_combs(j + 1, curr, combs, n, k)
        curr.pop()
```

## Permutations[[Link](https://neetcode.io/courses/advanced-algorithms/13)]
- Time Complexity: O(n * n!)

### Recursive
```python
def permutations(nums):
    return permutations_helper(0, nums)

def permutations_helper(i, nums):
    if i == len(nums):
        return [[]]
    
    res = []
    perms = permutations_helper(i + 1, nums)
    for p in perms:
        for j in range(len(p) + 1):
            p_copy = p.copy()
            p_copy.insert(j, nums[i])
            res.append(p_copy)
    
    return res
```

```text
The recursive solution is essentially the same as the iterative one in terms of time complexity. While both are bounded below by O(n·n!) due to the size 
of the output, the practical work including list copying makes the complexity closer to O(n²·n!) in the worst-case scenario. So yes, they have 
essentially the same time complexity characteristics.
```
### Iterative
- Time Complexity: O(n^2 * n!)

```python
def permutation(nums):
    perms = [[]]

    for n in nums:
        next_perms = []
        for p in perms:
            for i in range(len(p) + 1):
                p_copy = p.copy()
                p_copy.insert(i, n)
                next_perms.append(p_copy)
        perms = next_perms
    return perms
```

## Some LC Problems
### 77. Combinations[[Link](https://leetcode.com/problems/combinations/description/)]
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        combs, curr = [], []
        self.get_combs(1, curr, combs, n, k)
        return combs
    
    def get_combs(self, i, curr, combs, n, k):
        if len(curr) == k:
            combs.append(curr.copy())
            return 
        if i > n:
            return 
        
        for j in range(i, n + 1):
            curr.append(j)
            self.get_combs(j + 1, curr, combs, n, k)
            curr.pop()
```

### Combination Sum[[Link](https://neetcode.io/problems/combination-target-sum)]

- hint: need a variable `total` to be passed in dfs(i, total) and initialized be 0
```python
class Solution:
    def combinationSum(self, nums: List[int], target: int) -> List[List[int]]:
        combs, curr = [], []
        self.get_combs_sum(0, curr, combs, nums, target, 0)
        return combs
    
    def get_combs_sum(self, i, curr, combs, nums, target, total):
        if total == target:
            combs.append(curr.copy())
            return 
        if i >= len(nums) or total > target:
            return
        
        curr.append(nums[i])
        self.get_combs_sum(i, curr, combs, nums, target, nums[i] + total)
        curr.pop()

        self.get_combs_sum(i + 1, curr, combs, nums, target, total)

# in nested function style:
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        combs, curr = [], []

        def dfs(i, total):
            if total == target:
                combs.append(curr.copy())
                return
            
            if i >= len(candidates) or total > target:
                return
            
            curr.append(candidates[i])
            dfs(i, candidates[i] + total)
            curr.pop()

            dfs(i + 1, total)

        dfs(0, 0)
        return combs
```

### 40. Combination Sum II[[Link](https://leetcode.com/problems/combination-sum-ii/description/)]

- video explaination[[Link](https://neetcode.io/problems/combination-target-sum-ii?list=neetcode150)]

```python
# time: O(n * 2^n); space: O(n)
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        combs, curr = [], []

        def dfs(i, total):
            if total == target:
                combs.append(curr.copy())
                return
            
            if i >= len(candidates) or total > target:
                return
            
            curr.append(candidates[i])
            dfs(i + 1, candidates[i] + total)
            curr.pop()

            while i + 1 < len(candidates) and candidates[i] == candidates[i + 1]:
                i += 1
            dfs(i + 1, total)

        dfs(0, 0)
        return combs
```

### 17. Letter Combinations of a Phone Number[[Link](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/?envType=study-plan-v2&envId=top-interview-150)]

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        # construct a hash tablb
        table = {
            str(k): v 
            for k, v in zip(range(2, 10), ["abc", "def", "ghi", "jkl", "mno", "qprs", "tuv", "wxyz"])
        }
        combs, curr = [], ""
        self.get_letter_combs(0, curr, combs, digits, table)
        return combs
    
    def get_letter_combs(self, i, curr, combs, digits, table):
        if len(curr) == len(digits):
            combs.append(curr)
            return
        
        for letter in table[digits[i]]:
            self.get_letter_combs(i + 1, curr + letter, combs, digits, table)
```

### 46. Permutations[[Link](https://leetcode.com/problems/permutations/description/?envType=study-plan-v2&envId=top-interview-150)]
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return self.perms_helper(0, nums)
    
    def perms_helper(self, i, nums):
        if i == len(nums):
            return [[]]
        
        res = []
        perms = self.perms_helper(i + 1, nums)
        for p in perms:
            for j in range(len(p) + 1):
                p_copy = p.copy()
                p_copy.insert(j, nums[i])
                res.append(p_copy)
        return res

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        
        def dfs(i):
            if i == len(nums):
                return [[]]
            
            res = []
            perms = dfs(i + 1)
            for p in perms:
                for j in range(len(p) + 1):
                    p_copy = p.copy()
                    p_copy.insert(j, nums[i])
                    res.append(p_copy)
            return res
        
        return dfs(0)
```

### *47. Permutations II[[Link](https://leetcode.com/problems/permutations-ii/description/)]
- vids explaination[[Link](https://neetcode.io/solutions/permutations-ii)]
```python
# Time & Space: O(n * n!)
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        visited = [False] * len(nums)
        self.perms_helper(nums, visited, [], res)
        return res
    
    def perms_helper(self, nums, visited, path, res):
        if len(path) == len(nums):
            res.append(path.copy())
            return
        
        for i in range(len(nums)):
            # Skip elements that have already been used in the current permutation.
            if visited[i]:
                continue
            # If the current element is the same as the previous element, and the previous
            # element hasn't been used in this recursion branch, skip to avoid duplicates.
            if i > 0 and nums[i] == nums[i - 1] and not visited[i - 1]:
                continue
            # Mark the current element as used and add it to the path.
            visited[i] = True
            path.append(nums[i])
            self.perms_helper(nums, visited, path, res)
            # Remove the current element from the path and mark it as unused before the next iteration.
            path.pop()
            visited[i] = False

# Less parameters to pass if in nested function
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        visited = [False] * len(nums)

        def dfs(path):
            if len(path) == len(nums):
                res.append(path.copy())
                return

            for i in range(len(nums)):
                if visited[i]:
                    continue
                if i > 0 and nums[i] == nums[i - 1] and not visited[i - 1]:
                    continue

                visited[i] = True
                path.append(nums[i])
                dfs(path)
                path.pop()
                visited[i] = False

        dfs([])
        return res
```

### *79. Word Search[[Link](https://leetcode.com/problems/word-search/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://www.youtube.com/watch?v=pfiQ_PS1g8E&t=18s)]
- hint: recursion on all four directions

```python
# time complexity: O(n * m * dfs-> 4^len(word)) -> O(n * m * 4^n)
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])
        path = set() 

        def dfs(r, c, i):
            if i == len(word):
                return True
            
            if (r < 0 or c < 0 or r >= rows or c >= cols
                or word[i] != board[r][c] or (r, c) in path):
                return False

            
            path.add((r, c))
            res = (
                dfs(r + 1, c, i + 1) or
                dfs(r - 1, c, i + 1) or
                dfs(r, c + 1, i + 1) or
                dfs(r, c - 1, i + 1)
            )
            path.remove((r, c))
            return res
        
        for r in range(rows):
            for c in range(cols):
                if dfs(r, c, 0):
                    return True
        return False
```

### 22. Generate Parentheses[[Link](https://leetcode.com/problems/generate-parentheses/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/generate-parentheses)]
- hint: divide the dfs into recusively do open parentheses and close parentheses

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        combs, curr = [], []

        def dfs(opn_n, cls_n):
            if opn_n == cls_n == n:
                combs.append("".join(curr))
                return
            
            if opn_n < n:
                curr.append('(')
                dfs(opn_n + 1, cls_n)
                curr.pop()
            if cls_n < opn_n:
                curr.append(')')
                dfs(opn_n, cls_n + 1)
                curr.pop()
        dfs(0, 0)
        return combs 
```

### 131. Palindrome Partitioning[[Link](https://leetcode.com/problems/palindrome-partitioning/description/)]

- video explaination[[Link](https://neetcode.io/problems/palindrome-partitioning?list=neetcode150)]

```python
# time: O(n * 2^n); space: O(n) extra space, O(n * 2^n) for the output list
class Solution:
    def _is_palindrome(self, s, l, r):
        while l < r:
            if s[l] != s[r]:
                return False
            l += 1
            r -= 1
        return True

    def partition(self, s: str) -> List[List[str]]:
        res, curr = [], []

        def dfs(i):
            if i >= len(s):
                res.append(curr.copy())
                return
            
            for j in range(i, len(s)):
                if self._is_palindrome(s, i, j):
                    curr.append(s[i: j + 1])
                    dfs(j + 1)
                    curr.pop()
            
        dfs(0)
        return res
```

### 51. N-Queens[[Link](https://leetcode.com/problems/n-queens/description/)]

- video explaination[[Link](https://neetcode.io/problems/n-queens?list=neetcode150)]

```python
# time: O(n!); time: O(n^2)
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        col = set()
        postive_diag = set() # -> (r + c)
        negative_diag = set() # -> (r - c)

        res = []
        board = [["."] * n for _ in range(n)]

        def dfs(r):
            if r == n:
                copy = ["".join(row) for row in board]
                res.append(copy)
                return
            
            for c in range(n):
                if c in col or (r + c) in postive_diag or (r - c) in negative_diag:
                    continue
                col.add(c)
                postive_diag.add(r + c)
                negative_diag.add(r - c)
                board[r][c] = "Q"
                dfs(r + 1)

                # do the cleanup for the next iteration of the loop
                col.remove(c)
                postive_diag.remove(r + c)
                negative_diag.remove(r - c)
                board[r][c] = "."
            
        dfs(0)
        return res
```

### 52. N-Queens II[[Link](https://leetcode.com/problems/n-queens-ii/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/n-queens?list=neetcode150)]

```python
# time: O(n!); time: O(n^2)
class Solution:
    def totalNQueens(self, n: int) -> int:
        col = set()
        neg_diag = set()
        pos_diag = set()
        
        self.res = 0
        def dfs(r):
            if r == n:
                self.res += 1
                return
            
            for c in range(n):
                if c in col or (r + c) in pos_diag or (r - c) in neg_diag:
                    continue
                
                col.add(c)
                neg_diag.add(r - c)
                pos_diag.add(r + c)

                dfs(r + 1)

                col.remove(c)
                neg_diag.remove(r - c)
                pos_diag.remove(r + c)

        dfs(0)
        return self.res
```