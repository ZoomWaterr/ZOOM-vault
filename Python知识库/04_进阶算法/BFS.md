---
created: 2026-05-31
type: note
tags: [进阶算法, BFS, 搜索]
---

# BFS

BFS 是广度优先搜索：一层一层扩散。

## 模板

```python
from collections import deque

q = deque([start])
visited = {start}
step = 0

while q:
    for _ in range(len(q)):
        x = q.popleft()
        if x == target:
            print(step)
            break
        for y in next_states(x):
            if y not in visited:
                visited.add(y)
                q.append(y)
    step += 1
```

## 题目信号

- 最短步数。
- 一层层扩散。
- 迷宫。
- 从起点到终点最少操作次数。

## 为什么 BFS 能求最短步数

队列保证先处理距离起点近的状态。第一次到达终点时，就是最短步数。

## 易错点

- 入队时就标记 visited，不要出队后才标。
- `step` 增加的位置要和“层”对应。
