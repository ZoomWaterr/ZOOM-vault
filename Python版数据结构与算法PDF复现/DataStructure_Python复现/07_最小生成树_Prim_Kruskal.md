---
created: 2026-06-08
type: deep-note
source: DataStructure.pdf pages 79-86
tags: [Python, 图, 最小生成树, Prim, Kruskal, 并查集]
---

# 最小生成树：Prim 与 Kruskal

原 PDF 对应页：DataStructure.pdf 第 79-86 页。最小生成树只讨论“无向连通带权图”：要用最小总代价把所有点连起来，并且不能有环。

![Prim 原图](../assets/data_structure/data_structure_p080_01.png)

## 一句话抓本质

Prim 像“从一个岛慢慢扩张”，Kruskal 像“按边从小到大挑，只要不成环就要”。

## 最小生成树是什么

n 个点的生成树一定有 n - 1 条边。如果边少了，连不全；如果边多了，必有环。

| 目标 | 要求 |
|---|---|
| 连通所有点 | 每个点都在树里 |
| 不成环 | 只有 n - 1 条边 |
| 权值最小 | 所选边权值和最小 |

## Prim

```python
from heapq import heappop, heappush

def prim(n: int, edges: list[tuple[int, int, int]]) -> int | None:
    graph = [[] for _ in range(n)]
    for a, b, w in edges:
        graph[a].append((w, b))
        graph[b].append((w, a))

    visited = [False] * n
    heap = [(0, 0)]
    total = 0
    count = 0

    while heap and count < n:
        w, u = heappop(heap)
        if visited[u]:
            continue
        visited[u] = True
        total += w
        count += 1
        for nw, v in graph[u]:
            if not visited[v]:
                heappush(heap, (nw, v))

    if count != n:
        return None
    return total
```

变量角色：

| 变量 | 角色 |
|---|---|
| visited | 已经加入生成树的点集合 |
| heap | 从树内连向树外的候选边 |
| total | 当前总代价 |
| count | 已加入多少个点 |

## Kruskal 与并查集

```python
class DSU:
    def __init__(self, n: int):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x: int) -> int:
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a: int, b: int) -> bool:
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def kruskal(n: int, edges: list[tuple[int, int, int]]) -> int | None:
    dsu = DSU(n)
    total = 0
    used = 0
    for a, b, w in sorted(edges, key=lambda x: x[2]):
        if dsu.union(a, b):
            total += w
            used += 1
            if used == n - 1:
                return total
    return None
```

## 手推 Kruskal

边：0-1(1)，1-2(2)，0-2(4)，2-3(3)

| 顺序 | 边 | 是否选择 | 原因 |
|---:|---|---|---|
| 1 | 0-1(1) | 选 | 不成环 |
| 2 | 1-2(2) | 选 | 不成环 |
| 3 | 2-3(3) | 选 | 已连 4 点 |
| 4 | 0-2(4) | 不看 | 已有 n-1 条边 |

总权值 1 + 2 + 3 = 6。

## Prim 和 Kruskal 怎么选

| 算法 | 更像什么 | 适合 |
|---|---|---|
| Prim | 点集扩张 | 稠密图、从某点开始扩张 |
| Kruskal | 按边排序 | 稀疏图、边列表输入 |

Python 刷题里，Kruskal + 并查集非常常用，因为很多题直接给边列表。

## 常见错法

错误 1：有向图上套 MST。最小生成树通常是无向图概念。

错误 2：Kruskal 不判断成环，最后得到的不是树。

错误 3：图不连通时仍然返回 total。必须检查 used == n - 1 或 count == n。

## 题目信号

- “连接所有城市的最小成本”。
- “铺设光缆/道路/管道”。
- “保留一些边使得所有点连通且总代价最小”。

## 验收练习

1. 手写并查集 find 路径压缩过程。
2. 用 Kruskal 求 5 个点的 MST 总权值。
3. 解释为什么 MST 有 n - 1 条边。
