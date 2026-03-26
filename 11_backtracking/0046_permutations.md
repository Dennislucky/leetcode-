# 回溯法题合集

## 回溯法模板

```python
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.append(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

---

## 46. 全排列 (Permutations)

```python
class Solution:
    def permute(self, nums: list[int]) -> list[list[int]]:
        result = []
        
        def backtrack(path, remaining):
            if not remaining:
                result.append(path[:])
                return
            for i in range(len(remaining)):
                path.append(remaining[i])
                backtrack(path, remaining[:i] + remaining[i+1:])
                path.pop()
        
        backtrack([], nums)
        return result
```

---

## 78. 子集 (Subsets)

```python
class Solution:
    def subsets(self, nums: list[int]) -> list[list[int]]:
        result = []
        
        def backtrack(start, path):
            result.append(path[:])  # 每个状态都是一个子集
            for i in range(start, len(nums)):
                path.append(nums[i])
                backtrack(i + 1, path)
                path.pop()
        
        backtrack(0, [])
        return result
```

**关键区别：** 排列—每个位置都从头选；子集—从 start 开始选，避免重复。

---

## 17. 电话号码的字母组合 (Letter Combinations of a Phone Number)

```python
class Solution:
    def letterCombinations(self, digits: str) -> list[str]:
        if not digits:
            return []
        
        phone = {'2':'abc','3':'def','4':'ghi','5':'jkl',
                 '6':'mno','7':'pqrs','8':'tuv','9':'wxyz'}
        result = []
        
        def backtrack(index, path):
            if index == len(digits):
                result.append(''.join(path))
                return
            for char in phone[digits[index]]:
                path.append(char)
                backtrack(index + 1, path)
                path.pop()
        
        backtrack(0, [])
        return result
```

---

## 39. 组合总和 (Combination Sum)

数字可以**重复使用**。

```python
class Solution:
    def combinationSum(self, candidates: list[int], target: int) -> list[list[int]]:
        result = []
        
        def backtrack(start, path, remaining):
            if remaining == 0:
                result.append(path[:])
                return
            if remaining < 0:
                return
            for i in range(start, len(candidates)):
                path.append(candidates[i])
                backtrack(i, path, remaining - candidates[i])  # i 而不是 i+1，允许重复
                path.pop()
        
        backtrack(0, [], target)
        return result
```

**关键：** `backtrack(i, ...)` 而不是 `backtrack(i+1, ...)`，因为允许重复使用同一个数。

---

## 22. 括号生成 (Generate Parentheses)

```python
class Solution:
    def generateParenthesis(self, n: int) -> list[str]:
        result = []
        
        def backtrack(path, open_count, close_count):
            if len(path) == 2 * n:
                result.append(''.join(path))
                return
            
            if open_count < n:
                path.append('(')
                backtrack(path, open_count + 1, close_count)
                path.pop()
            
            if close_count < open_count:
                path.append(')')
                backtrack(path, open_count, close_count + 1)
                path.pop()
        
        backtrack([], 0, 0)
        return result
```

**剪枝条件：**
- 左括号数 < n 时可以加左括号
- 右括号数 < 左括号数时可以加右括号

---

## 79. 单词搜索 (Word Search)

```python
class Solution:
    def exist(self, board: list[list[str]], word: str) -> bool:
        m, n = len(board), len(board[0])
        
        def dfs(i, j, k):
            if k == len(word):
                return True
            if i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[k]:
                return False
            
            temp = board[i][j]
            board[i][j] = '#'  # 标记已访问
            
            found = (dfs(i+1,j,k+1) or dfs(i-1,j,k+1) or
                     dfs(i,j+1,k+1) or dfs(i,j-1,k+1))
            
            board[i][j] = temp  # 回溯
            return found
        
        for i in range(m):
            for j in range(n):
                if dfs(i, j, 0):
                    return True
        return False
```

---

## 131. 分割回文串 (Palindrome Partitioning)

```python
class Solution:
    def partition(self, s: str) -> list[list[str]]:
        result = []
        
        def backtrack(start, path):
            if start == len(s):
                result.append(path[:])
                return
            for end in range(start + 1, len(s) + 1):
                substring = s[start:end]
                if substring == substring[::-1]:  # 是回文
                    path.append(substring)
                    backtrack(end, path)
                    path.pop()
        
        backtrack(0, [])
        return result
```

---

## 51. N 皇后 (N-Queens)

```python
class Solution:
    def solveNQueens(self, n: int) -> list[list[str]]:
        result = []
        board = [['.' ] * n for _ in range(n)]
        cols = set()      # 占用的列
        diag1 = set()     # 占用的主对角线 (row - col)
        diag2 = set()     # 占用的副对角线 (row + col)
        
        def backtrack(row):
            if row == n:
                result.append([''.join(r) for r in board])
                return
            
            for col in range(n):
                if col in cols or (row-col) in diag1 or (row+col) in diag2:
                    continue
                
                board[row][col] = 'Q'
                cols.add(col)
                diag1.add(row - col)
                diag2.add(row + col)
                
                backtrack(row + 1)
                
                board[row][col] = '.'
                cols.remove(col)
                diag1.remove(row - col)
                diag2.remove(row + col)
        
        backtrack(0)
        return result
```

**关键：** 用集合记录被攻击的列和对角线，判断快速。主对角线用 `row-col`，副对角线用 `row+col`。
