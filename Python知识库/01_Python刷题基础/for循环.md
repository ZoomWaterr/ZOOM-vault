---
created: 2026-05-31
type: note
tags: [Python基础, 循环, 枚举]
---

# for循环

`for` 循环的本质是：按顺序拿出一批东西，一个一个处理。

## 遍历次数

```python
for i in range(n):
    print(i)
```

`range(n)` 产生 `0, 1, 2, ..., n-1`。

## 遍历列表

```python
nums = [3, 1, 4]
for x in nums:
    print(x)
```

`x` 是当前拿出来的元素，不是下标。

## 同时需要下标和值

```python
for i, x in enumerate(nums):
    print(i, x)
```

## 双层 for：先分清两个角色

不要把双层 `for` 看成“两个循环套一起”，要看成两个变量在扮演不同角色。

```python
for i in range(n):      # i：当前对象
    for j in range(i):  # j：当前对象左边的对象
        ...
```

这段代码枚举的是所有满足 `j < i` 的二元关系。

### 例子：小鱼比可爱

题意可以翻译成：

> 对每条鱼，数一数它左边有多少条鱼比它小。

```python
n = int(input())
nums = list(map(int, input().split()))

ans = []
for i in range(n):
    cnt = 0
    for j in range(i):
        if nums[j] < nums[i]:
            cnt += 1
    ans.append(cnt)

print(*ans)
```

变量解释：

- `i`：当前鱼的位置。
- `j`：当前鱼左边某条鱼的位置。
- `range(i)`：只看 `0` 到 `i-1`，不看自己，也不看右边。
- `cnt`：当前鱼左边比它小的数量。
- `ans`：每条鱼对应的答案。

## 双层 for 的常见模型

| 写法 | 含义 |
|---|---|
| `for i in range(n): for j in range(n):` | 所有有序二元组 |
| `for i in range(n): for j in range(i):` | 只看左边，避免重复 |
| `for i in range(n): for j in range(i + 1, n):` | 只看右边 |
| `for i in range(n): for j in range(m):` | 遍历矩阵 |

## 矩阵遍历

```python
for i in range(n):
    for j in range(m):
        print(grid[i][j])
```

- `i` 是第几行。
- `j` 是第几列。
- `grid[i][j]` 是第 `i` 行第 `j` 列。

## 易错点

- `range(i)` 不包含 `i`。
- 双层循环复杂度通常是 `O(n^2)`。
- 内层变量不要和外层变量重名。
- 先解释变量，再写循环。
