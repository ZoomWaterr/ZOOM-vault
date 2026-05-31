---
created: 2026-05-31
type: note
tags: [模板库, 搜索]
---

# DFS与BFS模板

## DFS

```python
def dfs(x):
    visited[x] = True
    for y in graph[x]:
        if not visited[y]:
            dfs(y)
```

## BFS

```python
from collections import deque

q = deque([start])
visited = {start}

while q:
    x = q.popleft()
    for y in graph[x]:
        if y not in visited:
            visited.add(y)
            q.append(y)
```

相关：[[DFS]]、[[BFS]]
