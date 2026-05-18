# DFS、BFS、回溯

tags: #算法/搜索 #算法/DFS #算法/BFS #算法/回溯

## 应该怎么做

问“所有方案”：DFS/回溯。问“最短步数”：BFS。

## DFS

一条路走到底，走不通再退。

```python
def dfs(x):
    if 到达终点:
        记录答案
        return

    for 选择 in 可选项:
        做选择
        dfs(下一层)
        撤销选择
```

## BFS

一层一层扩展，第一次到达就是最短。

```python
from collections import deque

q = deque([(起点, 0)])
visited = {起点}

while q:
    当前, 步数 = q.popleft()
    if 当前 == 终点:
        print(步数)
        break
    for 下一个 in 邻居(当前):
        if 下一个 not in visited:
            visited.add(下一个)
            q.append((下一个, 步数 + 1))
```

## 回溯提醒

选了什么，就撤销什么。忘记恢复现场，后面的分支会被污染。

## 什么时候翻它

- 所有排列/组合。
- 棋盘路径。
- 最短步数。
- 连通块。
