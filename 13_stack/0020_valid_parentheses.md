# 栈题合集

## 20. 有效的括号 (Valid Parentheses)

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        pairs = {')': '(', ']': '[', '}': '{'}
        
        for char in s:
            if char in pairs:
                if not stack or stack[-1] != pairs[char]:
                    return False
                stack.pop()
            else:
                stack.append(char)
        
        return not stack
```

---

## 155. 最小栈 (Min Stack)

每次入栈时同时记录当前最小值。

```python
class MinStack:
    def __init__(self):
        self.stack = []  # 存储 (val, current_min)
    
    def push(self, val: int) -> None:
        current_min = min(val, self.stack[-1][1] if self.stack else val)
        self.stack.append((val, current_min))
    
    def pop(self) -> None:
        self.stack.pop()
    
    def top(self) -> int:
        return self.stack[-1][0]
    
    def getMin(self) -> int:
        return self.stack[-1][1]
```

---

## 394. 字符串解码 (Decode String)

### 思路

遇到 `[` 时，将当前字符串和数字压入栈；遇到 `]` 时弹出并拼接。

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []
        current_str = ""
        current_num = 0
        
        for char in s:
            if char.isdigit():
                current_num = current_num * 10 + int(char)
            elif char == '[':
                stack.append((current_str, current_num))
                current_str = ""
                current_num = 0
            elif char == ']':
                prev_str, num = stack.pop()
                current_str = prev_str + current_str * num
            else:
                current_str += char
        
        return current_str
```

**示例：** `"3[a2[c]]"` → `"accaccacc"`

---

## 739. 每日温度 (Daily Temperatures)

### 思路

**单调递减栈**。栈中存储还未找到更高温度的日期下标。

```python
class Solution:
    def dailyTemperatures(self, temperatures: list[int]) -> list[int]:
        n = len(temperatures)
        result = [0] * n
        stack = []  # 存下标，对应温度单调递减
        
        for i in range(n):
            while stack and temperatures[i] > temperatures[stack[-1]]:
                prev = stack.pop()
                result[prev] = i - prev
            stack.append(i)
        
        return result
```

**过程演示：**
```
temperatures = [73, 74, 75, 71, 69, 72, 76, 73]

i=0: stack=[0]               (73)
i=1: 74>73, pop 0, result[0]=1. stack=[1]
i=2: 75>74, pop 1, result[1]=1. stack=[2]
i=3: stack=[2,3]             (75,71)
i=4: stack=[2,3,4]           (75,71,69)
i=5: 72>69, pop 4, result[4]=1. 72>71, pop 3, result[3]=2. stack=[2,5]
i=6: 76>72, pop 5, result[5]=1. 76>75, pop 2, result[2]=4. stack=[6]
i=7: stack=[6,7]

result = [1,1,4,2,1,1,0,0]
```

---

## 84. 柱状图中最大的矩形 (Largest Rectangle in Histogram)

### 思路（Hard）

**单调递增栈**。对于每个柱子，找到**左边第一个比它矮的**和**右边第一个比它矮的**，确定以该柱子高度为矩形高的宽度。

```python
class Solution:
    def largestRectangleArea(self, heights: list[int]) -> int:
        stack = []  # 存下标，对应高度单调递增
        max_area = 0
        heights.append(0)  # 哨兵，确保所有柱子都会被弹出
        
        for i in range(len(heights)):
            while stack and heights[i] < heights[stack[-1]]:
                h = heights[stack.pop()]
                # 宽度：当前位置 i（右边界）到栈顶（左边界）之间
                w = i if not stack else i - stack[-1] - 1
                max_area = max(max_area, h * w)
            stack.append(i)
        
        heights.pop()  # 恢复原数组
        return max_area
```

**关键：**
- 弹出栈顶时，当前柱子是**右边界**，新栈顶是**左边界**
- 末尾加哨兵 0 确保所有柱子都能被处理
- 这是单调栈的经典应用，也是面试 Hard 题的高频考点
