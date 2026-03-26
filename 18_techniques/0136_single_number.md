# 技巧题合集

## 136. 只出现一次的数字 (Single Number)

### 思路

**异或运算的性质：**
- `a ^ a = 0`（相同的数异或为 0）
- `a ^ 0 = a`
- 异或满足交换律和结合律

所有数字异或一遍，重复的数互相抵消，剩下的就是只出现一次的数。

```python
class Solution:
    def singleNumber(self, nums: list[int]) -> int:
        result = 0
        for num in nums:
            result ^= num
        return result
```

**时间 O(n)，空间 O(1)。** 位运算的经典应用。

---

## 169. 多数元素 (Majority Element)

### Boyer-Moore 投票算法

**核心思想：** 多数元素出现次数超过 n/2，如果把多数元素和其他元素一一抵消，最后一定剩下多数元素。

```python
class Solution:
    def majorityElement(self, nums: list[int]) -> int:
        candidate = None
        count = 0
        
        for num in nums:
            if count == 0:
                candidate = num
            count += 1 if num == candidate else -1
        
        return candidate
```

**直觉：** 把多数元素看作"正方"，其他元素看作"反方"，正方人多一定赢。

---

## 75. 颜色分类 (Sort Colors)

### 荷兰国旗问题

三个指针：`p0`（0 的右边界）、`p2`（2 的左边界）、`curr`（当前遍历）。

```python
class Solution:
    def sortColors(self, nums: list[int]) -> None:
        p0, curr, p2 = 0, 0, len(nums) - 1
        
        while curr <= p2:
            if nums[curr] == 0:
                nums[curr], nums[p0] = nums[p0], nums[curr]
                p0 += 1
                curr += 1
            elif nums[curr] == 2:
                nums[curr], nums[p2] = nums[p2], nums[curr]
                p2 -= 1
                # 注意：curr 不前进，因为交换过来的值还没检查
            else:
                curr += 1
```

**关键：** 当 `nums[curr] == 2` 时，交换后 `curr` **不前进**，因为从 `p2` 换过来的值还没检查。

---

## 31. 下一个排列 (Next Permutation)

### 思路

1. **从右往左**找第一个下降的位置 `i`（`nums[i] < nums[i+1]`）
2. **从右往左**找第一个大于 `nums[i]` 的数 `nums[j]`
3. 交换 `nums[i]` 和 `nums[j]`
4. 翻转 `i+1` 之后的部分

```python
class Solution:
    def nextPermutation(self, nums: list[int]) -> None:
        n = len(nums)
        
        # Step 1: 找第一个下降点
        i = n - 2
        while i >= 0 and nums[i] >= nums[i + 1]:
            i -= 1
        
        if i >= 0:
            # Step 2: 找比 nums[i] 大的最小数
            j = n - 1
            while nums[j] <= nums[i]:
                j -= 1
            # Step 3: 交换
            nums[i], nums[j] = nums[j], nums[i]
        
        # Step 4: 翻转 i+1 之后的部分
        left, right = i + 1, n - 1
        while left < right:
            nums[left], nums[right] = nums[right], nums[left]
            left += 1
            right -= 1
```

**示例：** `[1,2,3] → [1,3,2] → [2,1,3] → [2,3,1] → [3,1,2] → [3,2,1] → [1,2,3]`

---

## 287. 寻找重复数 (Find the Duplicate Number)

### 思路

**Floyd 判圈法**（和环形链表一样的方法！）

将数组看作一个隐式链表：`index → nums[index]`。有重复数意味着有环，重复的数就是环的入口。

```python
class Solution:
    def findDuplicate(self, nums: list[int]) -> int:
        # 阶段一：找相遇点
        slow = fast = 0
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break
        
        # 阶段二：找环入口
        ptr = 0
        while ptr != slow:
            ptr = nums[ptr]
            slow = nums[slow]
        
        return ptr
```

**时间 O(n)，空间 O(1)。不修改数组。**

**关键洞察：** 数组值域是 `[1, n]`，数组长度是 `n+1`，自然形成一个函数 `f(x) = nums[x]`。
由鸽巢原理，必有重复 → 必有环 → Floyd 算法找环入口 → 找到重复数。
