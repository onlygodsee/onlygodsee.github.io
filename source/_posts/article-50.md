---
title: 红黑树（上）
date: 2020-09-07 13:47:37
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

为什么工程中都用红黑树这种二叉树？

<!-- more -->

我们学习数据结构和算法，要学习它的由来、特性、适用的场景以及它能解决的问题。对于红黑树，也不例外。你如果能搞懂这几个问题，其实就已经足够了。 由来及解决的问题：普通二叉树会在某些特殊情况下，有二叉树退化为链表，导致时间复杂度从O(logN)退化到O(n)。所以，产生了平衡二叉树。 适用的场景：需要频繁的进行动态的插入、删除以及查找。 红黑树的特性：红黑树不是严格的平衡二叉树（任意一节点的左子树与右子树高度差不超过1），红黑树有可能最短路径和最长路径相差一倍。但是红黑树的优势在于：红黑树的增删查的性能没有相比于平衡二叉树低太多。而维护插入、删除等平衡操作时，付出的成本要低于AVL等严格的平衡二叉树。所以在工程中更加常用。 红黑树的缺点：实现起来比较复杂，可能更多的会采用跳表来进行实现。

AVL 树是一种高度平衡的二叉树，所以查找的效率非常高，但是，有利就有弊，AVL 树为了维持这种高度的平衡，就要付出更多的代价。每次插入、删除都要做调整，就比较复杂、耗时。所以，对于有频繁的插入、删除操作的数据集合，使用 AVL 树的代价就有点高了。红黑树只是做到了近似平衡，并不是严格的平衡，所以在维护平衡的成本上，要比 AVL 树要低。所以，红黑树的插入、删除、查找各种操作性能都比较稳定。对于工程应用来说，要面对各种异常情况，为了支撑这种工业级的应用，我们更倾向于这种性能稳定的平衡二叉查找树。



### 什么是“平衡二叉查找树”？

#### 定义

二叉树中任意一个节点的左右子树的高度相差不能大于 1。从这个定义来看，上一节我们讲的完全二叉树、满二叉树其实都是平衡二叉树，但是非完全二叉树也有可能是平衡二叉树。

![](dd9f5a4525f5029a8339c89ad1c8159b.jpg)

#### 意义

***发明平衡二叉查找树这类数据结构的初衷是，解决普通二叉查找树在频繁的插入、删除等动态更新的情况下，出现时间复杂度退化的问题。***

所以，平衡二叉查找树中“平衡”的意思，其实就是让整棵树左右看起来比较“对称”、比较“平衡”，不要出现左子树很高、右子树很矮的情况。这样就能让整棵树的高度相对来说低一些，相应的插入、删除、查找等操作的效率高一些。

### 如何定义一棵“红黑树”？

顾名思义，红黑树中的节点，一类被标记为黑色，一类被标记为红色。除此之外，一棵红黑树还需要满足这样几个要求：

- 根节点是黑色的；
- 每个叶子节点都是黑色的空节点（NIL），也就是说，叶子节点不存储数据；
- 任何相邻的节点都不能同时为红色，也就是说，红色节点是被黑色节点隔开的；
- 每个节点，从该节点到达其可达叶子节点的所有路径，都包含相同数目的黑色节点；

### 思考

#### 动态数据结构支持动态的数据插入、删除、查找操作，除了红黑树，我们前面还学习过哪些呢？能对比一下各自的优势、劣势，以及应用场景吗？

动态数据结构有链表，栈，队列，哈希表等等。链表适合遍历的场景，插入和删除操作方便，栈和队列可以算一种特殊的链表，分别适用先进后出和先进先出的场景。哈希表适合插入和删除比较少（尽量少的扩容和缩容），查找比较多的时候。红黑树对数据要求有序，对数据增删查都有一定要求的时候。