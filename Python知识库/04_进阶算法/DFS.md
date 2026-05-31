---
created: 2026-05-31
type: note
tags: [进阶算法, DFS, 搜索]
---

# DFS

DFS 是深度优先搜索：一条路走到底，再回头。

## 常见写法

```python
def dfs(x):
    visited[x] = True
    for y in graph[x]:
        if not visited[y]:
            dfs(y)
```

## 题目信号

- 从某个点出发能到哪里。
- 连通块。
- 树的遍历。
- 搜索所有可能路径。

## 核心变量

- `x`：当前所在位置。
- `visited`：防止重复访问。
- `graph`：关系图。

## 易错点

- 图有环时必须有 `visited`。
- 递归深度可能不够。
- DFS 不保证最短路。
