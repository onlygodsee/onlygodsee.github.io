---
title: 栈
date: 2020-08-31 14:50:33
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

## 栈

### 结构特点

后进先出，先进后出。



### 如何实现一个“栈”

用数组实现的栈，我们叫作***顺序栈****，用链表实现的栈，我们叫作**链式栈**。*

#### 顺序栈

```python
class MyStack:
    def __init__(self):
        self.my_stack = []
        self.length = 10
        self.count = 0

    def push(self, value):
        """
        入栈
        :return:
        """
        # 栈空间已满
        if self.count == self.length:
            return False

        print(f"before push, now length is : {self.count}")
        self.my_stack.insert(0, value)
        print(f"push {value}")
        self.count += 1
        print(f"after push, now length is : {self.count}")

    def pop(self):
        """
        出栈
        :return:
        """
        # 栈为空
        if self.count == 0:
            return False

        print(f"before pop, now length is : {self.count}")
        value = self.my_stack.pop(0)
        print(f"pop {value}")
        self.count -= 1
        print(f"after pop, now length is : {self.count}")
```

时间复杂度 O(1), 空间复杂度 O(1)

***对于动态扩容顺序栈，其入栈最好时间复杂度为O(1)， 最坏时间复杂度为O(n)，平均时间复杂度O(1)***



### 栈在函数调用中的应用

比较经典的一个应用场景就是***函数调用栈***

我们知道，操作系统给每个线程分配了一块独立的内存空间，这块内存被组织成“栈”这种结构, 用来存储函数调用时的临时变量。每进入一个函数，就会将临时变量作为一个栈帧入栈，当被调用函数执行完成，返回之后，将这个函数对应的栈帧出栈。



### 栈在表达式求值中的应用

编译器就是通过两个栈来实现的。其中一个保存操作数的栈，另一个是保存运算符的栈。

我们从左向右遍历表达式，当遇到数字，我们就直接压入操作数栈；当遇到运算符，就与运算符栈的栈顶元素进行比较。如果比运算符栈顶元素的优先级高，就将当前运算符压入栈；如果比运算符栈顶元素的优先级低或者相同，从运算符栈中取栈顶运算符，从操作数栈的栈顶取 2 个操作数，然后进行计算，再把计算完的结果压入操作数栈，继续比较。



### 栈在括号匹配中的应用

我们用栈来保存未匹配的左括号，从左到右依次扫描字符串。当扫描到左括号时，则将其压入栈中；当扫描到右括号时，从栈顶取出一个左括号。如果能够匹配，比如“(”跟“)”匹配，“[”跟“]”匹配，“{”跟“}”匹配，则继续扫描剩下的字符串。如果扫描的过程中，遇到不能配对的右括号，或者栈中没有数据，则说明为非法格式。

```python
class LegalBracket():
    def __init__(self):
        self.bracket_map = {
            ")": "(",
            "]": "[",
            "}": "{"
        }

    def is_legal_bracket(self, value):
        self.my_stack = []

        for val in value:
            if not self.my_stack or val in "([{":
                self.my_stack.insert(0, val)
                continue

            if self.bracket_map[val] == self.my_stack[0]:
                self.my_stack.pop(0)

        return not bool(self.my_stack)
```



### 思考

#### 为什么函数调用要用“栈”来保存临时变量呢？用其他数据结构不行吗？

其实，我们不一定非要用栈来保存临时变量，只不过如果这个函数调用符合后进先出的特性，用栈这种数据结构来实现，是最顺理成章的选择。

从调用函数进入被调用函数，对于数据来说，变化的是什么呢？是作用域。所以根本上，只要能保证每进入一个新的函数，都是一个新的作用域就可以。而要实现这个，用栈就非常方便。在进入被调用函数的时候，分配一段栈空间给这个函数的变量，在函数结束的时候，将栈顶复位，正好回到调用函数的作用域内。

