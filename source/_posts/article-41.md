---
title: 二分查找（上）
date: 2020-09-02 09:37:00
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

认识二分查找

<!-- more -->

### 原理

二分查找针对的是一个有序的数据集合，查找思想有点类似分治思想。每次都通过跟区间的中间元素对比，将待查找的区间缩小为之前的一半，直到找到要查找的元素，或者区间被缩小为 0。

### 性能分析

时间复杂度O(logn)

### 代码实现（有序数组无重复数据）

#### 递归

```python
def binary_search2(arrays, l, r, val):
    if l <= r:
        mid = l + ((r - l) >> 1)

        if arrays[mid] == val:
            return mid

        if arrays[mid] > val:
            return binary_search2(arrays, l, mid-1, val)
        else:
            return binary_search2(arrays, mid+1, r, val)
```



#### 非递归

```python
def binary_search(arrays, val):
    l = 0
    r = len(arrays)-1

    while l <= r:
        mid = l + ((r - l) >> 1)
        if arrays[mid] == val:
            return mid
        if arrays[mid] < val:
            l = mid + 1
        else:
            r = mid
```



### 局限性

#### 二分查找依赖的是顺序表结构，简单点说就是数组

主要原因是二分查找算法需要按照下标随机访问元素

#### 二分查找针对的是有序数据

二分查找只能用在插入、删除操作不频繁，一次排序多次查找的场景中。针对动态变化的数据集合，二分查找将不再适用。

#### 数据量太小不适合二分查找

有一个例外，如果数据之间的比较操作非常耗时，不管数据量大小，我都推荐使用二分查找。

#### 数据量太大也不适合二分查找

二分查找基于数组，而数组需要在连续的内存的空间。



### 思考

#### 实现“求一个数的平方根”？要求精确到小数点后 6 位。

```python
def my_sqrt(num, s):
    if num > 1:
        low = 1
        hight = num
    else:
        low = num
        hight = 1

    line = 1/pow(10, s)
    while low < hight:
        mid = (low + hight) / 2

        val = mid * mid
        if abs(val - num) <= line:
            return round(mid, s)

        if val > num:
            hight = mid
        else:
            low = mid
```

#### 如果数据使用链表存储，二分查找的时间复杂就会变得很高，那查找的时间复杂度究竟是多少呢？



假设链表长度为n，二分查找每次都要找到中间点(计算中忽略奇偶数差异):
第一次查找中间点，需要移动指针n/2次；
第二次，需要移动指针n/4次；
第三次需要移动指针n/8次；
......
以此类推，一直到1次为值

总共指针移动次数(查找次数) = n/2 + n/4 + n/8 + ...+ 1，这显然是个等比数列，根据等比数列求和公式：Sum = n - 1.

最后算法时间复杂度是：O(n-1)，忽略常数，记为O(n)，时间复杂度和顺序查找时间复杂度相同

但是稍微思考下，在二分查找的时候，由于要进行多余的运算，严格来说，会比顺序查找时间慢