---
created: 2026-05-31
updated: 2026-06-03
type: deep-note
tags: [Python基础, 循环, 枚举, 变量追踪]
---

# for循环

## 一句话抓本质

`for` 的本质不是“重复执行代码”，而是：

> 从一批东西里，按顺序一个一个拿出来处理。

所以你写 `for` 时，脑子里先不要想语法，先问一句：

```text
我现在要一个一个拿谁？
每次拿出来的东西叫什么？
每拿一次要干什么？
```

## 什么时候想到 for

看到这些信号，优先想 `for`：

| 题目信号 | 脑子里的翻译 |
|---|---|
| 给你 n 个数 | 一个一个读 / 一个一个处理 |
| 对每个元素 | 遍历列表 |
| 统计有多少个满足条件 | 遍历 + `cnt` |
| 求和 / 最大 / 最小 | 遍历 + `sum/ans` |
| 输出图形 / 矩阵 | 双层 `for` |
| 对每个位置看它左边 | `for i in range(n)` + `for j in range(i)` |

## `range` 先看成一排格子

```text
range(5)      -> 0 1 2 3 4
range(2, 6)   -> 2 3 4 5
range(1, 8, 2)-> 1 3 5 7
```

核心口诀：

```text
range(结束)              从 0 到 结束-1
range(开始, 结束)        从 开始 到 结束-1
range(开始, 结束, 步长)  每次跳 步长
```

最容易错的是：

```python
for i in range(n):
    print(i)
```

这里 `i` 最后到 `n - 1`，不会到 `n`。

## 三种最常见写法

### 1. 只需要次数

```python
for _ in range(3):
    print("hello")
```

`_` 表示：我只是需要循环 3 次，不关心当前是第几次。

### 2. 需要下标

```python
nums = [5, 2, 9]

for i in range(len(nums)):
    print(i, nums[i])
```

输出过程：

```text
i=0, nums[i]=5
i=1, nums[i]=2
i=2, nums[i]=9
```

### 3. 只需要值

```python
nums = [5, 2, 9]

for x in nums:
    print(x)
```

这里 `x` 是当前拿出来的值，不是下标。

如果同时要下标和值，用 `enumerate`：

```python
nums = [5, 2, 9]

for i, x in enumerate(nums):
    print(i, x)
```

## 双层 for：不要看成两个循环，要看成两个角色

你迷双层 `for`，通常不是语法问题，而是没看清两个变量分别在干什么。

典型写法：

```python
for i in range(n):
    for j in range(i):
        print(i, j)
```

翻译成人话：

```text
i：当前正在处理的位置
j：当前 i 左边的某个位置
range(i)：只枚举 0 到 i-1，也就是 i 左边
```

所以这段代码本质是：

> 对每个当前位置 `i`，把它左边所有位置 `j` 都看一遍。

## 真实例题：P1428 小鱼比可爱

题意翻译：

> 对每条鱼，数一数它左边有多少条鱼比它小。

样例数据：

```text
鱼的可爱值：4 3 0 5 1 2
下标：      0 1 2 3 4 5
```

先手算：

```text
i=0，左边没有鱼                       -> 0
i=1，左边是 [4]，比 3 小的没有          -> 0
i=2，左边是 [4,3]，比 0 小的没有        -> 0
i=3，左边是 [4,3,0]，比 5 小的有 3 条   -> 3
i=4，左边是 [4,3,0,5]，比 1 小的有 1 条 -> 1
i=5，左边是 [4,3,0,5,1]，比 2 小的有 2 条 -> 2

答案：0 0 0 3 1 2
```

代码：

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

## 逐轮变量追踪

只看 `i = 3` 这一轮：

```text
nums = [4, 3, 0, 5, 1, 2]

i = 3
nums[i] = 5
range(i) = range(3) = 0, 1, 2
```

| 内层 j | nums[j] | nums[i] | 判断 `nums[j] < nums[i]` | cnt |
|---:|---:|---:|---|---:|
| 0 | 4 | 5 | True | 1 |
| 1 | 3 | 5 | True | 2 |
| 2 | 0 | 5 | True | 3 |

