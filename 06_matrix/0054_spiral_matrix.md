# 54. 螺旋矩阵 (Spiral Matrix)

## 题目描述

给你一个 `m x n` 矩阵 `matrix`，请按照**顺时针螺旋顺序**，返回矩阵中的所有元素。

**示例：**
```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

## 思路分析

### 模拟法：四边界收缩 ✅

维护四个边界：`top, bottom, left, right`，按照 右→下→左→上 的顺序遍历，每遍历完一条边就收缩对应的边界。

## 代码实现

```python
class Solution:
    def spiralOrder(self, matrix: list[list[int]]) -> list[int]:
        result = []
        top, bottom = 0, len(matrix) - 1
        left, right = 0, len(matrix[0]) - 1
        
        while top <= bottom and left <= right:
            # 从左到右（上边）
            for j in range(left, right + 1):
                result.append(matrix[top][j])
            top += 1
            
            # 从上到下（右边）
            for i in range(top, bottom + 1):
                result.append(matrix[i][right])
            right -= 1
            
            # 从右到左（下边）
            if top <= bottom:
                for j in range(right, left - 1, -1):
                    result.append(matrix[bottom][j])
                bottom -= 1
            
            # 从下到上（左边）
            if left <= right:
                for i in range(bottom, top - 1, -1):
                    result.append(matrix[i][left])
                left += 1
        
        return result
```

## 复杂度分析

- **时间复杂度：** O(m × n)
- **空间复杂度：** O(1)（不算输出）

## 关键要点

1. 四个边界的管理是核心
2. 注意下边和左边遍历时要额外检查边界条件，防止单行/单列重复遍历
3. 模拟题要细心，画图帮助理解
