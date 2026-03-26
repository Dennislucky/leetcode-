# 21. 合并两个有序链表 (Merge Two Sorted Lists)

## 题目描述

将两个升序链表合并为一个新的升序链表并返回。

## 代码实现

```python
class Solution:
    def mergeTwoLists(self, list1, list2):
        dummy = ListNode(0)
        curr = dummy
        
        while list1 and list2:
            if list1.val <= list2.val:
                curr.next = list1
                list1 = list1.next
            else:
                curr.next = list2
                list2 = list2.next
            curr = curr.next
        
        # 接上剩余部分
        curr.next = list1 if list1 else list2
        
        return dummy.next
```

## 关键要点

1. **dummy 节点**简化头节点的处理
2. 归并排序中 merge 步骤的链表版本
3. 时间 O(m+n)，空间 O(1)

---

# 2. 两数相加 (Add Two Numbers)

## 题目描述

给你两个非空链表，表示两个非负整数。它们每位数字都是**逆序**存储。请你将两个数相加，并以相同形式返回一个表示和的链表。

## 代码实现

```python
class Solution:
    def addTwoNumbers(self, l1, l2):
        dummy = ListNode(0)
        curr = dummy
        carry = 0
        
        while l1 or l2 or carry:
            val = carry
            if l1:
                val += l1.val
                l1 = l1.next
            if l2:
                val += l2.val
                l2 = l2.next
            
            carry = val // 10
            curr.next = ListNode(val % 10)
            curr = curr.next
        
        return dummy.next
```

## 关键要点

1. 逐位相加，别忘了**进位 carry**
2. 循环条件包含 `carry`，处理最后的进位
3. dummy 节点简化代码

---

# 19. 删除链表的倒数第 N 个结点 (Remove Nth Node From End)

## 思路分析

**快慢指针：** 让 fast 先走 n 步，然后 fast 和 slow 一起走，当 fast 到末尾时，slow 到达目标位置的前一个。

## 代码实现

```python
class Solution:
    def removeNthFromEnd(self, head, n):
        dummy = ListNode(0, head)
        fast = slow = dummy
        
        # fast 先走 n+1 步
        for _ in range(n + 1):
            fast = fast.next
        
        # 一起走到末尾
        while fast:
            fast = fast.next
            slow = slow.next
        
        # 删除 slow 的下一个节点
        slow.next = slow.next.next
        
        return dummy.next
```

## 关键要点

1. dummy 节点处理删除头节点的边界情况
2. fast 先走 n+1 步（而不是 n 步），这样 slow 停在要删除节点的**前一个**

---

# 24. 两两交换链表中的节点 (Swap Nodes in Pairs)

## 代码实现

```python
class Solution:
    def swapPairs(self, head):
        dummy = ListNode(0, head)
        prev = dummy
        
        while prev.next and prev.next.next:
            a = prev.next
            b = prev.next.next
            
            # 交换
            prev.next = b
            a.next = b.next
            b.next = a
            
            prev = a  # 移动 prev 到下一对的前面
        
        return dummy.next
```

---

# 25. K 个一组翻转链表 (Reverse Nodes in k-Group)

## 思路分析

**Hard 题**。每 k 个节点为一组进行翻转，不足 k 个的保持原样。

## 代码实现

```python
class Solution:
    def reverseKGroup(self, head, k):
        dummy = ListNode(0, head)
        prev_group = dummy
        
        while True:
            # 检查是否有 k 个节点
            kth = prev_group
            for _ in range(k):
                kth = kth.next
                if not kth:
                    return dummy.next
            
            next_group = kth.next
            
            # 翻转这 k 个节点
            prev, curr = next_group, prev_group.next
            for _ in range(k):
                next_node = curr.next
                curr.next = prev
                prev = curr
                curr = next_node
            
            # 连接
            first_in_group = prev_group.next  # 翻转后变成最后一个
            prev_group.next = prev  # prev 是翻转后的头
            prev_group = first_in_group
        
        return dummy.next
```

