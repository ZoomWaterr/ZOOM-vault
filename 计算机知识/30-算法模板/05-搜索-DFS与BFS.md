# 搜索 · DFS / BFS / 回溯

---

## DFS ⭐⭐

> 一条路走到黑，走不通再回头。

```python
# 全排列
def permute(n):
    cur, used = [], [False]*(n+1)
    def dfs():
        if len(cur) == n:
            print(*cur); return
        for x in range(1, n+1):
            if not used[x]:
                used[x] = True; cur.append(x)
                dfs()
                cur.pop(); used[x] = False   # 回溯！
    dfs()
```

⚠️ `sys.setrecursionlimit(1000000)` · 回溯要恢复现场 · 别忘visited

---

## BFS ⭐⭐

> 一层一层向外扩。**最短距离/最少步数必用。**

```python
from collections import deque

def bfs(start):
    q = deque([start])
    visited = {start}
    steps = 0
    while q:
        for _ in range(len(q)):        # 按层处理
            cur = q.popleft()
            if is_target(cur): return steps
            for nxt in neighbors(cur):
                if nxt not in visited:
                    visited.add(nxt)
                    q.append(nxt)
        steps += 1
    return -1
```

| | DFS | BFS |
|:---|:---|:---|
| 结构 | 栈/递归 | 队列(deque) |
| 用 | "有没有解""所有解" | "最短距离""最少步数" |

---

## 回溯 ⭐⭐

> DFS + 恢复现场。求所有方案专用。

```python
# N皇后核心模板
def backtrack(row):
    if row == n:
        save(); return
    for col in range(n):
        if col in cols or (row+col) in d1 or (row-col) in d2:
            continue
        # 做选择
        board[row][col] = 'Q'; cols.add(col); d1.add(row+col); d2.add(row-col)
        backtrack(row + 1)
        # 恢复现场！
        board[row][col] = '.'; cols.remove(col); d1.remove(row+col); d2.remove(row-col)
```

---

## 🔗 相关

- [[../10-语法手册/10-常用标准库#deque]] · [[08-数据结构#并查集]]
