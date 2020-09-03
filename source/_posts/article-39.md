---
title: 线性排序
date: 2020-09-01 17:22:01
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

## 线性排序

桶排序、计数排序、基数排序。因为这些排序算法的时间复杂度是线性的，所以我们把这类排序算法叫作线性排序。

### 桶排序（Bucket sort）

#### 代码

```python
def bucket_sort(arrays):
    length = len(arrays)
    bucket_num = 5
    min_num = arrays[0]
    max_num = arrays[0]
    output = []

    for i in range(1, length):
        if arrays[i] < min_num:
            min_num = arrays[i]
        elif arrays[i] > max_num:
            max_num = arrays[i]

    buckets_size = (max_num - min_num + 1) // bucket_num
    bucket_lists = [[] for i in range(buckets_size)]

    for num in arrays:
        idx = (num-min_num) // buckets_size
        bucket_lists[idx].append(num)

    for bucket in bucket_lists:
      	# 使用快排对桶内数组进行排序
        quick_sort(bucket, 0, len(bucket)-1)

    for i in bucket_lists:
        output.extend(i)

    print(output)
```

#### 局限性

要排序的数据需要很容易就能划分成 m 个桶，并且，桶与桶之间有着天然的大小顺序。在极端情况下，如果数据都被划分到一个桶里，那就退化为 O(nlogn) 的排序算法了。

#### 适用场景

桶排序比较***适合用在外部排序中***。所谓的外部排序就是数据存储在外部磁盘中，数据量比较大，内存有限，无法将数据全部加载到内存中。

### 计数排序（Counting sort）

#### 原理

一种特殊的桶排序。当要排序的 n 个数据，所处的范围并不大的时候，比如最大值是 k，我们就可以把数据划分成 k 个桶。

#### “计数”的含义

利用另一个数组C记录小于等于当前桶值的数量，然后倒序遍历待排序数组A，然后根据数组C中对应的值确定当前值的位置，最后数组C对应数量减一，直至为零，往复循环。

#### 局限性

计数排序只能用在数据范围不大的场景中，如果数据范围 k 比要排序的数据 n 大很多，就不适合用计数排序了。而且，计数排序只能给非负整数排序，如果要排序的数据是其他类型的，要将其在不改变相对大小的情况下，转化为非负整数。

### 基数排序（Radix sort）

以手机号排序为例（长度一致），需从末尾开始遍历然后递进排序。

#### 局限性

基数排序对要排序的数据是有要求的，需要可以分割出独立的“位”来比较，而且位之间有递进的关系，如果 a 数据的高位比 b 数据大，那剩下的低位就不用比较了。除此之外，每一位的数据范围不能太大，要可以用线性排序算法来排序，否则，基数排序的时间复杂度就无法做到 O(n) 了

### 内容小结

桶排序和计数排序的排序思想是非常相似的，都是针对范围不大的数据，将数据划分成不同的桶来实现排序。基数排序要求数据可以划分成高低位，位之间有递进关系。比较两个数，我们只需要比较高位，高位相同的再比较低位。而且每一位的数据范围不能太大，因为基数排序算法需要借助桶排序或者计数排序来完成每一个位的排序工作。

