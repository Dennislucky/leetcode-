# 53. 最大子数组和 (Maximum Subarray)

## 题目描述

给你一个整数数组 `nums`，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例：**
```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6。
```

## 思路分析

### Kadane 算法（动态规划）✅

**状态定义：** `dp[i]` = 以 `nums[i]` 结尾的最大子数组和

**状态转移：**
```
dp[i] = max(nums[i], dp[i-1] + nums[i])
```

含义：对于位置 i，要么自己单独成一个子数组，要么接在前面的子数组后面。

**选择标准：** 如果前面的子数组和是正的（`dp[i-1] > 0`），就接上去；否则自己单干。

由于 `dp[i]` 只依赖 `dp[i-1]`，可以用一个变量代替数组。

## 代码实现

```python
class Solution:
    def maxSubArray(self, nums: list[int]) -> int:
        current_sum = nums[0]
        max_sum = nums[0]
        
        for i in range(1, len(nums)):
            # 要么接上前面的子数组，要么重新开始
            current_sum = max(nums[i], current_sum + nums[i])
            max_sum = max(max_sum, current_sum)
        
        return max_sum
```

## 过程演示

```
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]

i=0: current=-2, max=-2
i=1: current=max(1, -2+1)=1,   max=1
i=2: current=max(-3, 1-3)=-2,  max=1
i=3: current=max(4, -2+4)=4,   max=4
i=4: current=max(-1, 4-1)=3,   max=4
i=5: current=max(2, 3+2)=5,    max=5
i=6: current=max(1, 5+1)=6,    max=6  ← [4,-1,2,1]
i=7: current=max(-5, 6-5)=1,   max=6
i=8: current=max(4, 1+4)=5,    max=6

结果：6 ✅
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** O(1)

## 关键要点

1. **Kadane 算法**是求最大子数组和的经典算法
2. 核心决策：**续接 or 重新开始**
3. 空间优化：`dp` 数组可以压缩为单个变量
4. 这是一维 DP 的入门题，理解后可以拓展到二维