## 关键要点

1. 先检查够不够 k 个，不够就不翻转
2. 翻转局部链表后重新连接前后部分
3. 这是链表操作的集大成者

---

# 138. 随机链表的复制 (Copy List with Random Pointer)

## 思路分析

使用哈希表映射原节点到新节点，两遍遍历：第一遍创建节点，第二遍连接指针。

## 代码实现

```python
class Solution:
    def copyRandomList(self, head):
        if not head:
            return None
        
        # 哈希表：原节点 -> 新节点
        old_to_new = {}
        
        # 第一遍：创建所有新节点
        curr = head
        while curr:
            old_to_new[curr] = Node(curr.val)
            curr = curr.next
        
        # 第二遍：连接 next 和 random 指针
        curr = head
        while curr:
            old_to_new[curr].next = old_to_new.get(curr.next)
            old_to_new[curr].random = old_to_new.get(curr.random)
            curr = curr.next
        
        return old_to_new[head]
```

---

# 148. 排序链表 (Sort List)

## 思路分析

**归并排序**最适合链表排序（O(n log n)，O(1) 空间）。

1. 快慢指针找中点，断开链表
2. 递归排序左右两半
3. 合并两个有序链表

## 代码实现

```python
class Solution:
    def sortList(self, head):
        if not head or not head.next:
            return head
        
        # 找中点并断开
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        mid = slow.next
        slow.next = None
        
        # 递归排序
        left = self.sortList(head)
        right = self.sortList(mid)
        
        # 合并
        dummy = ListNode(0)
        curr = dummy
        while left and right:
            if left.val <= right.val:
                curr.next = left
                left = left.next
            else:
                curr.next = right
                right = right.next
            curr = curr.next
        curr.next = left or right
        
        return dummy.next
```

---

# 23. 合并 K 个升序链表 (Merge k Sorted Lists)

## 思路分析

使用**最小堆**每次取出最小的节点。

## 代码实现

```python
import heapq

class Solution:
    def mergeKLists(self, lists):
        dummy = ListNode(0)
        curr = dummy
        
        # 初始化堆（val, index, node）index 用于防止 val 相同时比较 node
        heap = []
        for i, node in enumerate(lists):
            if node:
                heapq.heappush(heap, (node.val, i, node))
        
        while heap:
            val, i, node = heapq.heappop(heap)
            curr.next = node
            curr = curr.next
            if node.next:
                heapq.heappush(heap, (node.next.val, i, node.next))
        
        return dummy.next
```

## 复杂度

- **时间：** O(N log k)，N 是所有节点总数，k 是链表个数
- **空间：** O(k)，堆的大小

---

# 146. LRU 缓存 (LRU Cache)

## 思路分析

**哈希表 + 双向链表**。哈希表 O(1) 查找，双向链表 O(1) 插入/删除。

## 代码实现

```python
class DLinkedNode:
    def __init__(self, key=0, val=0):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}  # key -> DLinkedNode
        # 虚拟头尾节点
        self.head = DLinkedNode()
        self.tail = DLinkedNode()
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self._move_to_head(node)
        return node.val
    
    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.val = value
            self._move_to_head(node)
        else:
            node = DLinkedNode(key, value)
            self.cache[key] = node
            self._add_to_head(node)
            if len(self.cache) > self.capacity:
                removed = self._remove_tail()
                del self.cache[removed.key]
    
    def _add_to_head(self, node):
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    def _remove_node(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev
    
    def _move_to_head(self, node):
        self._remove_node(node)
        self._add_to_head(node)
    
    def _remove_tail(self):
        node = self.tail.prev
        self._remove_node(node)
        return node
```

## 关键要点

1. LRU 是面试超高频题
2. 虚拟头尾节点避免边界判断
3. 所有操作都是 O(1)
