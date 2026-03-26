# 560. 和为 K 的子数组 (Subarray Sum Equals K)

## 题目描述

给你一个整数数组 `nums` 和一个整数 `k`，请你统计并返回该数组中和为 `k` 的子数组的个数。

**示例：**
```
输入：nums = [1,1,1], k = 2
输出：2
解释：[1,1] 出现了两次
```

## 思路分析

### 为什么不能用滑动窗口？

因为数组中可能有**负数**，滑动窗口的单调性假设不成立。

### 前缀和 + 哈希表 ✅

**核心观察：** 子数组 `nums[i..j]` 的和 = `prefix[j+1] - prefix[i]`

如果 `prefix[j+1] - prefix[i] == k`，那么 `prefix[i] == prefix[j+1] - k`

所以问题转化为：**对于每个前缀和 prefix[j]，之前有多少个前缀和等于 prefix[j] - k？**

**算法步骤：**
1. 用哈希表记录每个前缀和出现的次数
2. 遍历数组，维护当前前缀和 `prefix_sum`
3. 查找 `prefix_sum - k` 在哈希表中出现了多少次，累加到结果中
4. 将当前前缀和记入哈希表

## 代码实现

```python
from collections import defaultdict

class Solution:
    def subarraySum(self, nums: list[int], k: int) -> int:
        # 前缀和出现的次数
        prefix_count = defaultdict(int)
        prefix_count[0] = 1  # 前缀和为 0 出现 1 次（空前缀）
        
        prefix_sum = 0
        count = 0
        
        for num in nums:
            prefix_sum += num
            
            # 查找有多少个之前的前缀和等于 prefix_sum - k
            count += prefix_count[prefix_sum - k]
            
            # 记录当前前缀和
            prefix_count[prefix_sum] += 1
        
        return count
```

## 过程演示

```
nums = [1, 1, 1], k = 2
prefix_count = {0: 1}

i=0: prefix_sum=1, 查找 1-2=-1 → 0次, prefix_count={0:1, 1:1}, count=0
i=1: prefix_sum=2, 查找 2-2=0  → 1次, prefix_count={0:1, 1:1, 2:1}, count=1
i=2: prefix_sum=3, 查找 3-2=1  → 1次, prefix_count={0:1, 1:1, 2:1, 3:1}, count=2

结果：2 ✅ （子数组 [1,1] 出现在位置 0-1 和 1-2）
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** O(n)

## 关键要点

1. **前缀和 + 哈希表**是处理"子数组和"问题的万能工具
2. `prefix_count[0] = 1` 的初始化很关键，表示空前缀
3. 必须先查找再记录，保证使用的是"之前"的前缀和
4. 这个套路可以拓展到很多变体题目
