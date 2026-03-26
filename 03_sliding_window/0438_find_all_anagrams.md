# 438. 找到字符串中所有字母异位词 (Find All Anagrams in a String)

## 题目描述

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的**异位词**的子串，返回这些子串的起始索引。

**示例：**
```
输入：s = "cbaebabacd", p = "abc"
输出：[0, 6]
解释：
起始索引为 0 的子串是 "cba"，它是 "abc" 的异位词。
起始索引为 6 的子串是 "bac"，它是 "abc" 的异位词。
```

## 思路分析

### 固定长度滑动窗口 ✅

**核心观察：** 异位词的长度一定等于 `p` 的长度，所以窗口大小固定为 `len(p)`。

**算法步骤：**
1. 统计 `p` 中每个字符的频率
2. 维护一个长度为 `len(p)` 的滑动窗口，统计窗口内每个字符的频率
3. 每次窗口滑动时，比较窗口频率和 `p` 的频率是否相同
4. 相同则记录起始位置

**优化：** 不需要每次都比较完整的频率表，可以维护一个 `match` 计数器，追踪有多少个字符的频率已经匹配。

## 代码实现

### 简洁版

```python
from collections import Counter

class Solution:
    def findAnagrams(self, s: str, p: str) -> list[int]:
        if len(s) < len(p):
            return []
        
        p_count = Counter(p)
        window_count = Counter(s[:len(p)])
        result = []
        
        if window_count == p_count:
            result.append(0)
        
        for i in range(len(p), len(s)):
            # 加入右边新字符
            window_count[s[i]] += 1
            # 移除左边旧字符
            left_char = s[i - len(p)]
            window_count[left_char] -= 1
            if window_count[left_char] == 0:
                del window_count[left_char]
            
            if window_count == p_count:
                result.append(i - len(p) + 1)
        
        return result
```

### 优化版（match 计数器）

```python
from collections import Counter

class Solution:
    def findAnagrams(self, s: str, p: str) -> list[int]:
        if len(s) < len(p):
            return []
        
        p_count = Counter(p)
        window_count = Counter()
        result = []
        matched = 0  # 有多少种字符的频率完全匹配
        required = len(p_count)  # 需要匹配的字符种类数
        
        for i in range(len(s)):
            # 扩展窗口右边
            char = s[i]
            window_count[char] += 1
            if char in p_count and window_count[char] == p_count[char]:
                matched += 1
            
            # 收缩窗口左边（保持窗口大小为 len(p)）
            if i >= len(p):
                left_char = s[i - len(p)]
                if left_char in p_count and window_count[left_char] == p_count[left_char]:
                    matched -= 1
                window_count[left_char] -= 1
            
            # 检查是否所有字符都匹配
            if matched == required:
                result.append(i - len(p) + 1)
        
        return result
```

## 复杂度分析

- **时间复杂度：** O(n)，n 为 s 的长度。每个字符进出窗口各一次
- **空间复杂度：** O(1)，字符集大小固定（26个字母）

## 关键要点

1. **固定长度滑动窗口**：窗口大小 = `len(p)`，不需要动态调整
2. 每次窗口滑动：**右加一个，左减一个**
3. `match` 计数器优化将每次比较从 O(26) 降为 O(1)
4. 这道题是滑动窗口的模板题，掌握后可以举一反三
