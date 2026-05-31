---
created: 2026-05-31
type: note
tags: [模板库, DP]
---

# DP入门模板

## 线性 DP

```python
dp = [0] * (n + 1)
dp[0] = 初始值

for i in range(1, n + 1):
    dp[i] = 从前面的状态转移而来

print(dp[n])
```

## 斐波那契

```python
dp = [0] * (n + 2)
dp[1] = dp[2] = 1
for i in range(3, n + 1):
    dp[i] = dp[i - 1] + dp[i - 2]
print(dp[n])
```

## 写 DP 前先填

```text
dp[i] 表示：
dp[i] 从哪里来：
初始值：
遍历顺序：
答案位置：
```

相关：[[动态规划]]
