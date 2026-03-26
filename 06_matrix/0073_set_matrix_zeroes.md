# 73. 矩阵置零 (Set Matrix Zeroes)

## 题目描述

给定一个 `m x n` 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用**原地**算法。

**示例：**
```
输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

## 思路分析

### 用第一行和第一列作为标记 ✅

**O(1) 空间思路：** 用矩阵自身的第一行和第一列来标记哪些行/列需要置零。

1. 先记录第一行和第一列本身是否有 0
2. 用第一行/第一列作为标记数组
3. 根据标记置零
4. 最后处理第一行和第一列

## 代码实现

```python
class Solution:
    def setZeroes(self, matrix: list[list[int]]) -> None:
        m, n = len(matrix), len(matrix[0])
        
        # 记录第一行和第一列是否有 0
        first_row_zero = any(matrix[0][j] == 0 for j in range(n))
        first_col_zero = any(matrix[i][0] == 0 for i in range(m))
        
        # 用第一行和第一列标记
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == 0:
                    matrix[i][0] = 0  # 标记该行
                    matrix[0][j] = 0  # 标记该列
        
        # 根据标记置零（跳过第一行和第一列）
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0
        
        # 处理第一行
        if first_row_zero:
            for j in range(n):
                matrix[0][j] = 0
        
        # 处理第一列
        if first_col_zero:
            for i in range(m):
                matrix[i][0] = 0
```

## 复杂度分析

- **时间复杂度：** O(m × n)
- **空间复杂度：** O(1)

## 关键要点

1. **用矩阵自身作为标记空间**，避免额外的 O(m+n) 空间
2. 第一行/第一列需要特殊处理，因为它们被用作标记
3. 处理顺序很重要：先标记，再置零，最后处理首行首列
