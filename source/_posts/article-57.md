---
title: 字符串匹配基础（中）
date: 2020-09-10 15:44:50
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

BM（Boyer-Moore）算法

<!-- more -->

### BM 算法的核心思想

![](cf362f9e59c01aaf40a34d2f10e1ef15.jpg)

在这个例子里，主串中的 c，在模式串中是不存在的，所以，模式串向后滑动的时候，只要 c 与模式串有重合，肯定无法匹配。所以，我们可以一次性把模式串往后多滑动几位，把模式串移动到 c 的后面。

### BM 算法原理分析

**坏字符规则** 和 **好后缀规则**

#### 坏字符规则

按照模式串下标从大到小的顺序，如下图

![](540809418354024206d9989cb6cdd89e.jpg)

##### 偏移规律

当发生不匹配的时候，我们把坏字符对应的模式串中的字符下标记作 si。如果坏字符在模式串中存在，我们把这个坏字符在模式串中的下标记作 xi。如果不存在，我们把 xi 记作 -1。那模式串往后移动的位数就等于 si-xi。（注意，我这里说的下标，都是字符在模式串的下标）。

![](8f520fb9d9cec0f6ea641d4181eb432e.jpg)

##### 特别注意

如果坏字符在模式串里多处出现，那我们在计算 xi 的时候，***选择最靠后的那个***，因为这样不会让模式串滑动过多，导致本来可能匹配的情况被滑动略过。



***时间复杂度 O(n/m)***

#### 好后缀规则

![](d78990dbcb794d1aa2cf4a3c646ae58a.jpg)

我们把已经匹配的 bc 叫作好后缀，记作{u}。我们拿它在模式串中查找，如果找到了另一个跟{u}相匹配的子串{u*}，那我们就将模式串滑动到子串{u*}与主串中{u}对齐的位置。

如果在模式串中找不到另一个等于{u}的子串，我们就直接将模式串，滑动到主串中{u}的后面，因为之前的任何一次往后滑动，都没有匹配主串中{u}的情况。

但是，出现了一个严重的问题， ***过度滑动***

![](9b3fa3d1cd9c0d0f914a9b1f518ad070.jpg)

**如果好后缀在模式串中不存在可匹配的子串，那在我们一步一步往后滑动模式串的过程中，只要主串中的{u}与模式串有重合，那肯定就无法完全匹配。但是当模式串滑动到前缀与主串中{u}的后缀有部分重合的时候，并且重合的部分相等的时候，就有可能会存在完全匹配的情况。**

***所以，针对这种情况，我们不仅要看好后缀在模式串中，是否有另一个匹配的子串，我们还要考察好后缀的后缀子串，是否存在跟模式串的前缀子串匹配的。***

#### 如何选择

我们可以分别计算好后缀和坏字符往后滑动的位数，然后取两个数中最大的，作为模式串往后滑动的位数。这种处理方法还可以避免我们前面提到的，根据坏字符规则，计算得到的往后滑动的位数，有可能是负数的情况。




