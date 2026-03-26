# 堆/优先队列题合集

## 215. 数组中的第 K 个最大元素 (Kth Largest Element in an Array)

### 思路

**方法一：** 最小堆，维护大小为 k 的堆，堆顶就是第 k 大。

**方法二：** 快速选择（Quick Select），平均 O(n)。

### 最小堆法

```python
import heapq

class Solution:
    def findKthLargest(self, nums: list[int], k: int) -> int:
        # 维护大小为 k 的最小堆
        heap = nums[:k]
        heapq.heapify(heap)
        
        for num in nums[k:]:
            if num > heap[0]:
                heapq.heapreplace(heap, num)
        
        return heap[0]
```

### 快速选择法

```python
import random

class Solution:
    def findKthLargest(self, nums: list[int], k: int) -> int:
        target = len(nums) - k  # 第 k 大 = 第 n-k 小
        
        def quick_select(left, right):
            pivot_idx = random.randint(left, right)
            nums[pivot_idx], nums[right] = nums[right], nums[pivot_idx]
            pivot = nums[right]
            
            store = left
            for i in range(left, right):
                if nums[i] < pivot:
                    nums[i], nums[store] = nums[store], nums[i]
                    store += 1
            nums[store], nums[right] = nums[right], nums[store]
            
            if store == target:
                return nums[store]
            elif store < target:
                return quick_select(store + 1, right)
            else:
                return quick_select(left, store - 1)
        
        return quick_select(0, len(nums) - 1)
```

**堆：O(n log k)；快速选择：平均 O(n)，最坏 O(n²)。**

---

## 347. 前 K 个高频元素 (Top K Frequent Elements)

```python
from collections import Counter
import heapq

class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = Counter(nums)
        # nlargest 内部使用堆
        return [x for x, _ in count.most_common(k)]
```

### 手动堆实现

```python
from collections import Counter
import heapq

class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = Counter(nums)
        # 维护大小为 k 的最小堆
        return heapq.nlargest(k, count.keys(), key=count.get)
```

### 桶排序法 O(n)

```python
from collections import Counter

class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        count = Counter(nums)
        # 频率作为下标
        buckets = [[] for _ in range(len(nums) + 1)]
        for num, freq in count.items():
            buckets[freq].append(num)
        
        result = []
        for freq in range(len(buckets) - 1, -1, -1):
            for num in buckets[freq]:
                result.append(num)
                if len(result) == k:
                    return result
        return result
```

---

## 295. 数据流的中位数 (Find Median from Data Stream)

### 思路

用**两个堆**：
- **大顶堆**存较小的一半
- **小顶堆**存较大的一半

中位数从两个堆顶获取。

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.small = []  # 大顶堆（取负值模拟）
        self.large = []  # 小顶堆
    
    def addNum(self, num: int) -> None:
        # 先加入大顶堆
        heapq.heappush(self.small, -num)
        # 大顶堆的最大值移到小顶堆
        heapq.heappush(self.large, -heapq.heappop(self.small))
        
        # 保持大顶堆大小 >= 小顶堆
        if len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))
    
    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2
```

**关键：** Python 只有小顶堆，用取负值的方式模拟大顶堆。
