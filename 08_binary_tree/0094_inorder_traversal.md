# 二叉树基础题合集

## 94. 二叉树的中序遍历 (Binary Tree Inorder Traversal)

### 递归法

```python
class Solution:
    def inorderTraversal(self, root) -> list[int]:
        result = []
        def dfs(node):
            if not node:
                return
            dfs(node.left)
            result.append(node.val)
            dfs(node.right)
        dfs(root)
        return result
```

### 迭代法（用栈模拟递归）

```python
class Solution:
    def inorderTraversal(self, root) -> list[int]:
        result = []
        stack = []
        curr = root
        
        while curr or stack:
            # 一直走到最左
            while curr:
                stack.append(curr)
                curr = curr.left
            
            curr = stack.pop()
            result.append(curr.val)
            curr = curr.right
        
        return result
```

**关键要点：** 中序遍历 = 左→根→右。迭代法用栈模拟，先把所有左子节点入栈。

---

## 104. 二叉树的最大深度 (Maximum Depth of Binary Tree)

```python
class Solution:
    def maxDepth(self, root) -> int:
        if not root:
            return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```

**一行递归**，最经典的树递归入门题。

---

## 226. 翻转二叉树 (Invert Binary Tree)

```python
class Solution:
    def invertTree(self, root):
        if not root:
            return None
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

**关键：** 交换左右子树，然后递归处理。

---

## 101. 对称二叉树 (Symmetric Tree)

### 思路

判断一棵树是否对称 = 判断它的左右子树是否**镜像对称**。

两棵子树镜像对称的条件：
1. 根值相等
2. A 的左子树与 B 的右子树镜像对称
3. A 的右子树与 B 的左子树镜像对称

```python
class Solution:
    def isSymmetric(self, root) -> bool:
        def check(a, b):
            if not a and not b:
                return True
            if not a or not b:
                return False
            return a.val == b.val and check(a.left, b.right) and check(a.right, b.left)
        
        return check(root.left, root.right)
```

---

## 543. 二叉树的直径 (Diameter of Binary Tree)

### 思路

直径 = 经过某个节点的**左深度 + 右深度**的最大值。

```python
class Solution:
    def diameterOfBinaryTree(self, root) -> int:
        self.ans = 0
        
        def depth(node):
            if not node:
                return 0
            left = depth(node.left)
            right = depth(node.right)
            self.ans = max(self.ans, left + right)
            return 1 + max(left, right)
        
        depth(root)
        return self.ans
```

**关键：** 在求深度的同时，更新直径。直径不一定过根节点！

---

## 102. 二叉树的层序遍历 (Binary Tree Level Order Traversal)

### BFS

```python
from collections import deque

class Solution:
    def levelOrder(self, root) -> list[list[int]]:
        if not root:
            return []
        
        result = []
        queue = deque([root])
        
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            result.append(level)
        
        return result
```

**关键：** `for _ in range(len(queue))` 确保每次处理完整的一层。

---

## 108. 将有序数组转换为二叉搜索树 (Convert Sorted Array to BST)

```python
class Solution:
    def sortedArrayToBST(self, nums):
        if not nums:
            return None
        
        mid = len(nums) // 2
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])
        
        return root
```

**关键：** 选中间元素作为根，保证高度平衡。

---

## 98. 验证二叉搜索树 (Validate BST)

### 思路

BST 的性质：每个节点的值必须在一个**范围 (lower, upper)** 内。

```python
class Solution:
    def isValidBST(self, root) -> bool:
        def validate(node, lower=float('-inf'), upper=float('inf')):
            if not node:
                return True
            if node.val <= lower or node.val >= upper:
                return False
            return validate(node.left, lower, node.val) and \
                   validate(node.right, node.val, upper)
        
        return validate(root)
```

**关键：** 传递上下界，而不是只比较父子关系。

---

## 230. 二叉搜索树中第 K 小的元素 (Kth Smallest Element in BST)

**BST 的中序遍历是有序的！** 中序遍历取第 k 个即可。

```python
class Solution:
    def kthSmallest(self, root, k: int) -> int:
        stack = []
        curr = root
        
        while curr or stack:
            while curr:
                stack.append(curr)
                curr = curr.left
            
            curr = stack.pop()
            k -= 1
            if k == 0:
                return curr.val
            curr = curr.right
```

---

## 199. 二叉树的右视图 (Binary Tree Right Side View)

**BFS 层序遍历，取每层最后一个节点。**

```python
from collections import deque

class Solution:
    def rightSideView(self, root) -> list[int]:
        if not root:
            return []
        
        result = []
        queue = deque([root])
        
        while queue:
            level_size = len(queue)
            for i in range(level_size):
                node = queue.popleft()
                if i == level_size - 1:
                    result.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return result
```
