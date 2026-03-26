# 动态规划题合集（一维 DP）

## DP 解题四步法

1. **定义状态：** `dp[i]` 代表什么？
2. **状态转移方程：** `dp[i]` 和之前的状态有什么关系？
3. **初始条件：** 最简单的情况是什么？
4. **计算顺序：** 从小到大？还是从大到小？

---

## 70. 爬楼梯 (Climbing Stairs)

**状态：** `dp[i]` = 到达第 i 阶的方法数  
**转移：** `dp[i] = dp[i-1] + dp[i-2]`（最后一步走 1 阶或 2 阶）

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n
        a, b = 1, 2
        for _ in range(3, n + 1):
            a, b = b, a + b
        return b
```

实际上就是斐波那契数列。

---

## 118. 杨辉三角 (Pascal's Triangle)

```python
class Solution:
    def generate(self, numRows: int) -> list[list[int]]:
        triangle = []
        for i in range(numRows):
            row = [1] * (i + 1)
            for j in range(1, i):
                row[j] = triangle[i-1][j-1] + triangle[i-1][j]
            triangle.append(row)
        return triangle
```

---

## 198. 打家劫舍 (House Robber)

**状态：** `dp[i]` = 偷到第 i 家时的最大金额  
**转移：** `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`（偷或不偷当前这家）

```python
class Solution:
    def rob(self, nums: list[int]) -> int:
        if len(nums) <= 2:
            return max(nums)
        
        a, b = nums[0], max(nums[0], nums[1])
        for i in range(2, len(nums)):
            a, b = b, max(b, a + nums[i])
        return b
```

---

## 279. 完全平方数 (Perfect Squares)

**状态：** `dp[i]` = 组成 i 的最少完全平方数个数  
**转移：** `dp[i] = min(dp[i - j*j] + 1)` for all valid j

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [0] + [float('inf')] * n
        
        for i in range(1, n + 1):
            j = 1
            while j * j <= i:
                dp[i] = min(dp[i], dp[i - j*j] + 1)
                j += 1
        
        return dp[n]
```

---

## 322. 零钱兑换 (Coin Change)

**状态：** `dp[i]` = 凑出金额 i 的最少硬币数  
**转移：** `dp[i] = min(dp[i - coin] + 1)` for each coin

```python
class Solution:
    def coinChange(self, coins: list[int], amount: int) -> int:
        dp = [0] + [float('inf')] * amount
        
        for i in range(1, amount + 1):
            for coin in coins:
                if coin <= i:
                    dp[i] = min(dp[i], dp[i - coin] + 1)
        
        return dp[amount] if dp[amount] != float('inf') else -1
```

**这是完全背包问题的变形。**

---

## 139. 单词拆分 (Word Break)

**状态：** `dp[i]` = s 的前 i 个字符是否可以用 wordDict 中的单词拼出  
**转移：** `dp[i] = any(dp[j] and s[j:i] in wordSet)` for j in 0..i-1

```python
class Solution:
    def wordBreak(self, s: str, wordDict: list[str]) -> bool:
        word_set = set(wordDict)
        n = len(s)
        dp = [False] * (n + 1)
        dp[0] = True
        
        for i in range(1, n + 1):
            for j in range(i):
                if dp[j] and s[j:i] in word_set:
                    dp[i] = True
                    break
        
        return dp[n]
```

---

## 300. 最长递增子序列 (Longest Increasing Subsequence)

### 方法一：DP O(n²)

**状态：** `dp[i]` = 以 nums[i] 结尾的 LIS 长度  
**转移：** `dp[i] = max(dp[j] + 1)` for j < i and nums[j] < nums[i]

```python
class Solution:
    def lengthOfLIS(self, nums: list[int]) -> int:
        n = len(nums)
        dp = [1] * n
        
        for i in range(1, n):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i], dp[j] + 1)
        
        return max(dp)
```

### 方法二：贪心 + 二分查找 O(n log n)

维护一个单调递增序列 `tails`，用二分查找寻找插入位置。

```python
import bisect

class Solution:
    def lengthOfLIS(self, nums: list[int]) -> int:
        tails = []
        for num in nums:
            pos = bisect.bisect_left(tails, num)
            if pos == len(tails):
                tails.append(num)
            else:
                tails[pos] = num
        return len(tails)
```

---

## 152. 乘积最大子数组 (Maximum Product Subarray)

### 思路

因为负数×负数=正数，所以需要同时维护**最大值和最小值**。

```python
class Solution:
    def maxProduct(self, nums: list[int]) -> int:
        max_prod = min_prod = result = nums[0]
        
        for i in range(1, len(nums)):
            if nums[i] < 0:
                max_prod, min_prod = min_prod, max_prod
            
            max_prod = max(nums[i], max_prod * nums[i])
            min_prod = min(nums[i], min_prod * nums[i])
            result = max(result, max_prod)
        
        return result
```

**关键：** 遇到负数时，最大最小互换。

---

## 416. 分割等和子集 (Partition Equal Subset Sum)

### 思路

转化为 **0-1 背包**：能否从数组中选若干个数，使其和等于 `total_sum / 2`。

```python
class Solution:
    def canPartition(self, nums: list[int]) -> bool:
        total = sum(nums)
        if total % 2 != 0:
            return False
        
        target = total // 2
        dp = [False] * (target + 1)
        dp[0] = True
        
        for num in nums:
            for j in range(target, num - 1, -1):  # 倒序遍历！
                dp[j] = dp[j] or dp[j - num]
        
        return dp[target]
```

**倒序遍历**防止一个物品被重复选取。

---

## 32. 最长有效括号 (Longest Valid Parentheses)

### 栈方法

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stack = [-1]  # 哨兵，表示有效括号的起始位置之前
        max_len = 0
        
        for i, char in enumerate(s):
            if char == '(':
                stack.append(i)
            else:
                stack.pop()
                if not stack:
                    stack.append(i)  # 新的哨兵
                else:
                    max_len = max(max_len, i - stack[-1])
        
        return max_len
```

### DP 方法

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        n = len(s)
        dp = [0] * n  # dp[i] = 以 s[i] 结尾的最长有效括号长度
        max_len = 0
        
        for i in range(1, n):
            if s[i] == ')':
                if s[i-1] == '(':
                    dp[i] = (dp[i-2] if i >= 2 else 0) + 2
                elif i - dp[i-1] > 0 and s[i - dp[i-1] - 1] == '(':
                    dp[i] = dp[i-1] + 2
                    if i - dp[i-1] >= 2:
                        dp[i] += dp[i - dp[i-1] - 2]
            max_len = max(max_len, dp[i])
        
        return max_len
```
