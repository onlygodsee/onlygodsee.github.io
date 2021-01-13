---
title: 红黑树（下）
date: 2020-09-07 16:44:14
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

掌握这些技巧，你也可以实现一个红黑树

<!-- more -->

### 实现红黑树的基本思想

主体流程：遇到什么样的节点排布，我们就对应怎么去调整。

两个重要操作：***左旋、右旋***

![](0e37e597737012593a93105ebbf4591e.jpg)

### 插入操作的平衡调整

#### 插入

***红黑树规定，插入的节点必须是红色的。而且，二叉查找树中新插入的节点都是放在叶子节点上。***所以，关于插入操作的平衡调整，有这样两种特殊情况，但是也都非常好处理。

- 如果插入节点的父节点是黑色的，那我们什么都不用做，它仍然满足红黑树的定义。
- 如果插入的节点是根节点，那我们直接改变它的颜色，把它变成黑色就可以了。

除此之外，其他情况都会违背红黑树的定义，于是我们就需要进行调整，调整的过程包含两种基础的操作：***左右旋转和改变颜色***。

红黑树的平衡调整过程是一个迭代的过程。我们把正在处理的节点叫做***关注节点***。关注节点会随着不停地迭代处理，而不断发生变化。最开始的关注节点就是新插入的节点。

一般有三种情况：

##### CASE 1：

如果关注节点是 a，它的叔叔节点 d 是红色，我们就依次执行下面的操作：

- 将关注节点 a 的父节点 b、叔叔节点 d 的颜色都设置成黑色；
- 将关注节点 a 的祖父节点 c 的颜色设置成红色；
- 关注节点变成 a 的祖父节点 c；
- 跳到 CASE 2 或者 CASE 3。

![](603cf91f54b5db21bd02c6c5678ecf40.jpg)

##### CASE 2：

如果关注节点是 a，它的叔叔节点 d 是黑色，关注节点 a 是其父节点 b 的右子节点，我们就依次执行下面的操作：

- 关注节点变成节点 a 的父节点 b；
- 围绕新的关注节点b 左旋；
- 跳到 CASE 3。

![](4480a314f9d83c343b8adbb28b6782ad.jpg)

##### CASE 3：

如果关注节点是 a，它的叔叔节点 d 是黑色，关注节点 a 是其父节点 b 的左子节点，我们就依次执行下面的操作：

- 围绕关注节点 a 的祖父节点 c 右旋；
- 将关注节点 a 的父节点 b、兄弟节点 c 的颜色互换。
- 调整结束。

![](04650d9470b1e67899f5b8b7b8e33212.jpg)

### 删除操作的平衡调整

删除操作的平衡调整分为两步：

第一步是***针对删除节点初步调整***。初步调整只是保证整棵红黑树在一个节点删除之后，仍然满足最后一条定义的要求，也就是说，每个节点，从该节点到达其可达叶子节点的所有路径，都包含相同数目的黑色节点；

第二步是***针对关注节点进行二次调整***，让它满足红黑树的第三条定义，即不存在相邻的两个红色节点。

#### 针对删除节点初步调整

红黑树的定义中“只包含红色节点和黑色节点”，经过初步调整之后，为了保证满足红黑树定义的最后一条要求，***有些节点会被标记成两种颜色，“红 - 黑”或者“黑 - 黑”。如果一个节点被标记为了“黑 - 黑”***，那在计算黑色节点个数的时候，要算成两个黑色节点。

##### CASE 1：

如果要删除的节点是 a，它只有一个子节点 b，那我们就依次进行下面的操作：

- 删除节点 a，并且把节点 b 替换到节点 a 的位置，这一部分操作跟普通的二叉查找树的删除操作一样；
- 节点 a 只能是黑色，节点 b 也只能是红色，其他情况均不符合红黑树的定义。这种情况下，我们把节点 b 改为黑色；
- 调整结束，不需要进行二次调整。

![](a6c4c347b7cbdf57662bab399ed36cc3.jpg)

##### CASE 2：

***如果要删除的节点 a 有两个非空子节点，并且它的后继节点就是节点 a 的右子节点 c。***我们就依次进行下面的操作：

