# 240. 搜索二维矩阵 II (Search a 2D Matrix II)

## 题目描述

编写一个高效的算法来搜索 `m x n` 矩阵中的一个目标值 `target`。矩阵具有以下特性：
- 每行的元素从左到右升序排列
- 每列的元素从上到下升序排列

**示例：**
```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

## 思路分析

### 从右上角开始搜索 ✅

**核心观察：** 从右上角（或左下角）出发，每一步都能排除一行或一列。

从右上角 `(0, n-1)` 开始：
- 如果当前值 == target → 找到了
- 如果当前值 > target → 排除当前列（target 不可能在这一列），左移
- 如果当前值 < target → 排除当前行（target 不可能在这一行），下移

**为什么选右上角？** 因为向左数变小，向下数变大，形成了类似 BST 的结构。

## 代码实现

```python
class Solution:
    def searchMatrix(self, matrix: list[list[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        
        # 从右上角开始
        i, j = 0, n - 1
        
        while i < m and j >= 0:
            if matrix[i][j] == target:
                return True
            elif matrix[i][j] > target:
                j -= 1  # 左移，排除当前列
            else:
                i += 1  # 下移，排除当前行
        
        return False
```

## 复杂度分析

- **时间复杂度：** O(m + n)
- **空间复杂度：** O(1)

## 关键要点

1. **右上角/左下角**是搜索的起点，因为有明确的二分性
2. 每步排除一行或一列，最多走 m+n 步
3. 不要从左上角或右下角开始——两个方向都是增/减的，无法做决策
