# 搜索 · DFS / BFS / 回溯

---

## DFS 深度优先搜索 ⭐⭐

> 💡 **一条路走到黑**，走不通再回头。用递归（或栈）实现。

```
遍历顺序（先深入，再回头）：

      A        A → B → D → E → C
     / \       └ 左边一路到底 ┘  └ 回溯到C
    B   C
   / \
  D   E
```

### 📝 全排列模板

```python
def permutations(n: int):
    """输出 1~n 的所有排列"""
    cur = []
    used = [False] * (n + 1)

    def dfs():
        if len(cur) == n:
            print(*cur)
            return

        for num in range(1, n + 1):
            if not used[num]:
                used[num] = True        # 做选择
                cur.append(num)
                dfs()                   # 深入
                cur.pop()               # 回溯：撤销
                used[num] = False       # 恢复

    dfs()

# permutations(3) → 6 行：
# 1 2 3 / 1 3 2 / 2 1 3 / 2 3 1 / 3 1 2 / 3 2 1
```

### ⚠️ DFS 易错点

- 递归前务必写 `sys.setrecursionlimit(1000000)`
- **回溯时要恢复现场**（pop、置 False），否则影响兄弟分支
- 别忘 visited 标记，否则死循环

---

## BFS 广度优先搜索 ⭐⭐

> 💡 **一层一层向外扩**。用队列实现。**找最短距离/最少步数必用它。**

```
遍历顺序（一层一层扫）：

      A        第 0 层
     / \
    B   C      第 1 层
   / \
  D   E        第 2 层

BFS 顺序：A → B → C → D → E
```

### 📝 代码模板

```python
from collections import deque

def bfs(start) -> int:
    """返回从起点到目标的最短步数，找不到返回 -1"""
    q = deque([start])
    visited = {start}
    steps = 0

    while q:
        level_size = len(q)               # 当前层有多少个节点
        for _ in range(level_size):
            cur = q.popleft()             # 从队首取（FIFO）
            if is_target(cur):
                return steps
            for nxt in neighbors(cur):
                if nxt not in visited:
                    visited.add(nxt)
                    q.append(nxt)         # 加到队尾
        steps += 1                        # 一层处理完

    return -1                             # 队列空了还没找到
```

### 🎯 DFS vs BFS 选择

| | DFS | BFS |
|:---|:---|:---|
| 结构 | 栈（递归/手动） | 队列（deque） |
| 问什么 | "有没有解"、"所有解" | "最短距离"、"最少步数" |
| 空间 | O(深度) | O(宽度) 可能很大 |

---

## 回溯 ⭐⭐

> 💡 **DFS + 恢复现场**。试一种 → 不行就撤销 → 试下一种。求"所有方案"专用。

### 📝 N 皇后模板

```python
def n_queens(n: int):
    cols = set()                         # 已被占的列
    diag1 = set()                        # 正对角线：行+列 相等
    diag2 = set()                        # 反对角线：行-列 相等
    board = [['.'] * n for _ in range(n)]
    answers = []

    def backtrack(row):
        if row == n:
            answers.append([''.join(r) for r in board])
            return

        for col in range(n):
            if col in cols or (row+col) in diag1 or (row-col) in diag2:
                continue                 # 冲突，跳过

            # 做选择
            board[row][col] = 'Q'
            cols.add(col); diag1.add(row+col); diag2.add(row-col)

            backtrack(row + 1)           # 深入

            # 恢复现场 — 回溯的核心！
            board[row][col] = '.'
            cols.remove(col); diag1.remove(row+col); diag2.remove(row-col)

    backtrack(0)
    return answers
```

### ⚠️ 回溯易错点

**一定要恢复现场！** 选了什么就要撤销什么。不恢复的话，另一个分支会看到被污染的状态。

---

## 🔗 相关

- [[../10-语法手册/11-常用标准库#deque]] — BFS 用的 deque
- [[../10-语法手册/09-函数]] — 递归函数写法
- [[08-数据结构#并查集]] — 并查集做连通性
