# 238. 除自身以外数组的乘积 (Product of Array Except Self)

## 题目描述

给你一个整数数组 `nums`，返回数组 `answer`，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。

**不能使用除法，且在 O(n) 时间复杂度内完成。**

**示例：**
```
输入：nums = [1,2,3,4]
输出：[24,12,8,6]
```

## 思路分析

### 左右乘积数组 ✅

`answer[i]` = 左边所有元素的乘积 × 右边所有元素的乘积

**算法：**
1. 第一遍从左到右：计算每个位置**左边**所有元素的乘积
2. 第二遍从右到左：计算每个位置**右边**所有元素的乘积，直接乘到结果中

## 代码实现

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        n = len(nums)
        answer = [1] * n
        
        # 第一遍：计算左边的乘积
        left_product = 1
        for i in range(n):
            answer[i] = left_product
            left_product *= nums[i]
        
        # 第二遍：乘上右边的乘积
        right_product = 1
        for i in range(n - 1, -1, -1):
            answer[i] *= right_product
            right_product *= nums[i]
        
        return answer
```

## 过程演示

```
nums = [1, 2, 3, 4]

第一遍（左边乘积）：
  answer = [1, 1, 2, 6]
  含义：每个位置左边所有数的乘积

第二遍（乘上右边乘积）：
  i=3: answer[3] = 6×1 = 6,   right=4
  i=2: answer[2] = 2×4 = 8,   right=12
  i=1: answer[1] = 1×12 = 12, right=24
  i=0: answer[0] = 1×24 = 24, right=24

  answer = [24, 12, 8, 6] ✅
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** O(1)（不算输出数组）

## 关键要点

1. **前缀积 + 后缀积**的思路
2. 不使用除法，两次遍历即可
3. 用单个变量累积乘积，空间 O(1)
