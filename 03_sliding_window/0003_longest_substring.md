# 3. 无重复字符的最长子串 (Longest Substring Without Repeating Characters)

## 题目描述

给定一个字符串 `s`，请你找出其中不含有重复字符的**最长子串**的长度。

**示例：**
```
输入：s = "abcabcbb"
输出：3
解释：无重复字符的最长子串是 "abc"，长度为 3。
```

## 思路分析

### 滑动窗口 ✅

**核心思路：** 维护一个窗口 `[left, right]`，保证窗口内没有重复字符。

**算法步骤：**
1. 使用一个集合（或哈希表）记录窗口内的字符
2. `right` 向右扩展窗口：
   - 如果 `s[right]` 不在窗口中，加入集合，更新最大长度
   - 如果 `s[right]` 已在窗口中，**移动 left** 缩小窗口，直到去掉重复字符
3. 返回最大长度

**优化版本：** 用哈希表记录字符最后出现的位置，遇到重复时直接跳跃 left 到重复位置的下一个，不需要一步步收缩。

## 代码实现

### 基础版（集合）

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_set = set()
        left = 0
        max_len = 0
        
        for right in range(len(s)):
            # 如果有重复，收缩左边界
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1
            
            char_set.add(s[right])
            max_len = max(max_len, right - left + 1)
        
        return max_len
```

### 优化版（哈希表直接跳跃）

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        # 记录字符最后出现的位置
        char_index = {}
        left = 0
        max_len = 0
        
        for right in range(len(s)):
            if s[right] in char_index and char_index[s[right]] >= left:
                # 直接跳到重复字符的下一个位置
                left = char_index[s[right]] + 1
            
            char_index[s[right]] = right
            max_len = max(max_len, right - left + 1)
        
        return max_len
```

## 过程演示

```
s = "abcabcbb"

right=0: 'a', char_index={'a':0}, left=0, len=1
right=1: 'b', char_index={'a':0,'b':1}, left=0, len=2
right=2: 'c', char_index={'a':0,'b':1,'c':2}, left=0, len=3
right=3: 'a' 重复! left=0+1=1, char_index={'a':3,...}, len=3
right=4: 'b' 重复! left=1+1=2, char_index={'b':4,...}, len=3
right=5: 'c' 重复! left=2+1=3, char_index={'c':5,...}, len=3
right=6: 'b' 重复! left=4+1=5, char_index={'b':6,...}, len=2
right=7: 'b' 重复! left=6+1=7, char_index={'b':7,...}, len=1

结果：3 ✅
```

## 复杂度分析

- **时间复杂度：** O(n)。每个字符最多被访问两次（一次 right，一次 left）
- **空间复杂度：** O(min(n, m))，m 为字符集大小

## 关键要点

1. **滑动窗口**是处理子串/子数组问题的核心模板
2. 窗口的**不变量**：窗口内字符不重复
3. 优化版本避免了 while 循环逐步收缩，直接跳到正确位置
4. 注意 `char_index[s[right]] >= left` 这个条件，确保只考虑当前窗口内的重复
