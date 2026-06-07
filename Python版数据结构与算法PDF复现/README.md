---
created: 2026-06-08
updated: 2026-06-08
type: pdf-remake-index
tags: [Python, 数据结构, 算法, PDF复现]
---

# Python版数据结构与算法 PDF 复现

本文件夹按原 PDF 讲义重新排版，用 Markdown + Python 复现两份资料：

| 原 PDF | 页数 | Markdown 复现方式 |
|---|---:|---|
| [[00_原始PDF/Algorithm.pdf|Algorithm.pdf]] | 52 | 保持“查找和排序”的原章节顺序，补 Python 代码与公式 |
| [[00_原始PDF/DataStructure.pdf|DataStructure.pdf]] | 99 | 保持“数据结构”的原章节顺序，补 Python 代码与公式 |

> 来源说明：PDF 元数据作者字段为 By: LI LIANGJI (Wechat: llj907015000)。本资料包保留原 PDF、图片和章节脉络；正文把 C 语言代码复现为 Python 代码。

## 版式约定

每篇笔记尽量贴近原 PDF 讲义形态：

```text
原 PDF 页码
原 PDF 小节目录
定义 / 性质 / 公式
原 PDF 图示
Python 复现
复杂度与注意点
```

数学内容统一用 LaTeX：

$$
ASL=\sum_{i=1}^{n}P_iC_i
$$

代码统一用 Python，但变量命名尽量保留原 PDF 里的含义，例如 low/high/mid、front/rear、next/prefix、dist/path。

## DataStructure.pdf 复现目录

1. [[01_线性表_list与链表节点]]：顺序表、单链表、循环链表、双向链表
2. [[02_栈队列与递归调用栈]]：顺序栈、链栈、循环队列、链队、递归
3. [[03_字符串匹配_BF与KMP]]：BF、KMP、next 数组、环状病毒串
4. [[04_树二叉树与森林]]：二叉树性质、遍历、层序、树和森林转换
5. [[05_哈夫曼树与编码]]：WPL、最优二叉树、哈夫曼编码
6. [[06_图的表示_DFS_BFS]]：邻接矩阵、邻接表、DFS、BFS
7. [[07_最小生成树_Prim_Kruskal]]：Prim、Kruskal、并查集
8. [[08_最短路径_拓扑排序_关键路径]]：Dijkstra、Floyd、拓扑排序、关键路径

## Algorithm.pdf 复现目录

1. [[01_查找表_ASL与顺序查找]]：查找表、关键字、ASL、顺序查找
2. [[02_二分查找与分块查找]]：折半查找、判断树、分块查找
3. [[03_BST二叉搜索树]]：BST 查找、插入、创建、删除
4. [[04_AVL旋转与平衡]]：平衡因子、LL/RR/LR/RL 旋转
5. [[05_哈希表与冲突处理]]：哈希函数、冲突、除留余数法
6. [[06_排序总览_稳定性与复杂度]]：排序分类、稳定性、时间空间比较
7. [[07_插入希尔选择冒泡排序]]：直接插入、折半插入、希尔、冒泡、选择
8. [[08_快排归并堆排序基数排序]]：快排、归并、堆、基数排序

## 总验收

完成后用 [[99_总复习与验收题]] 检查：公式是否能写出、图示是否能解释、Python 模板是否能默写。
