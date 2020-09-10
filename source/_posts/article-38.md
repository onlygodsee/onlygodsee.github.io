---
title: 排序（下）- 归并和快排
date: 2020-09-01 14:23:48
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

归并排序和快速排序

<!-- more -->

### 归并排序（Merge Sort）

#### 原理

把数组从中间分成前后两部分，然后对前后两部分分别排序，再将排好序的两部分合并在一起，这样整个数组就都有序了。利用分治法的思想，和递归的编程技巧。

#### 代码

```python
# 方法一
def merge(arrays, l, mid, r):
    output = []
    i = l
    j = mid + 1

    while i <= mid and j <= r:
        if arrays[i] <= arrays[j]:
            output.append(arrays[i])
            i += 1
        else:
            output.append(arrays[j])
            j += 1

    for k in arrays[i:mid+1] + arrays[j:r+1]:
        output.append(k)

    for idx in range(len(output)):
        arrays[l+idx] = output[idx]

def mergesort(arrays, l, r):

    if l < r:
        mid = l + ((r - l) >> 1)
        mergesort(arrays, l, mid)
        mergesort(arrays, mid + 1, r)
        merge(arrays, l, mid, r)
```

```python
# 方法二
def merge(arrays, arr1, arr2):
    i = j = 0
    length = len(arrays)

    while i+j<length:
        if j == len(arr2) or (i < len(arr1) and arr1[i] <= arr2[j]):
            arrays[i+j] = arr1[i]
            i += 1
        else:
            arrays[i+j] = arr2[j]
            j += 1

def merge_sort(arrays):
    length = len(arrays)

    if length < 2:
        return arrays

    mid = length // 2
    arr1 = arrays[:mid]
    arr2 = arrays[mid:]

    merge_sort(arr1)
    merge_sort(arr2)

    merge(arrays, arr1, arr2)
```

#### 性能分析

###### 构造合理的merge函数，可以使得归并排序成为稳定排序

###### 时间复杂度分析

设b，c是a的子问题，由此可得到一下公式：

```
T(a) = T(b) + T(c) + K
# 其中 K 等于将两个子问题 b、c 的结果合并成问题 a 的结果所消耗的时间
```

可以得到一个重要的结论：***不仅递归求解的问题可以写成递推公式，递归代码的时间复杂度也可以写成递推公式***。

最终递推公式为：***T(n) = 2^kT(n/2^k)+kn***

所以，***归并排序的时间复杂度为nlogn***

###### 空间复杂度O(n)



### 快速排序

#### 原理

如果要排序数组中下标从 p 到 r 之间的一组数据，我们选择 p 到 r 之间的任意一个数据作为 pivot（分区点）。我们遍历 p 到 r 之间的数据，将小于 pivot 的放到左边，将大于 pivot 的放到右边，将 pivot 放到中间。经过这一步骤之后，数组 p 到 r 之间的数据就被分成了三个部分，前面 p 到 q-1 之间都是小于 pivot 的，中间是 pivot，后面的 q+1 到 r 之间是大于 pivot 的。

根据分治、递归的处理思想，我们可以用递归排序下标从 p 到 q-1 之间的数据和下标从 q+1 到 r 之间的数据，直到区间缩小为 1，就说明所有的数据都有序了。

#### 代码

```python
# 快速排序
def partition(arrays, l, r):
    val = arrays[r]
    idx = l - 1
    for i in range(l, r):
        if arrays[i] <= val:
            idx += 1
            arrays[idx], arrays[i] = arrays[i], arrays[idx]

    arrays[idx+1], arrays[r] = arrays[r], arrays[idx+1]

    return idx+1

def quick_sort(arrays, l, r):
    # 分区区间小于1时退出
    if l<r:
        idx = partition(arrays, l, r)
        quick_sort(arrays, l, idx-1)
        quick_sort(arrays, idx+1, r)
```

#### 与归并排序区别

归并排序的处理过程是***由下到上***的，先处理子问题，然后再合并。而快排正好相反，它的处理过程是***由上到下***的，先分区，然后再处理子问题。

#### 性能分析

最坏时间复杂度O(n^2)，平均复杂度O(nlogn)

### 内容小结

归并排序算法是一种在任何情况下时间复杂度都比较稳定的排序算法，这也使它存在致命的缺点，即归并排序不是原地排序算法，空间复杂度比较高，是 O(n)。

快速排序算法虽然最坏情况下的时间复杂度是 O(n2)，但是平均情况下时间复杂度都是 O(nlogn)。不仅如此，快速排序算法时间复杂度退化到 O(n2) 的概率非常小，我们可以通过合理地选择 pivot 来避免这种情况。

