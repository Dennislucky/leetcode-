# 42. 接雨水 (Trapping Rain Water)

## 题目描述

给定 `n` 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例：**
```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
```

## 思路分析

### 核心观察

对于每个位置 `i`，它能接的雨水量取决于：
```
water[i] = min(左边最高柱子, 右边最高柱子) - height[i]
```

如果这个值为负，就是 0（接不住雨水）。

### 方法一：前缀最大值（预处理）

1. 从左到右扫一遍，记录每个位置左边的最大高度 `left_max[i]`
2. 从右到左扫一遍，记录每个位置右边的最大高度 `right_max[i]`
3. 遍历每个位置，计算 `min(left_max[i], right_max[i]) - height[i]`

### 方法二：双指针（最优解）✅

**空间优化思路：** 不需要两个数组来存前缀最大值，用双指针可以做到 O(1) 空间。

**关键洞察：**
- 维护 `left_max` 和 `right_max`
- 如果 `left_max < right_max`，那么位置 `left` 能接的水由 `left_max` 决定（因为右边一定有比 `left_max` 高的柱子）
- 反之，位置 `right` 能接的水由 `right_max` 决定

## 代码实现

### 方法一：前缀最大值

```python
class Solution:
    def trap(self, height: list[int]) -> int:
        n = len(height)
        if n <= 2:
            return 0
        
        # 预计算左边最大值
        left_max = [0] * n
        left_max[0] = height[0]
        for i in range(1, n):
            left_max[i] = max(left_max[i-1], height[i])
        
        # 预计算右边最大值
        right_max = [0] * n
        right_max[n-1] = height[n-1]
        for i in range(n-2, -1, -1):
            right_max[i] = max(right_max[i+1], height[i])
        
        # 计算总雨水量
        total = 0
        for i in range(n):
            total += min(left_max[i], right_max[i]) - height[i]
        
        return total
```

### 方法二：双指针（最优）

```python
class Solution:
    def trap(self, height: list[int]) -> int:
        left, right = 0, len(height) - 1
        left_max, right_max = 0, 0
        total = 0
        
        while left < right:
            if height[left] < height[right]:
                # 左边较矮，处理左边
                if height[left] >= left_max:
                    left_max = height[left]
                else:
                    total += left_max - height[left]
                left += 1
            else:
                # 右边较矮，处理右边
                if height[right] >= right_max:
                    right_max = height[right]
                else:
                    total += right_max - height[right]
                right -= 1
        
        return total
```

## 过程演示（双指针法）

```
height = [0,1,0,2,1,0,1,3,2,1,2,1]

l=0,  r=11: h[l]=0 < h[r]=1, left_max=0, water+=0,     l++
l=1,  r=11: h[l]=1 = h[r]=1, right_max=1, water+=0,     r--
l=1,  r=10: h[l]=1 < h[r]=2, left_max=1, water+=0,      l++
l=2,  r=10: h[l]=0 < h[r]=2, water+=1-0=1,              l++
l=3,  r=10: h[l]=2 = h[r]=2, right_max=2, water+=0,     r--
l=3,  r=9:  h[l]=2 > h[r]=1, water+=2-1=1,              r--
l=3,  r=8:  h[l]=2 = h[r]=2, right_max=2, water+=0,     r--
l=3,  r=7:  h[l]=2 < h[r]=3, left_max=2, water+=0,      l++
l=4,  r=7:  h[l]=1 < h[r]=3, water+=2-1=1,              l++
l=5,  r=7:  h[l]=0 < h[r]=3, water+=2-0=2,              l++
l=6,  r=7:  h[l]=1 < h[r]=3, water+=2-1=1,              l++

total = 1+1+1+2+1 = 6 ✅
```

## 复杂度分析

| 方法 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 前缀最大值 | O(n) | O(n) |
| 双指针 | O(n) | O(1) |

## 关键要点

1. 每个位置能接的水 = `min(左边最高, 右边最高) - 自身高度`
2. 双指针的核心：**哪边矮就处理哪边**，因为矮的那边的水量已经可以确定
3. 这是 Hard 难度的经典题，面试中出现频率极高
4. 还有**单调栈**解法，详见栈章节
