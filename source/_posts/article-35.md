---
title: 队列
date: 2020-08-31 16:42:04
tags:
- 基础篇
- 数据结构与算法
categories:
- 数据结构与算法
---

初识队列

<!-- more -->

### 结构特点

先进者先出

队列跟栈一样，也是一种操作受限的线性表数据结构

### 顺序队列和链式队列

用数组实现的队列叫作***顺序队列****，用链表实现的队列叫作**链式队列**。*

#### 顺序队列

```python
class MyQueues:
    def __init__(self):
        self.length = 3
        self.my_queue = [0] * self.length
        self.head = 0
        self.tail = 0

    def push(self, val):
        """
        入队
        :param val:
        :return:
        """
        if self.tail == self.length:
            if self.head == 0:
                self.my_queue = self.my_queue[self.head:] + self.my_queue[:self.head]
                self.head = 0
                self.tail -= self.head
            else:
                return False

        print(f"before push, now length is {self.tail}")
        self.my_queue[self.tail] = val
        print(f"push {val}")
        self.tail += 1
        print(f"after push, now length is {self.tail}")

    def pop(self):
        """
        出队
        :return:
        """
        if self.head == self.tail:
            return False

        print(f"before pop, now head is {self.head}")
        val, self.my_queue[self.head] = self.my_queue[self.head], 0
        print(f"pop {val}")
        self.head += 1
        print(f"after pop, now head is {self.head}")
```

#### 链式队列

#### 循环队列

避免数据搬移，最关键的是，确定好队空和队满的判定条件。

队列为空：head == tail

队列已满：(tail + 1) % length == head

```python
class MyCircleQueue:
    def __init__(self):
        self.length = 3
        self.my_circle_queue = [0] * self.length
        self.head = 0
        self.tail = 0

    def push(self, value):
        """
        入队
        :param value:
        :return:
        """
        if self.head == self.tail:
            return False

        print(f"before push, queue is {self.my_circle_queue}")
        self.my_circle_queue[self.tail] = value
        print(f"push {value}")
        self.tail = (self.tail + 1) % self.length
        print(f"after push, queue is {self.my_circle_queue}")

    def pop(self):
        """
        出队
        :return:
        """
        if (self.tail + 1) % self.length == self.head:
            return False

        print(f"before pop, queue is {self.my_circle_queue}")
        value, self.my_circle_queue[self.head] = self.my_circle_queue[self.head], 0
        print(f"pop {value}")
        self.head = (self.head + 1) % self.length
        print(f"after pop, queue is {self.my_circle_queue}")
```



#### 阻塞队列和并发队列

***阻塞队列*** 其实就是在队列基础上增加了阻塞操作。简单来说，就是在队列为空的时候，从队头取数据会被阻塞。因为此时还没有数据可取，直到队列中有了数据才能返回；如果队列已经满了，那么插入数据的操作就会被阻塞，直到队列中有空闲位置后再插入数据，然后再返回。

可以使用队列实现一个 ***生产者->消费者*** 模型。

线程安全的队列我们叫作***并发队列***。最简单直接的实现方式是直接在 enqueue()、dequeue() 方法上加锁，但是锁粒度大并发度会比较低，同一时刻仅允许一个存或者取操作。实际上，***基于数组的循环队列，利用 CAS 原子操作，可以实现非常高效的并发队列***。这也是循环队列比链式队列应用更加广泛的原因。



### 线程池没有空闲线程时，新的任务请求线程资源时，线程池该如何处理？各种处理策略又是如何实现的呢？

我们一般有两种处理策略。第一种是非阻塞的处理方式，直接拒绝任务请求；另一种是阻塞的处理方式，将请求排队，等到有空闲线程时，取出排队的请求继续处理。那如何存储排队的请求呢？

我们希望公平地处理每个排队的请求，先进者先服务，所以队列这种数据结构很适合来存储排队请求。我们前面说过，队列有基于链表和基于数组这两种实现方式。这两种实现方式对于排队请求又有什么区别呢？

基于链表的实现方式，可以实现一个支持无限排队的无界队列（unbounded queue），但是可能会导致过多的请求排队等待，请求处理的响应时间过长。所以，针对响应时间比较敏感的系统，基于链表实现的无限排队的线程池是不合适的。

而基于数组实现的有界队列（bounded queue），队列的大小有限，所以线程池中排队的请求超过队列大小时，接下来的请求就会被拒绝，这种方式对响应时间敏感的系统来说，就相对更加合理。

不过，设置一个合理的队列大小，也是非常有讲究的。队列太大导致等待的请求太多，队列太小会导致无法充分利用系统资源、发挥最大性能。除了前面讲到队列应用在线程池请求排队的场景之外，队列可以应用在任何有限资源池中，用于排队请求，比如数据库连接池等。实际上，对于大部分资源有限的场景，当没有空闲资源时，基本上都可以通过“队列”这种数据结构来实现请求排队。



### 思考

#### 除了线程池这种池结构会用到队列排队请求，你还知道有哪些类似的池结构或者场景中会用到队列的排队请求呢？

分布式应用中的消息队列，也是一种队列结构



#### 今天讲到并发队列，关于如何实现无锁并发队列，网上有非常多的讨论。对这个问题，你怎么看呢？

考虑使用CAS实现无锁队列，则在入队前，获取tail位置，入队时比较tail是否发生变化，如果否，则允许入队，反之，本次入队失败。出队则是获取head位置，进行cas。