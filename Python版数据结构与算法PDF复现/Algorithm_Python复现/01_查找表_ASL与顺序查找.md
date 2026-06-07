---
created: 2026-06-08
updated: 2026-06-08
type: pdf-like-note
source: Algorithm.pdf P3-P5
tags: [Python, 查找, ASL, 顺序查找]
---

# 1. 查找表、ASL 与顺序查找

**原 PDF 页码：** Algorithm.pdf P3-P5

## 1.1 原 PDF 小节

- 查找表
- 关键字、主关键字、次关键字
- 查找表分类：线性表、树表、哈希表
- 静态查找表与动态查找表
- 平均查找长度 ASL
- 顺序查找、算法分析、哨兵优化

## 1.2 查找表定义

查找表是由同一类型数据元素构成的集合。查找时使用关键字 key 来定位记录。

| 名称 | 含义 |
|---|---|
| 主关键字 | 能唯一标识一个记录 |
| 次关键字 | 可以标识若干记录 |
| 静态查找表 | 只查找，不插入删除 |
| 动态查找表 | 查找、插入、删除都会发生 |

## 1.3 ASL 公式

平均查找长度：

$$
ASL=\sum_{i=1}^{n}P_iC_i
$$

其中：

| 符号 | 含义 |
|---|---|
| Pi | 查找第 i 个元素的概率 |
| Ci | 查找第 i 个元素需要的比较次数 |

若概率相等，顺序查找成功时：

$$
ASL_{success}=\frac{1}{n}\sum_{i=1}^{n}i=\frac{n+1}{2}
$$

失败时通常需要比较 n 次：

$$
ASL_{fail}=n
$$

## 1.4 顺序查找

```python
def sequential_search(data: list[int], key: int) -> int:
    for i, value in enumerate(data):
        if value == key:
            return i
    return -1
```

## 1.5 哨兵思想

原 PDF 用数组 0 号位置放 guard，目的是减少循环中的边界判断。Python 一般不用这种写法，但可以复现思想：

```python
def sequential_search_guard(data: list[int], key: int) -> int:
    arr = data[:] + [key]
    i = 0
    while arr[i] != key:
        i += 1
    return i if i < len(data) else -1
```

## 1.6 原 PDF 图示

![顺序查找分析](../assets/algorithm/algorithm_p005_01.png)

## 1.7 复杂度

| 查找方式 | 前提 | 成功平均比较 | 失败比较 | 时间复杂度 |
|---|---|---:|---:|---:|
| 顺序查找 | 无序或有序均可 | (n+1)/2 | n | O(n) |

## 1.8 提高查找效率

原 PDF 的两个方向：

1. 按查找概率高低存储，高概率元素放前面。
2. 查找概率无法确定时，根据访问频度动态调整。

Python 中更常见的改法：如果只关心按 key 查找，用 dict；如果数据有序，用二分。
