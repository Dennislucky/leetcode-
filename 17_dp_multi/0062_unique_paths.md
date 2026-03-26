# 多维动态规划题合集

## 62. 不同路径 (Unique Paths)

**状态：** `dp[i][j]` = 从左上角到 (i,j) 的路径数  
**转移：** `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [1] * n  # 第一行全是 1
        
        for i in range(1, m):
            for j in range(1, n):
                dp[j] += dp[j-1]
        
        return dp[n-1]
```

空间优化为一维数组。

---

## 64. 最小路径和 (Minimum Path Sum)

**转移：** `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`

```python
class Solution:
    def minPathSum(self, grid: list[list[int]]) -> int:
        m, n = len(grid), len(grid[0])
        
        # 初始化第一行
        for j in range(1, n):
            grid[0][j] += grid[0][j-1]
        # 初始化第一列
        for i in range(1, m):
            grid[i][0] += grid[i-1][0]
        
        for i in range(1, m):
            for j in range(1, n):
                grid[i][j] += min(grid[i-1][j], grid[i][j-1])
        
        return grid[m-1][n-1]
```

---

## 5. 最长回文子串 (Longest Palindromic Substring)

### 方法一：中心扩展法（推荐）

从每个位置（和每对相邻位置）向两边扩展，找最长回文。

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        def expand(left, right):
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            return s[left+1:right]
        
        result = ""
        for i in range(len(s)):
            # 奇数长度回文
            odd = expand(i, i)
            # 偶数长度回文
            even = expand(i, i + 1)
            
            longer = odd if len(odd) > len(even) else even
            if len(longer) > len(result):
                result = longer
        
        return result
```

### 方法二：DP

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        dp = [[False] * n for _ in range(n)]
        start, max_len = 0, 1
        
        # 所有单个字符都是回文
        for i in range(n):
            dp[i][i] = True
        
        # 从短到长枚举子串长度
        for length in range(2, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1
                if s[i] == s[j]:
                    if length == 2 or dp[i+1][j-1]:
                        dp[i][j] = True
                        if length > max_len:
                            start, max_len = i, length
        
        return s[start:start + max_len]
```

---

## 1143. 最长公共子序列 (Longest Common Subsequence)

**状态：** `dp[i][j]` = text1 前 i 个字符与 text2 前 j 个字符的 LCS 长度

**转移：**
- 如果 `text1[i-1] == text2[j-1]`：`dp[i][j] = dp[i-1][j-1] + 1`
- 否则：`dp[i][j] = max(dp[i-1][j], dp[i][j-1])`

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        return dp[m][n]
```

---

## 72. 编辑距离 (Edit Distance)

### 思路

**经典二维 DP。** 三种操作：插入、删除、替换。

**状态：** `dp[i][j]` = word1 前 i 个字符变成 word2 前 j 个字符的最少操作数

**转移：**
- 如果 `word1[i-1] == word2[j-1]`：`dp[i][j] = dp[i-1][j-1]`（不需要操作）
- 否则：`dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`
  - `dp[i-1][j] + 1`：删除 word1[i-1]
  - `dp[i][j-1] + 1`：插入 word2[j-1]
  - `dp[i-1][j-1] + 1`：替换

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        # 初始化：空串到非空串的编辑距离
        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
        
        return dp[m][n]
```

### 过程演示

```
word1 = "horse", word2 = "ros"

    ""  r  o  s
""   0  1  2  3
h    1  1  2  3
o    2  2  1  2
r    3  2  2  2
s    4  3  3  2
e    5  4  4  3

结果：3（horse → rorse → rose → ros）
```

**编辑距离是面试中最经典的二维 DP 题之一，理解它就理解了大部分 DP 问题的精髓。**
