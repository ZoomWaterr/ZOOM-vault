---
created: 2026-05-31
type: note
tags: [Python基础, 循环]
---

# while循环

`while` 适合“循环次数不确定，但结束条件明确”的题。

## 基本写法

```python
while 条件:
    循环体
```

## 例子：一直除到变成 1

```python
n = int(input())
cnt = 0

while n > 1:
    n //= 2
    cnt += 1

print(cnt)
```

## 常见场景

- 模拟过程直到某个条件发生。
- 指针不断移动。
- 辗转相除法。
- BFS 队列不为空。

## 易错点

- 循环里必须让条件有机会变成假，否则死循环。
- `break` 是直接跳出循环。
- `continue` 是跳过本轮剩下代码，进入下一轮。
