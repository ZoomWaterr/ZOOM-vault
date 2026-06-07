---
created: 2026-06-08
type: deep-note
source: DataStructure.pdf pages 67-78
tags: [Python, 图, DFS, BFS, 邻接表]
---

# 图的表示、DFS、BFS

原 PDF 对应页：DataStructure.pdf 第 67-78 页。原文讲图的术语、邻接矩阵、邻接表、十字链表、邻接多重表，以及 DFS/BFS。Python 刷题最常用的是邻接表。

![图原图](../assets/data_structure/data_structure_p070_01.png)

## 一句话抓本质

图就是“点”和“边”；邻接表回答“从这个点能去哪些点”，DFS 负责一路走到底，BFS 负责一层一层扩散。

## 邻接表

```python
def build_undirected_graph(n: int, edges: list[tuple[int, int]]) -> list[list[int]]:
    graph = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    return graph

print(build_undirected_graph(4, [(0, 1), (0, 2), (1, 3)]))
```

变量角色：

| 变量 | 角色 |
|---|---|
| n | 顶点数量 |
| edges | 边列表 |
| graph[u] | u 的所有邻居 |
| visited | 是否访问过，防止重复和死循环 |

## 邻接矩阵

```python
def build_matrix(n: int, edges: list[tuple[int, int]]) -> list[list[int]]:
    matrix = [[0] * n for _ in range(n)]
    for a, b in edges:
        matrix[a][b] = 1
        matrix[b][a] = 1
    return matrix
```

| 表示 | 查 a-b 是否有边 | 遍历 a 的邻居 | 空间 |
|---|---:|---:|---:|
| 邻接矩阵 | O(1) | O(n) | O(n²) |
| 邻接表 | O(deg) | O(deg) | O(n+m) |

比赛里稀疏图更多，邻接表更常用。

## DFS 递归版

```python
def dfs_recursive(graph: list[list[int]], start: int) -> list[int]:
    visited = [False] * len(graph)
    order: list[int] = []

    def dfs(u: int) -> None:
        visited[u] = True
        order.append(u)
        for v in graph[u]:
            if not visited[v]:
                dfs(v)

    dfs(start)
    return order
```

## DFS 手动栈版

```python
def dfs_stack(graph: list[list[int]], start: int) -> list[int]:
    visited = [False] * len(graph)
    stack = [start]
    order: list[int] = []
    while stack:
        u = stack.pop()
        if visited[u]:
            continue
        visited[u] = True
        order.append(u)
        for v in reversed(graph[u]):
            if not visited[v]:
                stack.append(v)
    return order
```

## BFS 队列版

```python
from collections import deque

def bfs(graph: list[list[int]], start: int) -> list[int]:
    visited = [False] * len(graph)
    q = deque([start])
    visited[start] = True
    order: list[int] = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in graph[u]:
            if not visited[v]:
                visited[v] = True
                q.append(v)
    return order
```

## 手推 BFS

图：0-1，0-2，1-3。从 0 开始。

| 步骤 | 队列 | 弹出 | 新加入 | order |
|---|---|---:|---|---|
| 初始 | [0] | - | - | [] |
| 1 | [] | 0 | 1, 2 | [0] |
| 2 | [2] | 1 | 3 | [0, 1] |
| 3 | [3] | 2 | 无 | [0, 1, 2] |
| 4 | [] | 3 | 无 | [0, 1, 2, 3] |

## 连通分量

```python
def count_components(n: int, edges: list[tuple[int, int]]) -> int:
    graph = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)

    visited = [False] * n

    def dfs(u: int) -> None:
        visited[u] = True
        for v in graph[u]:
            if not visited[v]:
                dfs(v)

    ans = 0
    for i in range(n):
        if not visited[i]:
            ans += 1
            dfs(i)
    return ans
```

## 常见错法

错误 1：无向图建边只加 graph[a].append(b)，忘了 b 到 a。

错误 2：BFS 出队时才标记 visited，导致同一个点被多个父节点重复入队。一般入队时就标记。

错误 3：递归 DFS 在深图上爆栈。Python 默认递归深度有限，链状图很长时要改成手动栈，或谨慎设置递归深度。

## 题目信号

- “能不能到达”：DFS/BFS。
- “最少几步”：BFS。
- “有几个连通块”：遍历所有点，每次从未访问点出发。
- “网格扩散”：把格子当图的节点。

## 验收练习

1. 给定 n 和 edges，判断 0 是否能到 n-1。
2. 写网格 BFS 求从 S 到 T 的最短步数。
3. 对比邻接矩阵和邻接表适合什么图。
