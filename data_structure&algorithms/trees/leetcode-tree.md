# Trees in LC

## 700. Search in a Binary Search Tree[[Link](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)]
- vide explaination[[Link](https://neetcode.io/solutions/search-in-a-binary-search-tree)]

```python
# time & space: O(H), Where H is the height of the given tree.
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root or root.val == val:
            return root
        
        return self.searchBST(root.left, val) if val < root.val else self.searchBST(root.right, val)
```

## 701. Insert into a Binary Search Tree[[Link](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)]
- vide explaination[[Link](https://neetcode.io/problems/insert-into-a-binary-search-tree?list=blind75)]

```python
# time & space: O(h)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val)

        if val > root.val:
            root.right = self.insertIntoBST(root.right, val)
        elif val < root.val:
            root.left = self.insertIntoBST(root.left, val)
        
        return root
```

## 450. Delete Node in a BST[[Link](https://leetcode.com/problems/delete-node-in-a-bst/description/)]
- video explaination[[Link](https://neetcode.io/problems/delete-node-in-a-bst?list=blind75)]

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def min_val(self, node):
        curr = node
        while curr and curr.left:
            curr = curr.left
        return curr

    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None
        
        if key > root.val:
            root.right = self.deleteNode(root.right, key)
        elif key < root.val:
            root.left = self.deleteNode(root.left, key)
        else:
            if not root.right:
                return root.left
            elif not root.left:
                return root.right
            else:
                # get the min_node
                min_node = self.min_val(root.right)
                # set the curr node value to min_node's
                root.val = min_node.val
                # at this time, we have two copies of min_node, remove the origin one by recursivly call deleteNode
                root.right = self.deleteNode(root.right, min_node.val)
        return root
```

## 1325. Delete Leaves With a Given Value[[Link](https://leetcode.com/problems/delete-leaves-with-a-given-value/description/)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def removeLeafNodes(self, root: Optional[TreeNode], target: int) -> Optional[TreeNode]:
        if not root:
            return None
        
        root.left = self.removeLeafNodes(root.left, target)
        root.right = self.removeLeafNodes(root.right, target)

        if not root.left and not root.right and root.val == target:
            return None
        
        return root
```

## 94. Binary Tree Inorder Traversal[[Link](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)]
- video explaination[[Link](https://neetcode.io/problems/binary-tree-inorder-traversal?list=blind75)]
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if not node:
                return None
            
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        
        dfs(root)
        return res
```

## 230. Kth Smallest Element in a BST[[Link](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)]
- video explaination[[Link](https://neetcode.io/problems/kth-smallest-integer-in-bst?list=blind75)]

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        res = []

        def dfs(node):
            if not node:
                return None
            
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        
        dfs(root)
        return res[:k][-1]
```

## 105. Construct Binary Tree from Preorder and Inorder Traversal[[Link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/?envType=study-plan-v2&envId=top-interview-150)]
- video explaination[[Link](https://neetcode.io/problems/binary-tree-from-preorder-and-inorder-traversal?list=blind75)]

```python
# time: O(n ^ 2); space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None

        root = TreeNode(preorder[0])
        mid = inorder.index(preorder[0])
        root.left = self.buildTree(preorder[1: mid + 1], inorder[: mid])
        root.right = self.buildTree(preorder[mid + 1: ], inorder[mid + 1: ])
        return root
```

## 106. Construct Binary Tree from Inorder and Postorder Traversal[[Link](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/?envType=study-plan-v2&envId=top-interview-150)]

```python
# time: O(n ^ 2); space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not inorder or not postorder:
            return None
        
        root = TreeNode(postorder[-1])
        mid = inorder.index(postorder[-1])
        root.left = self.buildTree(inorder[: mid], postorder[: mid])
        root.right = self.buildTree(inorder[mid+1:], postorder[mid:-1])
        return root
```

## 226. Invert Binary Tree[[Link](https://leetcode.com/problems/invert-binary-tree/description/?envType=study-plan-v2&envId=top-interview-150)]
- video explaination[[Link](https://neetcode.io/problems/invert-a-binary-tree?list=blind75)]

```python
# time: O(n); Space: O(h)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        
        if root:
            tmp = root.left
            root.left= self.invertTree(root.right)
            root.right = self.invertTree(tmp)
        
        return root
```

## 104. Maximum Depth of Binary Tree[[Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/?envType=study-plan-v2&envId=top-interview-150)]
- video explaination[[Link](https://neetcode.io/problems/depth-of-binary-tree?list=blind75)]

```python
# time: O(n); Space: O(h)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        
        def dfs(node):
            if not node:
                return 0
            
            left = dfs(node.left)
            right = dfs(node.right)

            return 1 + max(left, right)
        
        return dfs(root)
```

## 111. Minimum Depth of Binary Tree[[Link](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)]

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        
        def dfs(node):
            if not node:
                return 0
            
            left = dfs(node.left)
            right = dfs(node.right)

            if not node.left or not node.right:
                return 1 + max(left, right)

            return 1 + min(left, right)
        
        return dfs(root)
```

## 543. Diameter of Binary Tree[[Link](https://leetcode.com/problems/diameter-of-binary-tree/description/)]

- video explaination[[Link](https://neetcode.io/problems/binary-tree-diameter?list=blind75)]

```python
# time: O(n); Space: O(h)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.max_diameter = 0

        def dfs(node):
            if not node:
                return 0
            
            left = dfs(node.left)
            right = dfs(node.right)

            # update max diameter (edge = left + right)
            self.max_diameter = max(self.max_diameter, left + right)

            return 1 + max(left, right) # height of this node
        
        dfs(root)
        return self.max_diameter
```



## 110. Balanced Binary Tree[[Link](https://leetcode.com/problems/balanced-binary-tree/description/)]
- video explaination[[Link](https://neetcode.io/problems/balanced-binary-tree?list=blind75)]

```python
# time: O(n); Space: O(h)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        
        def dfs(node):
            if not node:
                return [True, 0]
            
            left = dfs(node.left)
            right = dfs(node.right)
            is_balanced = left[0] and right[0] and abs(left[1] - right[1]) <= 1
            return [is_balanced, 1 + max(left[1], right[1])]
        
        return dfs(root)[0]
```

## 100. Same Tree[[Link](https://leetcode.com/problems/same-tree/description/)]
- video explaination[[Link](https://neetcode.io/problems/same-binary-tree?list=blind75)]

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        if p and q and p.val == q.val: 
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        return False
```

## 572. Subtree of Another Tree[[Link](https://leetcode.com/problems/subtree-of-another-tree/description/)]
- video explaination[[Link](https://neetcode.io/problems/subtree-of-a-binary-tree?list=blind75)]

```python
# time: O(n *m); space: O(n + m)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        
        def is_same_tree(a, b):
            if not a and not b:
                return True
            if a and b and a.val == b.val:
                return is_same_tree(a.left, b.left) and is_same_tree(a.right, b.right)
            return False
        
        if not root:
            return False
        if not subRoot:
            return True
        
        if is_same_tree(root, subRoot):
            return True
        return (self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot))
```

## 235. Lowest Common Ancestor of a Binary Search Tree[[Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)]
- video explaination[[Link](https://neetcode.io/problems/lowest-common-ancestor-in-binary-search-tree?list=blind75)]

```python
# time: O(h); space: O(1)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        curr = root

        while curr:
            if p.val < curr.val and q.val < curr.val:
                curr = curr.left
            elif p.val > curr.val and q.val > curr.val:
                curr = curr.right
            else:
                return curr

# or
# time & space: O(h)
class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root

# or
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        
        def dfs(node, p, q):
            if not node:
                return None
            if p == node or q == node:
                return node
            
            left = dfs(node.left, p, q)
            right = dfs(node.right, p, q)

            if left and right:
                return node
            else:
                return left or right
        
        return dfs(root, p, q)
```

## 236. Lowest Common Ancestor of a Binary Tree[[Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)]
- video explaination[[Link](https://www.youtube.com/watch?v=WO1tfq2sbsI)]
- only deal with three cases

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return None
        if root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root # p and q found in different subtree
        else:
            return left or right # one node found in subtree
```

## 102. Binary Tree Level Order Traversal[[Link](https://leetcode.com/problems/binary-tree-level-order-traversal/description/?envType=study-plan-v2&envId=top-interview-150)]
- video explaination[[Link](https://neetcode.io/problems/level-order-traversal-of-binary-tree?list=blind75)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
            
        res = []
        q = deque()
        q.append(root)

        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            res.append(level)
        
        return res
```

## 199. Binary Tree Right Side View[[Link](https://leetcode.com/problems/binary-tree-right-side-view/description/?envType=study-plan-v2&envId=top-interview-150)]
- video explaination[[Link](https://neetcode.io/problems/binary-tree-right-side-view?list=blind75)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        q = deque()
        q.append(root)

        while q:
            right_side = None
            for _ in range(len(q)):
                node = q.popleft() 
                if node:
                    right_side = node
                    q.append(node.left)
                    q.append(node.right)
            if right_side:
                res.append(right_side.val)
        
        return res
```

## *993. Cousins in Binary Tree[[Link](https://leetcode.com/problems/cousins-in-binary-tree/description/)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isCousins(self, root: Optional[TreeNode], x: int, y: int) -> bool:
        if not root:
            return False
        
        q = deque()
        q.append((root, None)) # store (node, parent)
        
        while q:
            found = {} # the hashmap to build up the relationship between current level node and parent node
            for _ in range(len(q)):
                node, parent = q.popleft()
                if node.val == x or node.val == y:
                    found[node.val] = parent
                if node.left:
                    q.append((node.left, node))
                if node.right:
                    q.append((node.right, node))

            if x in found and y in found:
                return found[x] != found[y] # return true if two parents are not the same
            if x in found or y in found:
                return False
```

## *1448. Count Good Nodes in Binary Tree[[Link](https://leetcode.com/problems/count-good-nodes-in-binary-tree/description/)]
- video explaination[[Link](https://neetcode.io/problems/count-good-nodes-in-binary-tree?list=blind75)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def goodNodes(self, root: TreeNode) -> int:

        def dfs(node, max_val):
            if not node:
                return 0
            
            count = 1 if node.val >= max_val else 0 # means for every node, you deserve to get value 1 if your node val is larger than the current max val
            max_val = max(max_val, node.val)
            count += dfs(node.left, max_val)
            count += dfs(node.right, max_val)
            return count
        
        return dfs(root, root.val)

```

## 98. Validate Binary Search Tree[[Link](https://leetcode.com/problems/validate-binary-search-tree/description/)]

- video explaination[[Link](https://neetcode.io/problems/valid-binary-search-tree?list=blind75)]

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        
        def dfs(node, res):
            if node:
                dfs(node.left, res)
                res.append(node.val)
                dfs(node.right, res)
        
        res = []
        dfs(root, res)
        for i in range(1, len(res)):
            if res[i] <= res[i - 1]:
                return False
        
        return True
```

## 124. Binary Tree Maximum Path Sum[[Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/?envType=study-plan-v2&envId=top-interview-150)] 
- video explaination[[Link](https://neetcode.io/problems/binary-tree-maximum-path-sum?list=blind75)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.max_sum = root.val

        def dfs(node):
            if not node:
                return 0

            left = dfs(node.left) 
            right = dfs(node.right)
            left = max(left, 0)
            right = max(right, 0)
            # Candidate max path sum using both children + current node
            self.max_sum = max(self.max_sum, node.val + left + right)
            # Only return one-side path to parent
            return node.val + max(left, right)
        
        dfs(root)
        return self.max_sum
```

## 112. Path Sum[[Link](https://leetcode.com/problems/path-sum/description/)]

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False

        targetSum -= root.val
        if not root.left and not root.right:
            return targetSum == 0
        
        return self.hasPathSum(root.left, targetSum) or self.hasPathSum(root.right, targetSum)
```

## 113. Path Sum II[[Link](https://leetcode.com/problems/path-sum-ii/description/)]

```python
# time: O(n); space: O(h)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        res, curr = [], []

        def dfs(node, targetSum):
            if not node:
                return

            curr.append(node.val)
            targetSum -= node.val

            if not node.left and not node.right and targetSum == 0:
                res.append(curr.copy())
            else:
                dfs(node.left, targetSum)
                dfs(node.right, targetSum)

            curr.pop()
        
        dfs(root, targetSum)
        return res
                
```

## 437. Path Sum III[[Link](https://leetcode.com/problems/path-sum-iii/description/?envType=study-plan-v2&envId=leetcode-75)]

- hint: prefix sum
- related idea: subarray sum equals k

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        prefix_sums = defaultdict(int) # prefix sum -> how many times a given prefix sum has occurred
        prefix_sums[0] = 1 # base case: empty path sums to 0

        def dfs(node, curr_sum):
            if not node:
                return 0
            
            curr_sum += node.val
            count = prefix_sums[curr_sum - targetSum]

            prefix_sums[curr_sum] += 1 # Add this path sum
            count += dfs(node.left, curr_sum) # Explore left subtree
            count += dfs(node.right, curr_sum) # Explore right subtree
            prefix_sums[curr_sum] -= 1 # backtrack

            return count
        
        return dfs(root, 0)
```
---

### 🔧 Step 1: Visualize the Tree

```plaintext
            10
          /    \
        5       -3
      /   \        \
     3     2       11
    / \     \
   3  -2     1
```

---

### 🔍 Goal:

Count the number of **downward paths** that sum to `8`.

### ✅ Valid Paths:

1. `5 → 3`
2. `5 → 2 → 1`
3. `-3 → 11`

---

Now let’s trace the prefix sum logic step by step.

We use:

```python
prefix_sums = defaultdict(int)
prefix_sums[0] = 1  # Base case
```

This means: if `curr_sum - targetSum = 0`, there's a valid path from root to here.

---

## 🧭 Step-by-step DFS traversal

### ➤ At node 10:

```python
curr_sum = 0 + 10 = 10
prefix_sums[10] += 1 → {0:1, 10:1}
```

Check: `10 - 8 = 2 → prefix_sums[2] = 0` → no path yet.

### ➤ Go left to node 5:

```python
curr_sum = 10 + 5 = 15
prefix_sums[15] += 1 → {0:1, 10:1, 15:1}
```

Check: `15 - 8 = 7 → prefix_sums[7] = 0` → no match yet.

### ➤ Go left to node 3:

```python
curr_sum = 15 + 3 = 18
prefix_sums[18] += 1
Check: 18 - 8 = 10 → prefix_sums[10] = 1 ✅ MATCH!
→ count = 1 (path: 5 → 3)
```

### ➤ Go left to node 3 (leaf):

```python
curr_sum = 18 + 3 = 21
21 - 8 = 13 → no match
backtrack → prefix_sums[21] -= 1
```

### ➤ Go right to node -2:

```python
curr_sum = 18 + (-2) = 16
16 - 8 = 8 → no match
backtrack → prefix_sums[16] -= 1
```

Backtrack → prefix\_sums\[18] -= 1

---

### ➤ Go right to node 2:

```python
curr_sum = 15 + 2 = 17
prefix_sums[17] += 1
17 - 8 = 9 → no match
```

### ➤ Go right to node 1:

```python
curr_sum = 17 + 1 = 18
18 - 8 = 10 → prefix_sums[10] = 1 ✅ MATCH!
→ count += 1 (path: 5 → 2 → 1)
```

Backtrack → prefix\_sums\[18] -= 1
Backtrack → prefix\_sums\[17] -= 1
Backtrack → prefix\_sums\[15] -= 1
Backtrack → prefix\_sums\[10] still = 1

---

### ➤ Go right to node -3:

```python
curr_sum = 10 + (-3) = 7
7 - 8 = -1 → no match
prefix_sums[7] += 1
```

### ➤ Go right to node 11:

```python
curr_sum = 7 + 11 = 18
18 - 8 = 10 → prefix_sums[10] = 1 ✅ MATCH!
→ count += 1 (path: -3 → 11)
```

Backtrack → prefix\_sums\[18] -= 1
Backtrack → prefix\_sums\[7] -= 1
Backtrack → prefix\_sums\[10] -= 1

---

## ✅ Final `count = 3`

Paths found:

1. `5 → 3`
2. `5 → 2 → 1`
3. `-3 → 11`

---

## 🧠 Key Learning:

The prefix sum map helps you answer:
**"How many previous prefix sums exist such that subtracting them gives me the target?"**

And `+1`/`-1` keeps that map clean per path — so you **don’t leak state between branches**.



## *297. Serialize and Deserialize Binary Tree[[Link](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)]

- video explaination[[Link](https://neetcode.io/problems/serialize-and-deserialize-binary-tree?list=blind75)]
- use iter(), next() for deserialize

```python
# time & space: O(n)
# use "," to seperate node, use "#" to indicate null
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        res = []

        def dfs(node):
            if not node:
                res.append("#")
                return
            
            res.append(str(node.val))
            dfs(node.left)
            dfs(node.right)
        
        dfs(root)
        return ",".join(res)

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        values = iter(data.split(","))

        def dfs():
            value = next(values)
            if value == "#":
                return None
            node = TreeNode(int(value))
            node.left = dfs()
            node.right = dfs()
            return node
        
        return dfs()
        

# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```

## 684. Redundant Connection[[Link](https://leetcode.com/problems/redundant-connection/description/)]

- video explaination[[Link](https://neetcode.io/problems/redundant-connection?list=blind75)]
```python
# union find disjoint set

class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        parent = {}
        rank = {}

        for i in range(1, len(edges) + 1):
            parent[i] = i
            rank[i] = 0
        
        def find(n):
            p = parent[n]
            while p != parent[p]:
                parent[p] = parent[parent[p]]
                p = parent[p]
            return p
        
        def union(n1, n2):
            p1, p2 = find(n1), find(n2)
            if p1 == p2:
                return False

            if rank[p1] > rank[p2]:
                parent[p2] = p1
            elif rank[p1] < rank[p2]:
                parent[p1] = p2
            else:
                parent[p2] = p1
                rank[p1] += 1
            return True
        
        for n1, n2 in edges:
            if not union(n1, n2):
                return [n1, n2]

# dfs cycle detection
# time: O(E * (V + E)); space: O(V + E)
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        adj = [[] for _ in range(n + 1)] # or adj = defaultdict(list)

        def dfs(node, par):
            if visit[node]:
                return True
            
            visit[node] = True
            for nei in adj[node]:
                if nei == par:
                    continue
                if dfs(nei, node):
                    return True
            return False
        
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
            visit = [False] * (n + 1)
            
            if dfs(u, -1):
                return [u, v]
        return []
```

## 721. Accounts Merge[[Link](https://leetcode.com/problems/accounts-merge/description/)]

- video explaination[[Link](https://neetcode.io/problems/accounts-merge?list=neetcode150)]

```python
# time: O((n * m)log(n * m)); space: O(n * m)
class UnionFind:
    def __init__(self, n):
        self.parent = {}
        self.rank = {}

        for i in range(n):
            self.parent[i] = i
            self.rank[i] = 0
    
    def find(self, n):
        p = self.parent[n]
        while p != self.parent[p]:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p
    
    def union(self, n1, n2):
        p1, p2 = self.find(n1), self.find(n2)
        if p1 == p2:
            return False
        
        if self.rank[p1] > self.rank[p2]:
            self.parent[p2] = p1
        elif self.rank[p1] < self.rank[p2]:
            self.parent[p1] = p2
        else:
            self.parent[p2] = p1
            self.rank[p1] += 1
        return True

class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        uf = UnionFind(len(accounts))
        email_to_account = {} # email -> index of account

        for i, account in enumerate(accounts):
            for e in account[1:]:
                if e in email_to_account:
                    uf.union(i, email_to_account[e])
                else:
                    email_to_account[e] = i
        
        email_group = defaultdict(list)  # index of account -> list of emails
        for e, i in email_to_account.items():
            leader = uf.find(i)
            email_group[leader].append(e)
        
        res = []
        for i, email in email_group.items():
            name = accounts[i][0]
            res.append([name] + sorted(email))
        
        return res

# approch 2: DFS
class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        graph = defaultdict(list)
        email_to_name = {}

        # Step 1: Build the graph and email-to-name map
        for account in accounts:
            name = account[0]
            first_email = account[1]
            for email in account[1:]:
                graph[first_email].append(email)
                graph[email].append(first_email)
                email_to_name[email] = name

        # Step 2: DFS to find connected components (i.e., merged accounts)
        visited = set()
        res = []

        def dfs(email, component):
            visited.add(email)
            component.append(email)
            for neighbor in graph[email]:
                if neighbor not in visited:
                    dfs(neighbor, component)

        for email in graph:
            if email not in visited:
                component = []
                dfs(email, component)
                res.append([email_to_name[email]] + sorted(component))

        return res
```


## 323. Number of Connected Components in an Undirected Graph[[Link](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/description/)]

- video explaination[[Link](https://neetcode.io/problems/count-connected-components?list=neetcode150)]

```python
# time: O(V+(E∗α(V))); space: O(V)
class UnionFind:
    def __init__(self, n):
        self.parent = {}
        self.rank = {}

        for i in range(n):
            self.parent[i] = i
            self.rank[i] = 0
    
    def find(self, n):
        p = self.parent[n]
        while p != self.parent[p]:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p
    
    def union(self, n1, n2):
        p1, p2 = self.find(n1), self.find(n2)
        if p1 == p2:
            return False
        
        if self.rank[p1] > self.rank[p2]:
            self.parent[p2] = p1
        elif self.rank[p1] < self.rank[p2]:
            self.parent[p1] = p2
        else:
            self.parent[p2] = p1
            self.rank[p1] += 1
        return True

class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        uf = UnionFind(n)
        res = n
        for u, v in edges:
            if uf.union(u, v):
                res -= 1
        return res
```

## 307. Range Sum Query - Mutable[[Link](https://leetcode.com/problems/range-sum-query-mutable/description/)]

- explaination[[Link](https://neetcode.io/solutions/range-sum-query-mutable)]

```python

```

## 427. Construct Quad Tree[[Link](https://leetcode.com/problems/construct-quad-tree/description/)]

- video explaination[[Link](https://neetcode.io/problems/construct-quad-tree?list=neetcode250)]

```python
# time: O(n^2 log n); space: O(log n)
"""
# Definition for a QuadTree node.
class Node:
    def __init__(self, val, isLeaf, topLeft, topRight, bottomLeft, bottomRight):
        self.val = val
        self.isLeaf = isLeaf
        self.topLeft = topLeft
        self.topRight = topRight
        self.bottomLeft = bottomLeft
        self.bottomRight = bottomRight
"""

class Solution:
    def construct(self, grid: List[List[int]]) -> 'Node':
        
        def dfs(n, r, c):
            all_same = True
            for i in range(n):
                for j in range(n):
                    if grid[r][c] != grid[r + i][c + j]:
                        all_same = False
                        break
            # base case
            if all_same:
                return Node(grid[r][c], True)
            
            # construct quad node subtree
            n = n // 2
            topleft = dfs(n, r, c)
            topright = dfs(n, r, c + n)
            bottomleft = dfs(n, r + n, c)
            bottomright = dfs(n, r + n, c + n)

            return Node(0, False, topleft, topright, bottomleft, bottomright)
        
        return dfs(len(grid), 0, 0)

```

337. ## House Robber III[[Link](https://leetcode.com/problems/house-robber-iii/description/)]

- video explaination[[Link](https://neetcode.io/problems/house-robber-iii?list=neetcode250)]

```python
# time & space: O(n)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        
        def dfs(node):
            if not node:
                return [0, 0]
            
            left_pair = dfs(node.left)
            right_pair = dfs(node.right)

            with_root = node.val + left_pair[1] + right_pair[1]
            without_root = max(left_pair) + max(right_pair)

            return [with_root, without_root]
        
        return max(dfs(root))

```