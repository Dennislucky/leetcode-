# 160. 相交链表 (Intersection of Two Linked Lists)

## 题目描述

给你两个单链表的头节点 `headA` 和 `headB`，请你找出并返回两个单链表相交的起始节点。如果不存在相交节点，返回 `null`。

## 思路分析

### 双指针"浪漫相遇"法 ✅

**核心思路：** 让两个指针分别从 A 和 B 出发，走完自己的链表后再走对方的链表。如果有交点，它们一定会在交点相遇。

**为什么？**
- 指针 A 走过的路程：`a + c + b`
- 指针 B 走过的路程：`b + c + a`
- 两者路程相同！所以一定同时到达交点

其中 a 是 A 独有部分，b 是 B 独有部分，c 是公共部分。

## 代码实现

```python
class Solution:
    def getIntersectionNode(self, headA, headB):
        a, b = headA, headB
        
        while a != b:
            a = a.next if a else headB
            b = b.next if b else headA
        
        return a  # 要么是交点，要么都是 None
```

## 复杂度分析

- **时间复杂度：** O(m + n)
- **空间复杂度：** O(1)

## 关键要点

1. 代码极其简洁，核心是"`走完自己的路，走对方的路`"
2. 如果没有交点，两个指针最终都会走到 None（同时到达），循环自然结束
