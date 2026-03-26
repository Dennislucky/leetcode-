# 二分查找题合集

## 二分查找模板

```python
# 标准模板
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

---

## 35. 搜索插入位置 (Search Insert Position)

找到 target 的位置或应该插入的位置。

```python
class Solution:
    def searchInsert(self, nums: list[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return left  # left 就是插入位置
```

---

## 74. 搜索二维矩阵 (Search a 2D Matrix)

矩阵每行有序且下一行第一个元素大于上一行最后一个。将二维坐标映射为一维。

```python
class Solution:
    def searchMatrix(self, matrix: list[list[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        left, right = 0, m * n - 1
        
        while left <= right:
            mid = left + (right - left) // 2
            val = matrix[mid // n][mid % n]  # 一维下标转二维
            if val == target:
                return True
            elif val < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return False
```

---

## 34. 在排序数组中查找元素的第一个和最后一个位置

两次二分：一次找左边界，一次找右边界。

```python
class Solution:
    def searchRange(self, nums: list[int], target: int) -> list[int]:
        def find_left(nums, target):
            left, right = 0, len(nums) - 1
            while left <= right:
                mid = left + (right - left) // 2
                if nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1
            return left
        
        def find_right(nums, target):
            left, right = 0, len(nums) - 1
            while left <= right:
                mid = left + (right - left) // 2
                if nums[mid] <= target:
                    left = mid + 1
                else:
                    right = mid - 1
            return right
        
        lo = find_left(nums, target)
        hi = find_right(nums, target)
        
        if lo <= hi:
            return [lo, hi]
        return [-1, -1]
```

---

## 33. 搜索旋转排序数组 (Search in Rotated Sorted Array)

### 思路

数组被旋转后，至少有一半是有序的。利用有序的那一半判断 target 在哪边。

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left <= right:
            mid = left + (right - left) // 2
            
            if nums[mid] == target:
                return mid
            
            # 左半边有序
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            # 右半边有序
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        
        return -1
```

**关键：** 先判断哪半边有序，再判断 target 是否在有序的那半边。

---

## 153. 寻找旋转排序数组中的最小值

```python
class Solution:
    def findMin(self, nums: list[int]) -> int:
        left, right = 0, len(nums) - 1
        
        while left < right:
            mid = left + (right - left) // 2
            if nums[mid] > nums[right]:
                left = mid + 1  # 最小值在右半边
            else:
                right = mid     # 最小值在左半边（含 mid）
        
        return nums[left]
```

---

## 4. 寻找两个正序数组的中位数 (Median of Two Sorted Arrays)

### 思路（Hard）

二分查找较短的数组，找到一个分割点，使得左半部分的最大值 ≤ 右半部分的最小值。

```python
class Solution:
    def findMedianSortedArrays(self, nums1: list[int], nums2: list[int]) -> float:
        # 确保 nums1 是较短的
        if len(nums1) > len(nums2):
            nums1, nums2 = nums2, nums1
        
        m, n = len(nums1), len(nums2)
        left, right = 0, m
        
        while left <= right:
            i = (left + right) // 2     # nums1 的分割点
            j = (m + n + 1) // 2 - i    # nums2 的分割点
            
            left1 = nums1[i-1] if i > 0 else float('-inf')
            right1 = nums1[i] if i < m else float('inf')
            left2 = nums2[j-1] if j > 0 else float('-inf')
            right2 = nums2[j] if j < n else float('inf')
            
            if left1 <= right2 and left2 <= right1:
                # 找到正确的分割
                if (m + n) % 2 == 0:
                    return (max(left1, left2) + min(right1, right2)) / 2
                else:
                    return max(left1, left2)
            elif left1 > right2:
                right = i - 1
            else:
                left = i + 1
        
        return 0
```

**时间 O(log min(m,n))** — 这道题是二分查找的巅峰之作。
