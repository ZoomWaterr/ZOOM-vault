---
created: 2026-06-08
type: deep-note
source: Algorithm.pdf pages 18-28
tags: [Python, AVL, 平衡二叉树, 旋转]
---

# AVL 旋转与平衡

原 PDF 对应页：Algorithm.pdf 第 18-28 页。原文系统讲 AVL 的平衡因子、LL/RR/LR/RL 旋转和插入后及时平衡。Python 版不要求你马上手写完整工业级 AVL，但必须看懂旋转到底在保护什么。

![AVL 旋转原图](../assets/algorithm/algorithm_p020_01.png)

## 一句话抓本质

AVL 是始终保持左右子树高度差不超过 1 的 BST；旋转不是打乱顺序，而是在保留中序有序的前提下降低高度。

## 节点定义

```python
from dataclasses import dataclass

@dataclass
class AVLNode:
    key: int
    left: "AVLNode | None" = None
    right: "AVLNode | None" = None
    height: int = 1

def height(node: AVLNode | None) -> int:
    return node.height if node else 0

def update(node: AVLNode) -> None:
    node.height = 1 + max(height(node.left), height(node.right))

def balance_factor(node: AVLNode | None) -> int:
    if node is None:
        return 0
    return height(node.left) - height(node.right)
```

## 右旋：LL 型

```text
      y              x
     / \            / \
    x   T3   ->    T1  y
   / \                / \
  T1 T2              T2 T3
```

```python
from dataclasses import dataclass

@dataclass
class AVLNode:
    key: int
    left: "AVLNode | None" = None
    right: "AVLNode | None" = None
    height: int = 1

def height(node: AVLNode | None) -> int:
    return node.height if node else 0

def update(node: AVLNode) -> None:
    node.height = 1 + max(height(node.left), height(node.right))

def rotate_right(y: AVLNode) -> AVLNode:
    x = y.left
    assert x is not None
    t2 = x.right
    x.right = y
    y.left = t2
    update(y)
    update(x)
    return x
```

## 左旋：RR 型

```python
from dataclasses import dataclass

@dataclass
class AVLNode:
    key: int
    left: "AVLNode | None" = None
    right: "AVLNode | None" = None
    height: int = 1

def height(node: AVLNode | None) -> int:
    return node.height if node else 0

def update(node: AVLNode) -> None:
    node.height = 1 + max(height(node.left), height(node.right))

def rotate_left(x: AVLNode) -> AVLNode:
    y = x.right
    assert y is not None
    t2 = y.left
    y.left = x
    x.right = t2
    update(x)
    update(y)
    return y
```

## 插入并平衡

```python
from dataclasses import dataclass

@dataclass
class AVLNode:
    key: int
    left: "AVLNode | None" = None
    right: "AVLNode | None" = None
    height: int = 1

def height(node: AVLNode | None) -> int:
    return node.height if node else 0

def update(node: AVLNode) -> None:
    node.height = 1 + max(height(node.left), height(node.right))

def balance_factor(node: AVLNode | None) -> int:
    if node is None:
        return 0
    return height(node.left) - height(node.right)

def rotate_right(y: AVLNode) -> AVLNode:
    x = y.left
    assert x is not None
    t2 = x.right
    x.right = y
    y.left = t2
    update(y)
    update(x)
    return x

def rotate_left(x: AVLNode) -> AVLNode:
    y = x.right
    assert y is not None
    t2 = y.left
    y.left = x
    x.right = t2
    update(x)
    update(y)
    return y

def insert(root: AVLNode | None, key: int) -> AVLNode:
    if root is None:
        return AVLNode(key)
    if key < root.key:
        root.left = insert(root.left, key)
    elif key > root.key:
        root.right = insert(root.right, key)
    else:
        return root

    update(root)
    bf = balance_factor(root)

    if bf > 1 and root.left is not None and key < root.left.key:
        return rotate_right(root)
    if bf < -1 and root.right is not None and key > root.right.key:
        return rotate_left(root)
    if bf > 1 and root.left is not None and key > root.left.key:
        root.left = rotate_left(root.left)
        return rotate_right(root)
    if bf < -1 and root.right is not None and key < root.right.key:
        root.right = rotate_right(root.right)
        return rotate_left(root)
    return root
```

## 四种失衡怎么认

| 类型 | 新节点插入位置 | 修复 |
|---|---|---|
| LL | 左孩子的左侧 | 右旋 |
| RR | 右孩子的右侧 | 左旋 |
| LR | 左孩子的右侧 | 左旋左孩子，再右旋 |
| RL | 右孩子的左侧 | 右旋右孩子，再左旋 |

## 为什么旋转不破坏 BST

右旋前的中序：T1, x, T2, y, T3。右旋后的中序仍然是：T1, x, T2, y, T3。顺序没变，只是高度变了。

## 常见错法

错误 1：旋转后忘记更新高度。先更新下面的旧根，再更新新的根。

错误 2：把 LR 当 LL，只做一次右旋。LR 和 RL 都需要双旋。

错误 3：只会背图，不会看插入路径。判断类型要看新节点落在失衡节点的哪条孙子路径上。

## 题目信号

Python 比赛中很少要求手写 AVL，但会要求理解：

- 为什么普通 BST 会退化。
- 为什么平衡树能保证 O(log n)。
- 有序集合需要动态插入删除时，Python 标准库没有内置平衡树，要考虑 heap、bisect、sortedcontainers 或换模型。

## 验收练习

1. 插入 30, 20, 10，画出 LL 并右旋。
2. 插入 30, 10, 20，画出 LR 双旋。
3. 证明右旋前后中序序列不变。
