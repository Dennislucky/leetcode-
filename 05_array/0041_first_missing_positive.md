# 41. 缺失的第一个正数 (First Missing Positive)

## 题目描述

给你一个未排序的整数数组 `nums`，请你找出其中没有出现的最小的正整数。

要求 O(n) 时间和 O(1) 额外空间。

**示例：**
```
输入：nums = [3,4,-1,1]
输出：2
```

## 思路分析

### 原地哈希（数组即哈希表）✅

**核心思想：** 把数组本身当作哈希表，让 `nums[i] = i + 1`。

答案一定在 `[1, n+1]` 范围内（n 为数组长度）。所以可以将每个在 `[1, n]` 范围内的数放到它应该在的位置上。

**算法：**
1. 遍历数组，把每个值为 `v` 的元素放到 `nums[v-1]` 的位置（交换）
2. 再遍历一次，第一个 `nums[i] != i + 1` 的位置，`i + 1` 就是答案

## 代码实现

```python
class Solution:
    def firstMissingPositive(self, nums: list[int]) -> int:
        n = len(nums)
        
        # 原地交换，把每个数放到正确位置
        for i in range(n):
            while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
                # 把 nums[i] 放到 nums[nums[i]-1] 的位置
                correct_pos = nums[i] - 1
                nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        
        # 找第一个不在正确位置的数
        for i in range(n):
            if nums[i] != i + 1:
                return i + 1
        
        return n + 1
```

## 过程演示

```
nums = [3, 4, -1, 1], n = 4

i=0: nums[0]=3, 放到位置2 → swap → [-1, 4, 3, 1]
     nums[0]=-1, 不在[1,4]范围，停
i=1: nums[1]=4, 放到位置3 → swap → [-1, 1, 3, 4]
     nums[1]=1, 放到位置0 → swap → [1, -1, 3, 4]
     nums[1]=-1, 停
i=2: nums[2]=3, nums[2]==3, 已在正确位置
i=3: nums[3]=4, nums[3]==4, 已在正确位置

最终：[1, -1, 3, 4]
检查：nums[0]=1✓, nums[1]=-1≠2 → 返回 2 ✅
```

## 复杂度分析

- **时间复杂度：** O(n)。每个元素最多被交换一次
- **空间复杂度：** O(1)

## 关键要点

1. **原地哈希**：数组本身就是哈希表，`nums[i]` 应该存放 `i+1`
2. while 循环条件要包含 `nums[nums[i]-1] != nums[i]`，避免死循环
3. 答案范围是 `[1, n+1]`，这是推理的关键
4. 这是 Hard 难度，但思路理解后代码很短
