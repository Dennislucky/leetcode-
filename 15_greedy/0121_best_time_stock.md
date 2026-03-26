# 贪心题合集

## 121. 买卖股票的最佳时机 (Best Time to Buy and Sell Stock)

### 思路

记录到目前为止的最低价，每天计算如果今天卖出的利润。

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        min_price = float('inf')
        max_profit = 0
        
        for price in prices:
            min_price = min(min_price, price)
            max_profit = max(max_profit, price - min_price)
        
        return max_profit
```

**时间 O(n)，空间 O(1)。**

---

## 55. 跳跃游戏 (Jump Game)

### 思路

贪心：维护能到达的**最远位置**。如果最远位置 >= 最后一个下标，就能到达。

```python
class Solution:
    def canJump(self, nums: list[int]) -> bool:
        max_reach = 0
        
        for i in range(len(nums)):
            if i > max_reach:
                return False  # 无法到达位置 i
            max_reach = max(max_reach, i + nums[i])
        
        return True
```

---

## 45. 跳跃游戏 II (Jump Game II)

### 思路

贪心：每次跳跃选择能到达的**最远**的位置。类似 BFS 的层序遍历。

```python
class Solution:
    def jump(self, nums: list[int]) -> int:
        jumps = 0
        current_end = 0   # 当前跳跃的边界
        farthest = 0       # 下一步能到达的最远位置
        
        for i in range(len(nums) - 1):
            farthest = max(farthest, i + nums[i])
            
            if i == current_end:  # 到达当前跳跃的边界，必须跳
                jumps += 1
                current_end = farthest
        
        return jumps
```

**直觉：** 把数组分成若干段，每段代表一次跳跃能覆盖的范围。

---

## 763. 划分字母区间 (Partition Labels)

### 思路

1. 记录每个字母最后出现的位置
2. 贪心遍历，维护当前片段的最远边界

```python
class Solution:
    def partitionLabels(self, s: str) -> list[int]:
        # 每个字符最后出现的下标
        last = {c: i for i, c in enumerate(s)}
        
        result = []
        start = 0
        end = 0
        
        for i, c in enumerate(s):
            end = max(end, last[c])
            if i == end:
                result.append(end - start + 1)
                start = end + 1
        
        return result
```

**示例：** `"ababcbacadefegdehijhklij"`
```
片段1: "ababcbaca" (a最远8, b最远5, c最远7 → end=8)
片段2: "defegde"   (d最远14, e最远15, f最远11, g最远13 → end=15)
片段3: "hijhklij"  (h最远19, i最远22, j最远23, k最远20, l最远21 → end=23)

结果：[9, 7, 8]
```
