# 56. 合并区间 (Merge Intervals)

## 题目描述

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]`。请你合并所有重叠的区间，并返回一个不重叠的区间数组。

**示例：**
```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠，合并为 [1,6]。
```

## 思路分析

### 排序 + 一次遍历 ✅

**核心思路：** 先按区间起点排序，然后逐个合并。

排序后，重叠的区间一定是相邻的。对于当前区间：
- 如果它的起点 <= 上一个合并区间的终点，说明重叠，合并（取终点的较大值）
- 否则，不重叠，直接加入结果

## 代码实现

```python
class Solution:
    def merge(self, intervals: list[list[int]]) -> list[list[int]]:
        # 按区间起点排序
        intervals.sort(key=lambda x: x[0])
        
        merged = [intervals[0]]
        
        for i in range(1, len(intervals)):
            # 当前区间与上一个合并区间比较
            if intervals[i][0] <= merged[-1][1]:
                # 重叠，合并（更新终点）
                merged[-1][1] = max(merged[-1][1], intervals[i][1])
            else:
                # 不重叠，直接加入
                merged.append(intervals[i])
        
        return merged
```

## 过程演示

```
intervals = [[1,3],[2,6],[8,10],[15,18]]
排序后不变（已有序）

merged = [[1,3]]
i=1: [2,6], 2 <= 3 → 重叠, merged = [[1,6]]
i=2: [8,10], 8 > 6 → 不重叠, merged = [[1,6],[8,10]]
i=3: [15,18], 15 > 10 → 不重叠, merged = [[1,6],[8,10],[15,18]]

结果：[[1,6],[8,10],[15,18]] ✅
```

## 复杂度分析

- **时间复杂度：** O(n log n)，排序的开销
- **空间复杂度：** O(1)，不算输出空间

## 关键要点

1. **排序是关键**：排序后重叠区间必定相邻
2. 合并时只需更新终点：`max(merged[-1][1], intervals[i][1])`
3. 区间问题的通用套路：先排序，再处理
