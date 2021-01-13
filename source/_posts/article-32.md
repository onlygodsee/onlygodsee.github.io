---
title: 链表（上）
date: 2020-08-31 09:56:58
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

探索链表（上）

<!-- more -->

### 缓存

#### 介绍

缓存的作用:提高数据读取性能。 缓存设计的好不好，要看自己所设计的缓存的命中率高不高。

#### 缓存的应用场景

硬件中的缓存: CPU缓存，而cpu缓存又可以分为寄存器，一级缓存，二级缓存，三级缓存。 软件中的缓存: 数据库缓存，数据库本身产品就自带缓存。redis也可以作为数据库缓存. 浏览器缓存，就是我们常说的Cookie,本质上就是一个文件。

#### 缓存淘汰策略

先进先出策略 FIFO（First In，First Out）、最少使用策略 LFU（Least Frequently Used）、最近最少使用策略 LRU（Least Recently Used）

eg：

FIFO（先进先出调度器） 、Capacity Scheduler（容量调度器）和 Fair Sceduler（公平调度器）。

### 链表结构

*不需要一块连续的内存空间，它通过“指针”将一组**零散的内存块**串联起来使用。*

对内存要求方面: 数组对内存的要求更高。因为数组需要一块连续内存空间来存放数据。（可能出现的问题就是:内存总的剩余空间足够，但是申请容量较大的数组时申请失败） 链表对内存的要求较低，是因为链表不需要连续的内存空间，只要内存剩余空间足够，无论是否连续，用链表来申请空间一定会成功。 但是要注意:链表虽然方便。但是内存开销比数组大了将近一倍，假设存储100个整数，数组400个字节的存储空间足够了。但是如果用链表存储100个整数，链表得需要800个字节的存储空间，因为链表中的每个节点不止要存储数据，还要存储地址，内存的利用率就比数组低太多了。 由此还可以得出:如果内存容量本身就很小，要存储的数据也比较多。选择数组来存储数据更好，如果内存空间充足，那我们在存储数据的时候到底选择链表还是数组。这个就视具体的业务场景而定了。

三种常见链表结构：单链表， 双链表和循环链表

#### 单链表

习惯性地把第一个结点叫作**头结点**，把最后一个结点叫作**尾结点**。

**头结点**用来记录链表的基地址。有了它，我们就可以遍历得到整条链表。

**尾结点特殊的地方是**：指针不是指向下一个结点，而是指向一个空地址 NULL，表示这是链表上最后一个结点。



##### 查找、删除和插入

在链表中插入或者删除一个数据，我们并不需要为了保持内存的连续性而搬移结点，因为链表的存储空间本身就不是连续的。所以，在链表中插入和删除一个数据是非常快速的。

但是，因为链表中的数据并非连续存储的，所以无法像数组那样，根据首地址和下标，通过寻址公式就能直接计算出对应的内存地址，而是需要根据指针一个结点一个结点地依次遍历，直到找到相应的结点。时间复杂度O(n)



#### 循环链表

循环链表是一种特殊的单链表

跟单链表唯一的区别就在尾结点。我们知道，单链表的尾结点指针指向空地址，表示这就是最后的结点了。而循环链表的尾结点指针是指向链表的头结点。



#### 双向链表

它支持两个方向，每个结点不止有一个后继指针 next 指向后面的结点，还有一个前驱指针 prev 指向前面的结点。双向链表需要额外的两个空间来存储后继结点和前驱结点的地址。

双向链表可以支持 O(1) 时间复杂度的情况下找到前驱结点。



#### 空间换取时间

当内存空间充足的时候，如果我们更加追求代码的执行速度，我们就可以选择空间复杂度相对较高、但时间复杂度相对很低的算法或者数据结构。

对于执行较慢的程序，可以通过消耗更多的内存（空间换时间）来进行优化；而消耗过多内存的程序，可以通过消耗更多的时间（时间换空间）来降低内存的消耗。



### 链表 VS 数组

数组简单易用，在实现上使用的是连续的内存空间，可以借助 CPU 的缓存机制，预读数组中的数据，所以访问效率更高。而链表在内存中并不是连续存储，所以对 CPU 缓存不友好，没办法有效预读。***链表本身没有大小的限制，天然地支持动态扩容，这是它与数组最大的区别***。

eg：

CPU在从内存读取数据的时候，会先把读取到的数据加载到CPU的缓存中。而CPU每次从内存读取数据并不是只读取那个特定要访问的地址，而是读取一个数据块(这个大小我不太确定。。)并保存到CPU缓存中，然后下次访问内存数据的时候就会先从CPU缓存开始查找，如果找到就不需要再从内存中取。这样就实现了比内存访问速度更快的机制，也就是CPU缓存存在的意义:为了弥补内存访问速度过慢与CPU执行速度快之间的差异而引入。 对于数组来说，存储空间是连续的，所以在加载某个下标的时候可以把以后的几个下标元素也加载到CPU缓存这样执行速度会快于存储空间不连续的链表存储。



### 如何基于链表实现 LRU 缓存淘汰算法？

维护一个有序单链表，越靠近链表尾部的结点是越早之前访问的。

当有一个新的数据被访问时，我们从链表头开始顺序遍历链表。

1. 如果此数据之前已经被缓存在链表中了，我们遍历得到这个数据对应的结点，并将其从原来的位置删除，然后再插入到链表的头部。
2. 如果此数据没有在缓存链表中，又可以分为两种情况：
   - 如果此时缓存未满，则将此结点直接插入到链表的头部；
   - 如果此时缓存已满，则链表尾结点删除，将新的数据结点插入链表的头部。

思路优化：引入散列表（Hash table）来记录每个数据的位置，将缓存访问的时间复杂度降到 O(1)。



### 思考

#### 如何判断一个字符串是否是回文字符串的问题？

1 快慢指针定位中间节点
2 从中间节点对后半部分逆序
3 前后半部分比较，判断是否为回文
4 后半部分逆序复原

时间复杂度On, 空间复杂度O1