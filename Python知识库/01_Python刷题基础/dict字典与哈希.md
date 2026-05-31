---
created: 2026-05-31
type: note
tags: [Python基础, 字典, 哈希]
---

# dict字典与哈希

字典就是“谁 -> 对应什么”的映射。

## 一句话理解

当你需要通过一个东西快速找到另一个东西，就想字典。

## 统计次数

```python
count = {}
for x in nums:
    count[x] = count.get(x, 0) + 1
```

`count.get(x, 0)` 的意思是：如果 `x` 出现过，就拿它原来的次数；如果没出现过，就当作 0。

## 判断出现过没有

```python
seen = {}
for x in nums:
    if x in seen:
        print("出现过")
    seen[x] = True
```

判断存在更常用 [[set集合]]。

## 什么时候用字典

- 统计出现次数。
- 建立编号到名字、字符到次数、数到下标的关系。
- 找出现最多/最少。
- 快速查某个东西对应的值。

## 易错点

- `d[x]` 在 `x` 不存在时会报错。
- 不确定 key 是否存在时，用 `get`。
- 字典遍历 `for k, v in d.items()` 最常用。
