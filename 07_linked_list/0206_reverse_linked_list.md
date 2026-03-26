# 206. 反转链表 (Reverse Linked List)

## 题目描述

给你单链表的头节点 `head`，请你反转链表，并返回反转后的链表。

## 思路分析

### 迭代法 ✅

使用三个指针：`prev`, `curr`, `next`，逐个翻转指针方向。

### 递归法

递归到链表末尾，然后回溯时逐个翻转。

## 代码实现

### 迭代法

```python
class Solution:
    def reverseList(self, head):
        prev = None
        curr = head
        
        while curr:
            next_node = curr.next  # 保存下一个节点
            curr.next = prev       # 翻转指针
            prev = curr            # prev 前进
            curr = next_node       # curr 前进
        
        return prev
```

### 递归法

```python
class Solution:
    def reverseList(self, head):
        if not head or not head.next:
            return head
        
        new_head = self.reverseList(head.next)
        head.next.next = head  # 让下一个节点指向自己
        head.next = None       # 断开原来的指向
        
        return new_head
```

## 过程演示（迭代）

```
1 → 2 → 3 → None

Step 1: prev=None, curr=1
        None ← 1   2 → 3 → None
        prev=1, curr=2

Step 2: None ← 1 ← 2   3 → None
        prev=2, curr=3

Step 3: None ← 1 ← 2 ← 3
        prev=3, curr=None

返回 prev = 3 (新头节点)
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** 迭代 O(1)，递归 O(n)

## 关键要点

1. **反转链表是链表题的基础中的基础**
2. 迭代法的核心：保存 next → 翻转指针 → 移动指针
3. 很多链表题都需要用到部分反转的操作
