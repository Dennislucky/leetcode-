# 15. 三数之和 (3Sum)

## 题目描述

给你一个整数数组 `nums`，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j != k`，同时 `nums[i] + nums[j] + nums[k] == 0`。

返回所有和为 0 且**不重复**的三元组。

**示例：**
```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

## 思路分析

### 排序 + 双指针 ✅

**核心思路：** 先排序，然后固定一个数，用双指针找另外两个数。

**为什么要排序？**
1. 排序后可以用双指针在 O(n) 时间内找到两数之和等于目标值
2. 排序后容易**跳过重复元素**，避免结果重复

**算法步骤：**
1. 对数组排序
2. 枚举第一个数 `nums[i]`（i 从 0 到 n-3）：
   - 如果 `nums[i] > 0`，后面都是正数，三数之和不可能为 0，直接 break
   - 如果 `nums[i] == nums[i-1]`，跳过（避免重复三元组）
   - 在 `[i+1, n-1]` 范围内用双指针找两数之和等于 `-nums[i]`
3. 双指针的逻辑：
   - `sum < 0`：左指针右移（需要更大的数）
   - `sum > 0`：右指针左移（需要更小的数）
   - `sum == 0`：记录结果，同时跳过左右的重复元素

**去重的关键：**
- 第一层：枚举 i 时跳过相同的 `nums[i]`
- 第二层：找到答案后，left 和 right 都要跳过相同的值

## 代码实现

```python
class Solution:
    def threeSum(self, nums: list[int]) -> list[list[int]]:
        nums.sort()
        result = []
        n = len(nums)
        
        for i in range(n - 2):
            # 最小值大于 0，不可能有解
            if nums[i] > 0:
                break
            
            # 跳过重复的第一个数
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            # 双指针
            left, right = i + 1, n - 1
            target = -nums[i]
            
            while left < right:
                total = nums[left] + nums[right]
                
                if total < target:
                    left += 1
                elif total > target:
                    right -= 1
                else:
                    # 找到一组解
                    result.append([nums[i], nums[left], nums[right]])
                    
                    # 跳过重复的左指针
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    # 跳过重复的右指针
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    
                    left += 1
                    right -= 1
        
        return result
```

## 过程演示

```
nums = [-1, 0, 1, 2, -1, -4]
排序后: [-4, -1, -1, 0, 1, 2]

i=0, nums[i]=-4, target=4
  l=1, r=5: -1+2=1 < 4, l++
  l=2, r=5: -1+2=1 < 4, l++
  l=3, r=5: 0+2=2 < 4, l++
  l=4, r=5: 1+2=3 < 4, l++
  结束

i=1, nums[i]=-1, target=1
  l=2, r=5: -1+2=1 = 1 ✅ → [-1,-1,2]
  l=3, r=4: 0+1=1 = 1 ✅ → [-1,0,1]
  结束

i=2, nums[i]=-1, 与 nums[1] 相同，跳过

i=3, nums[i]=0, target=0
  l=4, r=5: 1+2=3 > 0, r--
  结束

结果：[[-1,-1,2], [-1,0,1]] ✅
```

## 复杂度分析

- **时间复杂度：** O(n²)。排序 O(n log n)，双指针嵌套循环 O(n²)
- **空间复杂度：** O(1)，不算排序和结果空间

## 关键要点

1. **排序是前提**，使得双指针和去重都成为可能
2. **三重去重**：i 层去重、left 去重、right 去重
3. 固定一个数，问题退化为**两数之和**
4. 这道题是双指针的综合应用，也是面试高频题
