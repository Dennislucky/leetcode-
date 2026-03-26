# 11. 盛最多水的容器 (Container With Most Water)

## 题目描述

给定一个长度为 `n` 的整数数组 `height`。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])`。

找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

**示例：**
```
输入：height = [1,8,6,2,5,4,8,3,7]
输出：49
解释：选择第 2 条（高 8）和第 9 条（高 7），宽度为 7，面积 = 7 × 7 = 49
```

## 思路分析

### 双指针法 ✅

**面积公式：** `area = min(height[left], height[right]) × (right - left)`

面积由两个因素决定：
1. **宽度**：`right - left`
2. **高度**：取决于较矮的那条线 `min(height[left], height[right])`

**算法策略：**
1. 左右指针分别从数组两端开始，此时**宽度最大**
2. 每次移动**较矮**的那个指针，向中间收缩
3. 记录过程中的最大面积

**为什么移动较矮的指针？**

这是最核心的问题。假设 `height[left] < height[right]`：
- 如果移动右指针（较高的），宽度减小，而高度受限于左边不变或更小 → 面积只会减小或不变
- 如果移动左指针（较矮的），宽度减小，但高度可能增加 → 面积**有可能增大**

所以，移动较矮指针是唯一有可能找到更大面积的选择。

**正确性证明：**
- 每次移动较矮指针时，我们排除的所有组合（该矮指针与之间所有位置的组合）面积都不可能超过当前面积
- 因此不会错过最优解

## 代码实现

```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        left, right = 0, len(height) - 1
        max_area = 0
        
        while left < right:
            # 计算当前面积
            width = right - left
            h = min(height[left], height[right])
            max_area = max(max_area, width * h)
            
            # 移动较矮的指针
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        
        return max_area
```

## 过程演示

```
height = [1,8,6,2,5,4,8,3,7]

l=0, r=8: min(1,7)×8 = 8,  h[l]<h[r], l++
l=1, r=8: min(8,7)×7 = 49, h[l]>h[r], r--  ← 最大值
l=1, r=7: min(8,3)×6 = 18, h[l]>h[r], r--
l=1, r=6: min(8,8)×5 = 40, h[l]=h[r], r--
l=1, r=5: min(8,4)×4 = 16, h[l]>h[r], r--
l=1, r=4: min(8,5)×3 = 15, h[l]>h[r], r--
l=1, r=3: min(8,2)×2 = 4,  h[l]>h[r], r--
l=1, r=2: min(8,6)×1 = 6,  h[l]>h[r], r--
结束

结果：49 ✅
```

## 复杂度分析

- **时间复杂度：** O(n)，左右指针一共走 n 步
- **空间复杂度：** O(1)

## 关键要点

1. **对撞双指针**的经典应用
2. 核心策略：**永远移动较矮的指针**，因为移动较高的不可能带来更好的结果
3. 这道题的正确性证明本质上是一个**贪心**思想
