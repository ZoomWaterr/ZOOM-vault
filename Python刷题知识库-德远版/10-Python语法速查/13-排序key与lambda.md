      ---
      aliases:
- key
- lambda
- 自定义排序
- 按第二项排序
- 按长度排序
      tags:
- python/语法
- 算法/排序
- 状态/常忘
      level: 基础
      status: 常用
      entry: 语法
      source: 综合整理
      ---

# 排序 key 与 lambda

## 先看这个

        `key` 告诉排序：别直接比元素本身，要拿元素的某个特征来比。

        ## 什么时候用

        按长度、按分数、按出现次数、按元组某一项排序时。

        ## 怎么写

        ```python
        words.sort(key=len)
pairs.sort(key=lambda x: x[1])
pairs.sort(key=lambda x: (x[0], -x[1]))
best = max(count, key=count.get)
        ```

        ## 我容易摔在哪里

        `lambda x: x[1]` 的意思是：对每个元素 x，取它下标 1 的部分当排序依据。

## 常见例子

```python
students = [("A", 90), ("B", 80), ("C", 95)]
students.sort(key=lambda x: x[1], reverse=True)
```

## 多关键字排序

```python
# 先按分数降序，再按名字升序
students.sort(key=lambda x: (-x[1], x[0]))
```

## 个人提醒

你看到 `max(count, key=count.get)` 觉得像天书很正常。把它翻译成人话：在所有 key 里，找 value 最大的那个 key。
## 参考

- Python 官方文档：<https://docs.python.org/3/tutorial/>
- Python 标准库：<https://docs.python.org/3/library/>
- Obsidian Help：<https://help.obsidian.md/>
- OI Wiki：<https://oi-wiki.org/>
- CP-Algorithms：<https://cp-algorithms.com/>
