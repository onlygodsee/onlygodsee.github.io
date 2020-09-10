---
title: 二分查找（下）
date: 2020-09-02 14:46:00
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

二分查找的一些例子

<!-- more -->

### 查找第一个值等于给定值的元素(有序有重复数据)

```python
# 第一种
def binary_search(arrays, val):
    low = 0
    high = len(arrays) - 1

    while low <= high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] >= val:
            high = mid - 1
        else:
            low = mid + 1

    if (low < len(arrays)) and arrays[low] == val:
        return low

    return -1
```

```python
# 第二种
def binary_search(arrays, val):
    low = 0
    high = len(arrays) - 1

    while low < high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] > val:
            high = mid - 1
        elif arrays[mid] < val:
            low = mid + 1
        else:
            if mid == 0 or arrays[mid-1] != val:
                return mid
            high = mid - 1

    return -1
```

### 查找最后一个值等于给定值的元素

```python
# 第一种
def binary_search(arrays, val):
    length = len(arrays)
    low = 0
    high = length - 1

    while low <= high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] <= val:
            low = mid + 1
        else:
            high = mid - 1

    if high < length and arrays[high] == val:
        return high

    return -1
```

```python
# 第二种
def binary_search(arrays, val):
    length = len(arrays)
    low = 0
    high = length - 1

    while low <= high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] > val:
            high = mid - 1
        elif arrays[mid] < val:
            low = mid + 1
        else:
            if (mid == (length-1)) or arrays[mid + 1] != val:
                return mid
            low = mid + 1

    return -1
```

### 查找第一个大于等于给定值的元素

```python
def binary_search(arrays, val):
    length = len(arrays)
    low = 0
    high = length - 1

    while low <= high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] >= val:
            if mid == 0 or arrays[mid-1] < val:
                return mid
            high = mid - 1
        else:
            low = mid + 1

    return -1
```

### 查找最后一个小于等于给定值的元素

```python
def binary_search(arrays, val):
    length = len(arrays)
    low = 0
    high = length - 1

    while low <= high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] <= val:
            if mid == (length-1) or arrays[mid+1] > val:
                return mid
            low = mid + 1
        else:
            high = mid - 1

    return -1
```

### 思考

#### 如果有序数组是一个循环有序数组，比如 4，5，6，1，2，3。针对这种情况，如何实现一个求“值等于给定值”的二分查找算法呢？

```python
def binary_search(arrays, val):
    length = len(arrays)
    low = 0
    high = length - 1

    while low <= high:
        mid = low + ((high - low) >> 1)

        if arrays[mid] == val:
            return mid

        if arrays[mid] > val:
            # 左半序列
            if arrays[low] > val:
                low = mid + 1
            else:
                high = mid - 1
        else:
            if arrays[high] < val:
                high = mid -1
            else:
                low = mid + 1

    return -1
```

