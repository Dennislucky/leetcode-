# 二叉树进阶题合集

## 114. 二叉树展开为链表 (Flatten Binary Tree to Linked List)

### 思路

将二叉树按**前序遍历**展开为链表（只有右子节点）。

**技巧：** 对于每个节点，将左子树插入到右子树的位置，原右子树接到左子树最右节点后面。

```python
class Solution:
    def flatten(self, root) -> None:
        curr = root
        while curr:
            if curr.left:
                # 找左子树的最右节点
                rightmost = curr.left
                while rightmost.right:
                    rightmost = rightmost.right
                
                # 将右子树接到左子树最右节点后面
                rightmost.right = curr.right
                # 左子树移到右边
                curr.right = curr.left
                curr.left = None
            
            curr = curr.right
```

**时间 O(n)，空间 O(1)**

---

## 105. 从前序与中序遍历序列构造二叉树 (Construct Binary Tree from Preorder and Inorder)

### 思路

- **前序遍历**的第一个元素是根
- 在**中序遍历**中找到根的位置，左边是左子树，右边是右子树
- 递归构建

```python
class Solution:
    def buildTree(self, preorder, inorder):
        if not preorder:
            return None
        
        # 用哈希表加速查找
        inorder_map = {val: idx for idx, val in enumerate(inorder)}
        
        def build(pre_start, pre_end, in_start, in_end):
            if pre_start > pre_end:
                return None
            
            root_val = preorder[pre_start]
            root = TreeNode(root_val)
            
            # 根在中序中的位置
            in_root = inorder_map[root_val]
            left_size = in_root - in_start  # 左子树大小
            
            root.left = build(pre_start + 1, pre_start + left_size,
                            in_start, in_root - 1)
            root.right = build(pre_start + left_size + 1, pre_end,
                             in_root + 1, in_end)
            
            return root
        
        return build(0, len(preorder) - 1, 0, len(inorder) - 1)
```

**关键：** 通过左子树大小 `left_size` 确定前序遍历中左右子树的范围。

---

## 437. 路径总和 III (Path Sum III)

### 思路

路径不一定从根开始，也不一定到叶子结束。使用**前缀和 + 哈希表**。

```python
from collections import defaultdict

class Solution:
    def pathSum(self, root, targetSum: int) -> int:
        prefix_count = defaultdict(int)
        prefix_count[0] = 1
        
        def dfs(node, curr_sum):
            if not node:
                return 0
            
            curr_sum += node.val
            # 有多少前缀和等于 curr_sum - targetSum
            count = prefix_count[curr_sum - targetSum]
            
            prefix_count[curr_sum] += 1
            count += dfs(node.left, curr_sum)
            count += dfs(node.right, curr_sum)
            prefix_count[curr_sum] -= 1  # 回溯
            
            return count
        
        return dfs(root, 0)
```

**关键：** 树上的前缀和！回溯时要恢复哈希表，因为不同路径不能共享前缀和。

---

## 236. 二叉树的最近公共祖先 (Lowest Common Ancestor)

### 思路

递归：如果当前节点是 p 或 q，返回当前节点。否则在左右子树中找，如果左右都找到了，当前节点就是 LCA。

```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if not root or root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        
        if left and right:
            return root  # p 和 q 分别在左右子树
        
        return left if left else right
```

**这段代码虽短，但逻辑完美。**

---

## 124. 二叉树中的最大路径和 (Binary Tree Maximum Path Sum)

### 思路

**Hard 题。** 路径可以不经过根。

对于每个节点，它能参与的最大路径和 = 自身值 + 左子树贡献 + 右子树贡献。
但是向上传递时，只能选择左或右的一条路径。

```python
class Solution:
    def maxPathSum(self, root) -> int:
        self.max_sum = float('-inf')
        
        def max_gain(node):
            if not node:
                return 0
            
            # 递归计算左右子树的最大贡献（负数则不选）
            left_gain = max(max_gain(node.left), 0)
            right_gain = max(max_gain(node.right), 0)
            
            # 以当前节点为"拐点"的路径和
            path_sum = node.val + left_gain + right_gain
            self.max_sum = max(self.max_sum, path_sum)
            
            # 返回给父节点时，只能选一条路
            return node.val + max(left_gain, right_gain)
        
        max_gain(root)
        return self.max_sum
```

**关键区分：**
- **更新答案时**：可以同时用左右两条路径（形成"人字形"）
- **返回给父节点时**：只能选一条（路径不能分叉）
- `max(gain, 0)`：不选负贡献的子树
