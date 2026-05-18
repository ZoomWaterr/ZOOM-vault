      ---
      aliases:
- DFS
- BFS
- 搜索
- 回溯
- 队列
      tags:
- 算法
- 算法/搜索DFS
      level: 入门算法
      status: 常用
      entry: 算法
      source: 旧AI库+官方资料+算法资料改写
      ---

# 搜索DFS与BFS

## 先看这个

DFS 一条路走到底再回头；BFS 一层一层扩散，常用于最短步数。

## 什么时候用

- DFS：枚举方案、排列组合、回溯。
- BFS：最短路径、最少步数、从起点扩散。

## DFS 模板

```python
def dfs(path, used):
    if len(path) == n:
        print(path)
        return
    for i in range(n):
        if used[i]:
            continue
        used[i] = True
        path.append(i)
        dfs(path, used)
        path.pop()
        used[i] = False
```

## BFS 模板

```python
from collections import deque

q = deque([start])
visited = {start}
while q:
    cur = q.popleft()
    for nxt in neighbors(cur):
        if nxt in visited:
            continue
        visited.add(nxt)
        q.append(nxt)
```

## 我容易摔在哪里

DFS 别忘恢复现场；BFS 别忘 visited，否则会重复走甚至死循环。


## 参考

- Python 官方文档：<https://docs.python.org/3/tutorial/>
- Python 标准库：<https://docs.python.org/3/library/>
- Obsidian Help：<https://help.obsidian.md/>
- OI Wiki：<https://oi-wiki.org/>
- CP-Algorithms：<https://cp-algorithms.com/>
