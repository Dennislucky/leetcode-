# 49. 字母异位词分组 (Group Anagrams)

## 题目描述

给你一个字符串数组，请你将**字母异位词**组合在一起。可以按任意顺序返回结果列表。

**字母异位词**是由重新排列源单词的所有字母得到的一个新单词。

**示例：**
```
输入：strs = ["eat","tea","tan","ate","nat","bat"]
输出：[["bat"],["nat","tan"],["ate","eat","tea"]]
```

## 思路分析

### 核心观察

字母异位词有一个关键性质：**它们排序后得到相同的字符串**。

例如：
- "eat" 排序后 → "aet"
- "tea" 排序后 → "aet"  
- "ate" 排序后 → "aet"

所以，我们可以用排序后的字符串作为**分组的 key**。

### 算法步骤

1. 创建一个哈希表，key 是排序后的字符串，value 是属于该组的所有原始字符串列表
2. 遍历每个字符串：
   - 对字符串排序，得到 key
   - 将原始字符串加入对应的列表中
3. 返回哈希表中所有的 value

### 替代方案：字符计数法

还可以用每个字母出现的次数作为 key（比如 "aet" 可以用 (1,0,0,0,1,0,...,1,0,0) 表示），但排序法更简洁。

## 代码实现

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
        # 哈希表：排序后的字符串 -> 原始字符串列表
        groups = defaultdict(list)
        
        for s in strs:
            # 将字符串排序后作为 key
            key = ''.join(sorted(s))
            groups[key].append(s)
        
        return list(groups.values())
```

### 字符计数法（替代实现）

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: list[str]) -> list[list[str]]:
        groups = defaultdict(list)
        
        for s in strs:
            # 用26个字母的计数作为 key
            count = [0] * 26
            for c in s:
                count[ord(c) - ord('a')] += 1
            key = tuple(count)  # list 不能做 key，转为 tuple
            groups[key].append(s)
        
        return list(groups.values())
```

## 复杂度分析

**排序法：**
- **时间复杂度：** O(n × k log k)，n 为字符串个数，k 为最长字符串的长度
- **空间复杂度：** O(n × k)

**字符计数法：**
- **时间复杂度：** O(n × k)，不需要排序
- **空间复杂度：** O(n × k)

## 关键要点

1. **找到异位词的"不变量"**：排序后相同，或者字符频率相同
2. 用这个不变量作为哈希表的 key 来分组
3. `defaultdict(list)` 是 Python 中分组问题的常用工具
4. Python 中 `tuple` 可以作为字典的 key，`list` 不行
