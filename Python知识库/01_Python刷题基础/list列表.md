---
created: 2026-05-31
type: note
tags: [Python基础, 列表]
---

# list列表

`list` 是刷题最常用的数据结构：一排有顺序的格子。

## 初始化

```python
a = []
a = [0] * n
nums = list(map(int, input().split()))
```

## 常用操作

```python
a.append(x)
a.pop()
a.sort()
b = sorted(a)
len(a)
a[i]
a[l:r]
```

## 切片

```python
s[l:r]      # 左闭右开，不包含 r
s[::-1]     # 反转
s[:-2]      # 从开头到倒数第二个之前
```

## 什么时候用列表

- 存一组输入。
- 存答案。
- 做前缀和、差分、DP 表。
- 排序后处理。

## 易错点

- `a.sort()` 会改原列表。
- `sorted(a)` 返回新列表。
- `[[]] * n` 会让每一行引用同一个列表，二维数组不要这样建。

正确二维数组：

```python
grid = [[0] * m for _ in range(n)]
```