内层结束后，`cnt = 3`，所以 `ans.append(3)`。

关键点：

```text
cnt 是“当前这条鱼”的计数。
所以 cnt 必须放在外层循环里面重置。
ans.append(cnt) 必须等内层全部看完以后再做。
```

## 两个真实错法

### 错法 1：`cnt` 放在外面

```python
cnt = 0
ans = []

for i in range(n):
    for j in range(i):
        if nums[j] < nums[i]:
            cnt += 1
    ans.append(cnt)
```

错在哪里：

```text
cnt 变成了“总共累计多少次”，不是“当前鱼左边有多少条更小”。
每条鱼都应该重新数，所以 cnt 要放进外层循环。
```

### 错法 2：`ans.append(cnt)` 放进内层

```python
for i in range(n):
    cnt = 0
    for j in range(i):
        if nums[j] < nums[i]:
            cnt += 1
        ans.append(cnt)
```

错在哪里：

```text
你每比较一次就 append 一次。
但题目要的是“每条鱼一个答案”，不是“每次比较一个答案”。
```

## 双层 for 的复杂度

这段代码不是完整的 `n * n`，但仍然是 `O(n^2)`：

```python
for i in range(n):
    for j in range(i):
        print(i, j)
```

它的次数是：

```text
i=0 -> 0 次
i=1 -> 1 次
i=2 -> 2 次
i=3 -> 3 次
中间继续增加
i=n-1 -> n-1 次

总次数 = 0 + 1 + 2 + 中间项 + (n-1)
       = n(n-1)/2
       = O(n^2)
```

所以：

```text
n <= 5000 可能还能试
n = 100000 基本不行
```

如果数据很大，就要想：能不能用 [[哈希计数]]、[[前缀和]]、[[排序]]、[[二分查找]]、[[树状数组]] 优化。

## 矩阵双层 for

矩阵遍历时，两个变量角色变了：

```python
for i in range(n):
    for j in range(m):
        print(grid[i][j])
```

```text
i：第几行
j：第几列
grid[i][j]：第 i 行第 j 列的元素
```

小样例：

```text
grid =
1 2 3
4 5 6

n = 2, m = 3
```

追踪：

| i | j | grid[i][j] |
|---:|---:|---:|
| 0 | 0 | 1 |
| 0 | 1 | 2 |
| 0 | 2 | 3 |
| 1 | 0 | 4 |
| 1 | 1 | 5 |
| 1 | 2 | 6 |

## 写 for 前的 5 个问题

每次写循环前，先填这张小表：

```text
1. 我要遍历谁？
2. 当前拿出来的变量叫什么？
3. 它是下标还是值？
4. 每一轮要更新什么？
5. 答案是一轮一个，还是全部结束后一个？
```

对 P1428：

```text
1. 遍历每条鱼的位置。
2. 外层 i，内层 j。
3. i 和 j 都是下标。
4. 内层更新 cnt。
5. 每条鱼一个答案，所以外层结束时 append。
```

## 练习阶梯

1. 手写 `range(6)`、`range(1, 6)`、`range(1, 6, 2)` 会产生什么。
2. 给 `nums = [2, 5, 1, 4]`，手算 P1428 的答案。
3. 把 P1428 的 `i=3` 这一轮变量追踪表自己写一遍。
4. 写代码：统计每个数左边有多少个数比它大。
5. 写代码：给一个矩阵，输出每一行的和。

## 验收问题

不看笔记，能回答这些才算过：

- `range(i)` 为什么只看左边？
- `cnt` 为什么要放在外层循环里面？
- `ans.append(cnt)` 为什么不能放在内层？
- `for i in range(n): for j in range(i):` 为什么还是 `O(n^2)`？
- 矩阵里 `i` 和 `j` 分别表示什么？

## 相关链接

- [[变量角色]]
- [[复杂度分析]]
- [[枚举]]
- [[暴力到优化]]
- [[P1428 小鱼比可爱复盘样例]]
