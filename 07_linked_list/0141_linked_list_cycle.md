# 141. 环形链表 (Linked List Cycle)

## 题目描述

给你一个链表的头节点 `head`，判断链表中是否有环。

## 思路分析

### 快慢指针（Floyd 判圈法）✅

- 慢指针每次走 1 步，快指针每次走 2 步
- 如果有环，快指针一定会追上慢指针（相遇）
- 如果无环，快指针会先到 None

## 代码实现

```python
class Solution:
    def hasCycle(self, head) -> bool:
        slow, fast = head, head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        
        return False
```

## 复杂度分析

- **时间复杂度：** O(n)
- **空间复杂度：** O(1)

---

# 142. 环形链表 II (Linked List Cycle II)

## 题目描述

给定一个链表的头节点 `head`，返回链表开始入环的第一个节点。如果无环，返回 `null`。

## 思路分析

### Floyd 算法第二阶段 ✅

**数学推导：**
设链表头到环入口距离为 `a`，环入口到相遇点距离为 `b`，环长为 `c`。

相遇时：
- 慢指针走了 `a + b`
- 快指针走了 `a + b + nc`（多走了 n 圈）
- 快指针速度是慢指针2倍：`2(a+b) = a+b+nc` → `a+b = nc` → `a = nc-b = (n-1)c + (c-b)`

所以从头节点走 `a` 步等于从相遇点走 `c-b+（n-1）圈` → 两者在**环入口相遇**！

## 代码实现

```python
class Solution:
    def detectCycle(self, head):
        slow, fast = head, head
        
        # 第一阶段：找到相遇点
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                # 第二阶段：找环入口
                ptr = head
                while ptr != slow:
                    ptr = ptr.next
                    slow = slow.next
                return ptr
        
        return None
```

## 关键要点

1. **Floyd 算法**分两个阶段：先判环，再找入口
2. 数学推导保证了第二阶段的正确性
3. 这两道题几乎总是一起考
