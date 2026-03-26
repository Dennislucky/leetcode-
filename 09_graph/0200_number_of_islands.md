# 图论题合集

## 200. 岛屿数量 (Number of Islands)

### 思路

遍历二维网格，遇到 '1' 就启动 DFS/BFS 把整个岛屿标记为已访问，计数 +1。

```python
class Solution:
    def numIslands(self, grid: list[list[str]]) -> int:
        if not grid:
            return 0
        
        m, n = len(grid), len(grid[0])
        count = 0
        
        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] != '1':
                return
            grid[i][j] = '0'  # 标记已访问
            dfs(i+1, j)
            dfs(i-1, j)
            dfs(i, j+1)
            dfs(i, j-1)
        
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    count += 1
                    dfs(i, j)
        
        return count
```

**时间 O(m×n)，空间 O(m×n) 递归栈。**

---

## 994. 腐烂的橘子 (Rotting Oranges)

### 思路

**多源 BFS。** 所有腐烂橘子同时开始扩散，BFS 的层数就是分钟数。

```python
from collections import deque

class Solution:
    def orangesRotting(self, grid: list[list[int]]) -> int:
        m, n = len(grid), len(grid[0])
        queue = deque()
        fresh = 0
        
        # 初始化：找到所有腐烂橘子和新鲜橘子
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    queue.append((i, j))
                elif grid[i][j] == 1:
                    fresh += 1
        
        if fresh == 0:
            return 0
        
        minutes = 0
        directions = [(0,1),(0,-1),(1,0),(-1,0)]
        
        while queue:
            minutes += 1
            for _ in range(len(queue)):
                x, y = queue.popleft()
                for dx, dy in directions:
                    nx, ny = x+dx, y+dy
                    if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] == 1:
                        grid[nx][ny] = 2
                        fresh -= 1
                        queue.append((nx, ny))
        
        return minutes - 1 if fresh == 0 else -1
```

**关键：** 多源 BFS — 所有腐烂橘子同时入队作为起点。

---

## 207. 课程表 (Course Schedule)

### 思路

**拓扑排序。** 课程依赖关系是有向图，判断是否有环。

```python
from collections import deque, defaultdict

class Solution:
    def canFinish(self, numCourses: int, prerequisites: list[list[int]]) -> bool:
        # 建图 + 计算入度
        graph = defaultdict(list)
        in_degree = [0] * numCourses
        
        for course, prereq in prerequisites:
            graph[prereq].append(course)
            in_degree[course] += 1
        
        # BFS 拓扑排序
        queue = deque()
        for i in range(numCourses):
            if in_degree[i] == 0:
                queue.append(i)
        
        count = 0
        while queue:
            node = queue.popleft()
            count += 1
            for neighbor in graph[node]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)
        
        return count == numCourses
```

**关键：** 如果所有节点都被处理了（count == numCourses），则无环，可以完成所有课程。

---

## 208. 实现 Trie (前缀树) (Implement Trie)

### 思路

每个节点包含 26 个子节点（对应 26 个字母）和一个结束标记。

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        node = self._find(word)
        return node is not None and node.is_end
    
    def startsWith(self, prefix: str) -> bool:
        return self._find(prefix) is not None
    
    def _find(self, prefix: str):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return None
            node = node.children[char]
        return node
```

**关键要点：**
1. Trie 的每个节点代表一个字符
2. `search` 要求完整匹配（检查 `is_end`）
3. `startsWith` 只要求前缀存在
