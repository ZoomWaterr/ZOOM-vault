# BFS 与队列

**快速结论**
BFS 一层一层扩展，第一次到达目标通常就是最短步数。

**什么时候用**
- 最短路径。
- 最少操作次数。
- 网格扩散。
- 每一步代价相同的状态转换。

**标准模板**
```python
from collections import deque

q = deque([(start, 0)])
seen = {start}

while q:
    cur, step = q.popleft()
    if cur == target:
        print(step)
        break

    for nxt in neighbors(cur):
        if nxt in seen:
            continue
        seen.add(nxt)
        q.append((nxt, step + 1))
```

**网格 BFS 模板**
```python
from collections import deque

dirs = [(1,0), (-1,0), (0,1), (0,-1)]
q = deque([(sr, sc, 0)])
seen = {(sr, sc)}

while q:
    r, c, step = q.popleft()
    for dr, dc in dirs:
        nr, nc = r + dr, c + dc
        if 0 <= nr < n and 0 <= nc < m and grid[nr][nc] != '#':
            if (nr, nc) not in seen:
                seen.add((nr, nc))
                q.append((nr, nc, step + 1))
```

**常见变体**
- 多源 BFS：多个起点同时入队。
- 双向 BFS：起点和终点同时扩展。
- 0-1 BFS：边权只有 0/1，用 deque。

**复杂度**
`O(状态数 + 转移数)`。

**易错点**
- 入队时就标记 seen，避免重复入队。
- 队列用 `deque`，不要用 `list.pop(0)`。
- BFS 适合最短步数，不适合列出所有方案。
