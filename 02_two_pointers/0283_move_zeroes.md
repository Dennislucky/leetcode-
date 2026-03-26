# 283. 移动零 (Move Zeroes)

## 题目描述

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**必须在不复制数组的情况下原地对数组进行操作。**

**示例：**
```
输入：nums = [0,1,0,3,12]
输出：[1,3,12,0,0]
```

## 思路分析

### 双指针法 ✅

使用**快慢指针**：
- `slow`：指向下一个非零元素应该放置的位置
- `fast`：用于遍历数组

**核心逻辑：**
- `fast` 扫描整个数组
- 当 `fast` 遇到非零元素时，将其放到 `slow` 的位置，然后 `slow` 前进
- 最后 `slow` 之后的位置全部填 0

**直觉理解：** slow 指针左边都是已处理好的非零元素；fast 指针用来探索未处理的元素。

## 代码实现

```python
class Solution:
    def moveZeroes(self, nums: list[int]) -> None:
        slow = 0
        
        # fast 扫描，遇到非零就放到 slow 位置
        for fast in range(len(nums)):
            if nums[fast] != 0:
                nums[slow], nums[fast] = nums[fast], nums[slow]
                slow += 1
```

### 更易理解的两遍写法

```python
class Solution:
    def moveZeroes(self, nums: list[int]) -> None:
        slow = 0
        
        # 第一遍：把所有非零元素移到前面
        for fast in range(len(nums)):
            if nums[fast] != 0:
                nums[slow] = nums[fast]
                slow += 1
        
        # 第二遍：剩余位置填 0
        for i in range(slow, len(nums)):
            nums[i] = 0
```

## 过程演示

```
初始：  [0, 1, 0, 3, 12]
         s
         f

f=0: nums[0]=0, 跳过
f=1: nums[1]=1≠0, swap(s=0, f=1) → [1, 0, 0, 3, 12], s=1
f=2: nums[2]=0, 跳过
f=3: nums[3]=3≠0, swap(s=1, f=3) → [1, 3, 0, 0, 12], s=2
f=4: nums[4]=12≠0, swap(s=2, f=4) → [1, 3, 12, 0, 0], s=3

结果：[1, 3, 12, 0, 0] ✅
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** O(1)，原地操作

## 关键要点

1. **快慢指针**是数组原地操作的基本技巧
2. swap 版本只需一次遍历，更优雅
3. slow 左边始终是已排好的非零元素，这是**循环不变量**
