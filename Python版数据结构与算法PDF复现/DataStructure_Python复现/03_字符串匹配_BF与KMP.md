---
created: 2026-06-08
updated: 2026-06-08
type: pdf-like-note
source: DataStructure.pdf P32-P41
tags: [Python, 字符串, BF, KMP]
---

# 3. 字符串匹配：BF 算法与 KMP 算法

**原 PDF 页码：** DataStructure.pdf P32-P41

## 3.1 原 PDF 小节

- 字符串存储
- BF 算法 Brute Force
- KMP 算法
- 求 next 数组
- 字符串匹配玩具：环状病毒序列

## 3.2 字符串匹配问题

给定主串 S 和模式串 T，要求找到 T 第一次出现在 S 中的位置。

$$
S=s_0s_1\cdots s_{n-1},\quad T=t_0t_1\cdots t_{m-1}
$$

## 3.3 BF 算法

BF 算法从主串每个位置开始尝试匹配。

$$
T[0:m] \stackrel{?}{=} S[i:i+m]
$$

Python 复现：

```python
def bf_match(text: str, pattern: str) -> int:
    n, m = len(text), len(pattern)
    if m == 0:
        return 0
    for i in range(n - m + 1):
        j = 0
        while j < m and text[i + j] == pattern[j]:
            j += 1
        if j == m:
            return i
    return -1
```

最坏情况复杂度：

$$
T(n,m)=O(nm)
$$

## 3.4 KMP 的 next / prefix 数组

原 PDF 用 next[j] 表示模式串回退位置。Python 常用 0 下标 prefix 数组：

$$
prefix[i]=\max\{k\mid T[0:k]=T[i-k+1:i+1]\}
$$

也就是：T[0:k] 是当前前缀的最长相等真前缀和真后缀。

![KMP next 图示](../assets/data_structure/data_structure_p039_01.png)

Python 复现：

```python
def build_prefix(pattern: str) -> list[int]:
    prefix = [0] * len(pattern)
    j = 0
    for i in range(1, len(pattern)):
        while j > 0 and pattern[i] != pattern[j]:
            j = prefix[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        prefix[i] = j
    return prefix
```

## 3.5 KMP 匹配

KMP 的核心是不让主串指针 i 回退。失败时，只让模式串指针 j 回退：

$$
j \leftarrow prefix[j-1]
$$

```python
def kmp_match(text: str, pattern: str) -> int:
    if pattern == "":
        return 0
    prefix = build_prefix(pattern)
    j = 0
    for i, ch in enumerate(text):
        while j > 0 and ch != pattern[j]:
            j = prefix[j - 1]
        if ch == pattern[j]:
            j += 1
        if j == len(pattern):
            return i - len(pattern) + 1
    return -1

def build_prefix(pattern: str) -> list[int]:
    prefix = [0] * len(pattern)
    j = 0
    for i in range(1, len(pattern)):
        while j > 0 and pattern[i] != pattern[j]:
            j = prefix[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        prefix[i] = j
    return prefix
```

复杂度：

$$
T(n,m)=O(n+m)
$$

## 3.6 手算 prefix 示例

模式串 ababaca：

| i | 字符 | 最长相等前后缀 | prefix[i] |
|---:|---|---|---:|
| 0 | a | 空 | 0 |
| 1 | b | 空 | 0 |
| 2 | a | a | 1 |
| 3 | b | ab | 2 |
| 4 | a | aba | 3 |
| 5 | c | 空 | 0 |
| 6 | a | a | 1 |

## 3.7 环状病毒序列

原 PDF 的病毒串是环状序列。若病毒串为 xyz，则 xyz、yzx、zxy 都是同一个环。处理方法：

$$
Virus'=Virus+Virus
$$

若样本串长度相等，并且出现在 Virus' 中，则匹配。

```python
def circular_match(virus: str, sample: str) -> bool:
    if len(virus) != len(sample):
        return False
    return kmp_match(virus + virus, sample) != -1
```

## 3.8 C 与 Python 下标差异

| 原 PDF | Python 版 |
|---|---|
| 串从 1 开始存字符 | 字符串从下标 0 开始 |
| next[1] = 0 | prefix[0] = 0 |
| i、j 同时比较 | i 遍历 text，j 指向 pattern |