- 如果节点 a 的后继节点就是右子节点 c，那右子节点 c 肯定没有左子树。我们把节点 a 删除，并且将节点 c 替换到节点 a 的位置。这一部分操作跟普通的二叉查找树的删除操作无异；
- 然后把节点 c 的颜色设置为跟节点 a 相同的颜色；
- 如果节点 c 是黑色，为了不违反红黑树的最后一条定义，我们给节点 c 的右子节点 d 多加一个黑色，这个时候节点 d 就成了“红 - 黑”或者“黑 - 黑”；
- 这个时候，关注节点变成了节点 d，第二步的调整操作就会针对关注节点来做。

![](48e3bd2cdd66cb635f8a4df8fb8fd64e.jpg)

##### CASE 3：

***如果要删除的是节点 a，它有两个非空子节点，并且节点 a 的后继节点不是右子节点***，我们就依次进行下面的操作：

- 找到后继节点 d，并将它删除，删除后继节点 d 的过程参照 CASE 1；
- 将节点 a 替换成后继节点 d；
- 把节点 d 的颜色设置为跟节点 a 相同的颜色；
- 如果节点 d 是黑色，为了不违反红黑树的最后一条定义，我们给节点 d 的右子节点 c 多加一个黑色，这个时候节点 c 就成了“红 - 黑”或者“黑 - 黑”；
- 这个时候，关注节点变成了节点 c，第二步的调整操作就会针对关注节点来做。

![](b93c1fa4de16aee5482424ddf49f3c29.jpg)

#### 针对关注节点进行二次调整

二次调整是为了让红黑树中不存在相邻的红色节点。

##### CASE 1：

***如果关注节点是 a，它的兄弟节点 c 是红色的，***我们就依次进行下面的操作：

- 围绕关注节点 a 的父节点 b 左旋；
- 关注节点 a 的父节点 b 和祖父节点 c 交换颜色；
- 关注节点不变；
- 继续从四种情况中选择适合的规则来调整。

![](ac76d78c064a2486e2a5b4c4903acb91.jpg)

##### CASE 2：

***如果关注节点是 a，它的兄弟节点 c 是黑色的，并且节点 c 的左右子节点 d、e 都是黑色的，***我们就依次进行下面的操作：

- 将关注节点 a 的兄弟节点 c 的颜色变成红色；
- 从关注节点 a 中去掉一个黑色，这个时候节点 a 就是单纯的红色或者黑色；
- 给关注节点 a 的父节点 b 添加一个黑色，这个时候节点 b 就变成了“红 - 黑”或者“黑 - 黑”；
- 关注节点从 a 变成其父节点 b；
- 继续从四种情况中选择符合的规则来调整。

![](eca118d673c607eb2b103f3476fb24ec.jpg)

##### CASE 3：

***如果关注节点是 a，它的兄弟节点 c 是黑色，c 的左子节点 d 是红色，c 的右子节点 e 是黑色，***我们就依次进行下面的操作：

- 围绕关注节点 a 的兄弟节点 c 右旋；
- 节点 c 和节点 d 交换颜色；
- 关注节点不变；
- 跳转到 CASE 4，继续调整。

![](44075213100edd70315e1492422c92af.jpg)

##### CASE 4：

***如果关注节点 a 的兄弟节点 c 是黑色的，并且 c 的右子节点是红色的，***我们就依次进行下面的操作：

- 围绕关注节点 a 的父节点 b 左旋；
- 将关注节点 a 的兄弟节点 c 的颜色，跟关注节点 a 的父节点 b 设置成相同的颜色；
- 将关注节点 a 的父节点 b 的颜色设置为黑色；
- 从关注节点 a 中去掉一个黑色，节点 a 就变成了单纯的红色或者黑色；
- 将关注节点 a 的叔叔节点 e 设置为黑色；
- 调整结束。

![](5f73f61bf77a7f2bb75f168cf432ec44.jpg)

### 内容总结

第一点，***把红黑树的平衡调整的过程比作魔方复原，不要过于深究这个算法的正确性。***

第二点，***找准关注节点，不要搞丢、搞错关注节点。***

第三点，***插入操作的平衡调整比较简单，但是删除操作就比较复杂。***
