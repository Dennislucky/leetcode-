# 234. 回文链表 (Palindrome Linked List)

## 题目描述

给你一个单链表的头节点 `head`，请你判断该链表是否为回文链表。要求 O(1) 空间。

## 思路分析

### 快慢指针 + 反转后半部分 ✅

1. **找中点：** 快慢指针找到链表中点
2. **反转后半部分**
3. **比较前后两部分**

## 代码实现

```python
class Solution:
    def isPalindrome(self, head) -> bool:
        # 1. 快慢指针找中点
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        # 2. 反转后半部分
        prev = None
        curr = slow
        while curr:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        
        # 3. 比较前后两部分
        left, right = head, prev
        while right:  # right 是较短的那一半
            if left.val != right.val:
                return False
            left = left.next
            right = right.next
        
        return True
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** O(1)

## 关键要点

1. **快慢指针找中点** + **反转链表** 的组合技
2. 注意奇偶长度的处理：fast 到 None 时 slow 恰好在中间或中间偏右
3. 如需保持链表结构不变，比较完后可以再反转回来
