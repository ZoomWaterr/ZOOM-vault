      ---
      aliases:
- EOF
- 读到文件末尾
- sys.stdin
- 快读
- 大量输入
      tags:
- python/语法
- 刷题/输入输出
      level: 基础
      status: 常用
      entry: 语法
      source: 综合整理
      ---

# EOF 与快读

## 先看这个

        数据很多时，`input()` 可能慢；题目没说有几行时，可能要读到 EOF。

        ## 什么时候用

        输入规模超过 10^5，或题目说多组数据直到文件结束。

        ## 怎么写

        ```python
        import sys

for line in sys.stdin:
    nums = list(map(int, line.split()))

data = sys.stdin.buffer.read().split()
        ```

        ## 我容易摔在哪里

        快读会让代码没那么直观。基础题优先用 `input()`，数据大或 EOF 再上 `sys.stdin`。

## 读到 EOF

```python
import sys

for line in sys.stdin:
    if not line.strip():
        continue
    a, b = map(int, line.split())
    print(a + b)
```

## 快速读全部

```python
import sys

data = list(map(int, sys.stdin.buffer.read().split()))
```

## 相关

- [[01-输入输出]]
- [[../20-刷题方法与题型/06-边界与复杂度]]
## 参考

- Python 官方文档：<https://docs.python.org/3/tutorial/>
- Python 标准库：<https://docs.python.org/3/library/>
- Obsidian Help：<https://help.obsidian.md/>
- OI Wiki：<https://oi-wiki.org/>
- CP-Algorithms：<https://cp-algorithms.com/>
