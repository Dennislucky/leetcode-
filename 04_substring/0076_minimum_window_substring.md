# 76. 最小覆盖子串 (Minimum Window Substring)

## 题目描述

给你一个字符串 `s`、一个字符串 `t`。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果不存在，返回空字符串。

**示例：**
```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```

## 思路分析

### 滑动窗口 ✅

**经典的可变长度滑动窗口问题。**

**算法步骤：**
1. 统计 `t` 中每个字符的需求量
2. 用 `right` 扩展窗口，直到窗口包含 `t` 的所有字符
3. 用 `left` 收缩窗口，尽量缩小窗口，同时保持满足条件
4. 记录满足条件时的最小窗口

**关键变量：**
- `need`：记录 t 中每个字符还需要多少个
- `missing`：还缺少多少个字符（当 missing == 0 时，窗口满足条件）

## 代码实现

```python
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if not s or not t:
            return ""
        
        # t 中每个字符的需求量
        need = Counter(t)
        missing = len(t)  # 还缺少的字符总数
        
        left = 0
        min_start = 0
        min_len = float('inf')
        
        for right in range(len(s)):
            # 扩展窗口
            if need[s[right]] > 0:
                missing -= 1
            need[s[right]] -= 1
            
            # 当窗口满足条件时，尝试收缩
            while missing == 0:
                # 更新最小窗口
                window_len = right - left + 1
                if window_len < min_len:
                    min_len = window_len
                    min_start = left
                
                # 收缩左边界
                need[s[left]] += 1
                if need[s[left]] > 0:
                    missing += 1
                left += 1
        
        return "" if min_len == float('inf') else s[min_start:min_start + min_len]
```

## 过程演示

```
s = "ADOBECODEBANC", t = "ABC"
need = {'A':1, 'B':1, 'C':1}, missing = 3

right=0 'A': need['A']=0, missing=2, 窗口"A"
right=1 'D': need['D']=-1, missing=2
right=2 'O': need['O']=-1, missing=2
right=3 'B': need['B']=0, missing=1, 窗口"ADOB"
right=4 'E': need['E']=-1, missing=1
right=5 'C': need['C']=0, missing=0 ✓ 窗口"ADOBEC"(长度6)
  收缩: left=0'A', need['A']=1>0, missing=1, left=1
  
right=9 'A': need['A']=0, missing=0 ✓ 窗口"DOBECODEBA"
  收缩到 left=5...
  
right=10 'N': ...
right=11 'C': missing=0 ✓ 窗口"BANC"(长度4) ← 最短！

结果："BANC" ✅
```

## 复杂度分析

- **时间复杂度：** O(|s| + |t|)
- **空间复杂度：** O(|字符集|)

## 关键要点

1. **滑动窗口模板：右扩展，左收缩**
2. `missing` 计数器是判断窗口是否满足条件的关键
3. `need[char]` 可能为负数，表示窗口中该字符的多余量
4. 只有 `need[char] > 0` 时减少/增加 `missing`，避免多余字符影响判断
5. 这道 Hard 题是滑动窗口的终极模板题
