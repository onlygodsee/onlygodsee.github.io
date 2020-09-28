---
title: Python面试题收集
date: 2020-07-30 11:00:50
tags:
- 面试
categories:
- 面试
---

Python面试题收集

<!-- more -->

## 字符串

#### 将"hello world"转换为首字母大写"Hello World"

```python
"hello world".title()
```



#### 如何检测字符串中只含有数字?

```python
"".isdigit()
```



#### 将字符串"ilovechina"进行反转

```python
"ilovechina"[::-1]
```

```python
tmp = "ilovechina"
output = ""
for i in tmp:
  output = i + output
```



#### Python 中的字符串格式化方式你知道哪些？

%s, format, fstring



#### 有一个字符串开头和末尾都有空格，比如“ adabdw ”，要求写一个函数把这个字符串的前后空格都去掉。

```python
def clean_space(tmp):
  return tmp.strip()
```



#### 获取字符串”123456“最后的两个字符。

```python
"123456"[-2:]
```



#### 一个编码为 GBK 的字符串 S，要将其转成 UTF-8 编码的字符串，应如何操作？

```python
S.encode('gbk').decode('utf8', 'ignore')
```



#### （1）s="info：xiaoZhang 33 shandong"，用正则切分字符串输出['info', 'xiaoZhang', '33', 'shandong']。（2）a = "你好 中国 "，去除多余空格只留一个空格。

(1)

```python
import re
s = "info：xiaoZhang 33 shandong"
output = re.split(r"：| ", s)
print(output)
```

(2)

```python
a = "你好   中国 "
output = ' '.join(a.split())
print(output)
```



#### (1) 怎样将字符串转换为小写。 (2) 单引号、双引号、三引号的区别？

(1)

```python
"".lower()
```



### 列表

#### 已知 AList = [1,2,3,1,2]，对 AList 列表元素去重，写出具体过程。

```python
List(set(Alist))
```



#### 如何实现 "1,2,3" 变成 ["1","2","3"]

```python
"1,2,3".split(",")
```



#### 给定两个 list，A 和 B，找出相同元素和不同元素

```python
print(set(A) & set(B))
```

```python
print(set(A) ^ set(B))
```



#### [[1,2],[3,4],[5,6]] 一行代码展开该列表，得出 [1,2,3,4,5,6]

```python
tmp = [[1,2],[3,4],[5,6]]
print([j for i in tmp for j in i])
```



#### 合并列表 [1,5,7,9] 和 [2,2,6,8]

```python
a =  [1,5,7,9]
b =  [2,2,6,8]
a.extend(b)
```



#### 如何打乱一个列表的元素？

```python
import random

tmp = [1,2,3,4,5]
random.shuffle(tmp)
print(tmp)
```



### 字典

#### 字典操作中 del 和 pop 有什么区别

del 可以根据索引（元素所在位置）来删除的，没有返回值。 pop 可以根据索引弹出一个值，然后可以接收它的返回值。



#### 按照字典的内的年龄排序

```python
d1 = [
    {'name':'alice', 'age':38},
    {'name':'bob', 'age':18},
    {'name':'Carl', 'age':28},
]

print(sorted(d1, key=lambda d:d['age']))
```



#### 请合并下面两个字典 a = {"A"：1,"B"：2},b = {"C"：3,"D"：4}

```python
a = {"A":1,"B":2}
b = {"C":3,"D":4}
a.update(b)
print(a)
```

```python
print({**a, **b})
```



#### 如何使用生成式的方式生成一个字典，写一段功能代码。

```python
tmp = [['name', 'Boss'], ['age', 18]]

output = {k:v for k, v in tmp}
print(output)
```



#### 如何把元组 ("a","b") 和元组 (1,2)，变为字典 {"a"：1,"b"：2}

```python
a = ("a","b")
b = (1,2)
output = dict(zip(a, b))
print(output)
```



### 数据类型 - 综合

#### 下列字典对象键类型不正确的是？

```python
A：{1：0,2：0,3：0}
B：{"a"：0, "b"：0, "c"：0}
C： {(1,2)：0, (2,3)：0}
D： {[1,2]：0, [2,3]：0}
```

D, 字典的key必须是可哈希的，list是可变数据类型，不符合



#### 如何交换字典 {"A"：1,"B"：2}的键和值

```python
tmp = {"A":1,"B":2}

# 方法一
output = {v:k for k, v in tmp.items()}

# 方法二
output = dict(zip(tmp.values(), tmp.keys()))

print(output)
```



#### Python 里面如何实现 tuple 和 list 的转换？

Python 中的类型转换，一般通过类型强转即可完成 tuple 转 list 是 list() 方法 list 转 tuple 使用 tuple() 方法



#### 我们知道对于列表可以使用切片操作进行部分元素的选择，那么如何对生成器类型的对象实现相同的功能呢？

```python
from itertools import islice

gen = iter(range(10))

for i in islice(gen, 0, 4):
  	print(i)
```

这个题目考察了 Python 标准库的 itertools 模快的掌握情况，该模块提供了操作生成器的一些方法。 对于生成器类型我们使用 islice 方法来实现切片的功能。



#### 请将 [i for i in range(3)] 改成生成器

```python
(i for i in range(3))
```



#### a="hello" 和 b="你好" 编码成 bytes 类型

```python
a = b"hello"
b = bytes("你好", "utf-8")
c = "你好".encode("utf-8")
print(a, b, c)
```



#### 下面的代码输出结果是什么？

```python
a = (1,2,3,[4,5,6,7],8)
a[2] = 2
```

出现异常



#### 下面的代码输出的结果是什么?

```python
a = (1,2,3,[4,5,6,7],8)
a[3][0] = 2
```

```python
(1,2,3,[2,5,6,7],8)
```



#### 在读文件操作的时候会使用 read、readline 或者 readlines，简述它们各自的作用

read() 每次读取整个文件，它通常用于将文件内容放到一个字符串变量中。如果希望一行一行的输出那么就可以使用 readline()，该方法会把文件的内容加载到内存，所以对于对于大文件的读取操作来说非常的消耗内存资源，此时就可以通过 readlines 方法，将文件的句柄生成一个生产器，然后去读就可以了。



#### json 序列化时，可以处理的数据类型有哪些？如何定制支持 datetime 类型？

```python
import json
from datetime import date, datetime

class DateJson(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, datetime):
            return o.strftime('%Y-%m-%d %H:%M:%S')
        elif isinstance(o, date):
            return o.strftime("%Y-%m-%d")
        else:
            return json.JSONEncoder.default(o)

d1 = {
    'name': "Boss",
    'date': datetime.now()
}

print(json.dumps(d1, cls=DateJson))
```



#### json 序列化时，默认遇到中文会转换成 unicode，如果想要保留中文怎么办？

```python
import json

d = {'name': '张三','desc': '法外狂徒'}
print(json.dumps(d, ensure_ascii=False))
```



#### 有两个磁盘文件 A 和 B，各存放一行字母，要求把这两个文件中的信息合并(按字母顺序排列)，输出到一个新文件 C 中。

```python
with open('A.text', 'r') as f:
    a_text = f.readline()

with open('B.text', 'r') as f:
    b_text = f.readline()

output = a_text + b_text

c_list = sorted(output)

with open('c.text', 'a+') as f:
    f.write(''.join(c_list))
```



#### 如果当前的日期为 20190530，要求写一个函数输出 N 天后的日期，(比如 N 为 2，则输出 20190601)。

```python
from datetime import datetime
from dateutil.relativedelta import relativedelta

now_date = "20190530"
def date_add(now_date ,N):
    date = datetime.strptime(now_date, "%Y%m%d")
    new_date = date + relativedelta(days=2)
    return new_date.strftime('%Y%m%d')

print(date_add(now_date, 2))
```



#### 写一个函数，接收整数参数 n，返回一个函数，函数的功能是把函数的参数和 n 相乘并把结果返回。

```python
def func_out(n):
    def func_inner(val):
        return n * val

    return func_inner

run = func_out(8)
print(run(5))
```



#### 下面代码会存在什么问题，如何改进？

```python
def strappend(num)：
    str='first'
    for i in range(num)：
        str+=str(i)
    return str
```

首先不应该使用 Python 的内置类似 str 作为变量名这里我把它改为了 s,另外在Python,str 是个不可变对象，每次迭代都会生成新的存储空间，num 越大，创建的 str 对象就会越多，内存消耗越大。使用 yield 改成生成器即可, 还有一点就是命名规范的位置，函数名改为_分割比较好

```python
def str_append(num):
    s = 'first'
    for i in range(num):
        s += str(i)
        yield s
```



#### 一行代码输出 1-100 之间的所有偶数。

```python
# 方法一
print(list(filter(lambda num: num % 2 == 0, range(1, 101))))

# 方法二
print(list(range(2, 101, 2)))

# 方法三
print([i for i in range(1, 101) if i % 2 == 0])
```



#### with 语句的作用，写一段代码？

with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，比如文件使用后自动关闭、线程中锁的自动获取和释放等。

```python
#一般访问文件资源时我们会这样处理：

f = open(
    'c：\test.txt', 'r')
data = f.read()
f.close()
# 这样写没有错，但是容易犯两个毛病：
# 1. 如果在读写时出现异常而忘了异常处理。
# 2. 忘了关闭文件句柄

#以下的加强版本的写法：

f = open('c：\test.txt', 'r')
try：
    data = f.read()
finally：
    f.close()

#以上的写法就可以避免因读取文件时异常的发生而没有关闭问题的处理了。代码长了一些。
#但使用 with 有更优雅的写法：

with open(r'c：\test.txt', 'r') as f：
    data = f.read()
#with 的实现

class Test：
    def __enter__(self)：
        print('__enter__() is call!')
        return self

    def dosomething(self)：
        print('dosomethong!')

    def __exit__(self, exc_type, exc_value, traceback)：
        print('__exit__() is call!')
        print(f'type：{exc_type}')
        print(f'value：{exc_value}')
        print(f'trace：{traceback}')
        print('__exit()__ is call!')

with Test() as sample：
      pass

#当对象被实例化时，就会主动调用__enter__()方法，任务执行完成后就会调用__exit__()方法，
#另外，注意到，__exit__()方法是带有三个参数的(exc_type, exc_value, traceback),
#依据上面的官方说明：如果上下文运行时没有异常发生，那么三个参数都将置为 None, 
#这里三个参数由于没有发生异常，的确是置为了 None, 与预期一致.

# 修改后不出异常了
class Test：
    def __enter__(self)：
        print('__enter__() is call!')
        return self

    def dosomething(self)：
        x = 1/0
        print('dosomethong!')

    def __exit__(self, exc_type, exc_value, traceback)：
        print('__exit__() is call!')
        print(f'type：{exc_type}')
        print(f'value：{exc_value}')
        print(f'trace：{traceback}')
        print('__exit()__ is call!')
        return True


with Test() as sample：
```



#### Python 字典和 json 字符串相互转化方法

```python
# dict -> json
d = {
  'name': 'Bob',
  'age': 18
}
output = json.dumps(d)
print(output)

# json -> dict 
s = '{"name": "Bob", "age": 18}'
output = json.loads(s)
print(output)
```

注意：json.loads(jsonstr) 这里面的参数只能是 jsonstr 格式的字符串. 当我们使用 str 将字典 dic 转化为字符串以后，得到的结果为:"{'a': 123, 'b': '456', 'c': 'liming'}"。 如果直接使用 json.loads(str(dic)) 你会发现出现错误，原因就是，单引号的字符串不符合Json的标准格式所以再次使用了 replace("'", "\\"")。



#### 请写一个 Python 逻辑，计算一个文件中的大写字母数量

```python
with open('test.text', 'r') as f:
    content = f.read()

def pre_count(content):
    content = content.replace(' ', '')
    return content

def do_count(content):
    COUNT = 0
    content = pre_count(content)

    for i in content:
        if i.isupper():
            COUNT += 1

    return COUNT

print(do_count(content))
```

```python
with open('A.txt') as fs：
    count = 0
    for i in fs.read()：
        if i.isupper()：
            count += 1
print(count)
```



#### 请写一段 Python连接Mongo数据库，然后查询的代码。

```python
from pymongo import MongoClient

mongo = MongoClient(
    host='localhost',
    port=27017,
)

db = mongo.tutorial
collection = db.QuoteItem

result = collection.find()

for i in result:
    print(i)

mongo.close()
```

```python
import pymongo
db_configs = {
    'type': 'mongo',
    'host': '地址',
    'port': '端口',
    'user': 'spider_data',
    'passwd': '密码',
    'db_name': 'spider_data'
}


class Mongo():
    def __init__(self, db=db_configs["db_name"], username=db_configs["user"],
                 password=db_configs["passwd"]):
        self.client = pymongo.MongoClient(f'mongodb://{db_configs["host"]}:db_configs["port"]')
        self.username = username
        self.password = password
        if self.username and self.password:
            self.db1 = self.client[db].authenticate(self.username, self.password)
        self.db1 = self.client[db]

    def find_data(self):
        # 获取状态为0的数据
        data = self.db1.test.find({"status": 0})
        gen = (item for item in data)
        return gen

if __name__ == '__main__':
    m = Mongo()
    print(m.find_data())
```



#### 说一说Redis的基本类型

Redis 支持五种数据类型： string（字符串） 、 hash（哈希）、list（列表） 、 set（集合） 及 zset(sorted set： 有序集合)。



#### 请写一段 Python连接Redis数据库的代码。

```python
from redis import ConnectionPool, StrictRedis

pool = ConnectionPool.from_url("redis://127.0.0.1:6379/2", decode_components=True)
conn = StrictRedis(connection_pool=pool)
```



#### 请写一段 Python连接Mysql数据库的代码。

```python
import pymysql

conn = pymysql.connect(
    host="localhost",
    port=3306,
    user="root",
    password="251314wq",
    db="spider"
)
```



#### 了解Redis的事务么

简单理解，可以认为 redis 事务是一些列 redis 命令的集合，并且有如下两个特点： 1.事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。 2.事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。 一般来说，事务有四个性质称为ACID，分别是原子性，一致性，隔离性和持久性。 一个事务从开始到执行会经历以下三个阶段：

```python
import redis
import sys
def run():   
    try:
        conn=redis.StrictRedis('192.168.80.41')
       # Python中redis事务是通过pipeline的封装实现的
        pipe=conn.pipeline()
        pipe.sadd('s001','a')
        sys.exit()
        #在事务还没有提交前退出，所以事务不会被执行。
        pipe.sadd('s001','b')
        pipe.execute()
        pass
    except Exception as err:
        print(err)
        pass
if __name__=="__main__":
      run()
```



#### 了解数据库的三范式么？

经过研究和对使用中问题的总结，对于设计数据库提出了一些规范，这些规范被称为范式 一般需要遵守下面3范式即可: 第一范式（1NF）：强调的是列的原子性，即列不能够再分成其他几列。 第二范式（2NF）：首先是 1NF，另外包含两部分内容，一是表必须有一个主键；二是没有包含在主键中的列必须完全依赖于主键，而不能只依赖于主键的一部分。 第三范式（3NF）：首先是 2NF，另外非主键列必须直接依赖于主键，不能存在传递依赖。即不能存在：非主键列 A 依赖于非主键列 B，非主键列 B 依赖于主键的情况。



#### 了解分布式锁么

分布式锁是控制分布式系统之间的同步访问共享资源的一种方式。 对于分布式锁的目标，我们必须首先明确三点：

- 任何一个时间点必须只能够有一个客户端拥有锁。
- 不能够有死锁，也就是最终客户端都能够获得锁，尽管可能会经历失败。
- 错误容忍性要好，只要有大部分的Redis实例存活，客户端就应该能够获得锁。 分布式锁的条件 互斥性：分布式锁需要保证在不同节点的不同线程的互斥 可重入性：同一个节点上的同一个线程如果获取了锁之后，能够再次获取这个锁。 锁超时：支持超时释放锁，防止死锁 高效，高可用：加锁和解锁需要高效，同时也需要保证高可用防止分布式锁失效，可以增加降级。 支持阻塞和非阻塞：可以实现超时获取失败，tryLock(long timeOut) 支持公平锁和非公平锁

分布式锁的实现方案 1、数据库实现（乐观锁） 2、基于zookeeper的实现 3、基于Redis的实现（推荐）



#### 用 Python 实现一个 Reids 的分布式锁的功能

REDIS分布式锁实现的方式：SETNX + GETSET,NX是Not eXists的缩写，如SETNX命令就应该理解为：SET if Not eXists。 多个进程执行以下Redis命令：

```
SETNX lock.foo <current Unix time + lock timeout + 1>
```

如果 SETNX 返回1，说明该进程获得锁，SETNX将键 lock.foo 的值设置为锁的超时时间（当前时间 + 锁的有效时间）。 如果 SETNX 返回0，说明其他进程已经获得了锁，进程不能进入临界区。进程可以在一个循环中不断地尝试 SETNX 操作，以获得锁。

```python
import time
import redis
from conf.config import REDIS_HOST, REDIS_PORT, REDIS_PASSWORD

class RedisLock:
    def __init__(self):
        self.conn = redis.Redis(host=REDIS_HOST, port=REDIS_PORT, password=REDIS_PASSWORD, db=1)
        self._lock = 0
        self.lock_key = ""
    @staticmethod
    def my_float(timestamp):
        """
        Args:
            timestamp:
        Returns:
            float或者0
            如果取出的是None，说明原本锁并没人用，getset已经写入，返回0，可以继续操作。
        """
        if timestamp:
            return float(timestamp)
        else:
            #防止取出的值为None，转换float报错
            return 0
          
    @staticmethod
    def get_lock(cls, key, timeout=10):
        cls.lock_key = f"{key}_dynamic_lock"
        while cls._lock != 1:
            timestamp = time.time() + timeout + 1
            cls._lock = cls.conn.setnx(cls.lock_key, timestamp)
            # if 条件中，可能在运行到or之后被释放，也可能在and之后被释放
            # 将导致 get到一个None，float失败。
            if cls._lock == 1 or (
                            time.time() > cls.my_float(cls.conn.get(cls.lock_key)) and
                            time.time() > cls.my_float(cls.conn.getset(cls.lock_key, timestamp))):
                break
            else:
                time.sleep(0.3)

    @staticmethod
    def release(cls):
        if cls.conn.get(cls.lock_key) and time.time() < cls.conn.get(cls.lock_key):
            cls.conn.delete(cls.lock_key)


def redis_lock_deco(cls):
    def _deco(func):
        def __deco(*args, **kwargs):
            cls.get_lock(cls, args[1])
            try:
                return func(*args, **kwargs)
            finally:
                cls.release(cls)
        return __deco
    return _deco


@redis_lock_deco(RedisLock())
def my_func():
    print("myfunc() called.")
    time.sleep(20)

if __name__ == "__main__":
    my_func()
```



#### 写一段 Python 使用 mongo 数据库创建索引的代码:



### 高级特性

#### 函数装饰器有什么作用？请列举说明？

装饰器就是一个函数，它可以在不需要做任何代码变动的前提下给一个函数增加额外功能，启动装饰的效果。 它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。 下面是一个日志功能的装饰器

```python
from functools import wraps

def log(label):
    def decorate(func):
        @wraps(func)
        def _wrap(*args, **kwargs):
            try:
                func(*args, **kwargs)
                print('name', func.__name__)
            except Exception as e:
                print(e)

        return _wrap
    return decorate

@log('info')
def foo(a, b, c):
    print(a + b +c)
    print("in foo")

foo(1, 2, 3)
```





#### Python 垃圾回收机制？

1.引用计数机制 

```
当创建一个对象的实例并在堆上申请内存时，对象的引用计数就为1，在其他对象中需要持有这个对象时，就需要把该对象的引用计数加1，需要释放一个对象时，就将该对象的引用计数减1，直至对象的引用计数为0，对象的内存会被立刻释放。

优点：
  尽快的回收不再被使用的对象
  回收的过程中不会导致长时间的停顿
  可以清晰的标明一个对象的生存周期

缺点：
  频繁更新引用计数会降低运行效率
  无法解决循环引用的问题（两个对象甲乙，甲持有乙并且乙持有甲，此时甲乙的引用计数会永远大于等于1。）
```



2.标记-清除 

```
标记阶段：
	从根对象开始遍历所有对象，如果是可达的（reachable），也就是还有对象引用它，那么就标记该对象可达。
清除：
	再次遍历对象，如果发现某个对象没有标记为可达，就将其回收掉。
	
为了实现上述的两个步骤，Python 维护了两个链表，一个是 root 链表，用于存放待扫描的对象，另一个是 unreachable 链表，用于存放不可达对象。每个对象的 gc_ref 会被初始化为引用计数的值。

过程：
	遍历 root 链表的对象，将当前对象所引用的对象的 gc_ref减一
	再次扫描 root 链表的对象，如果对象的 gc_ref为0，那么这个对象就被标记为 GC_TENTATIVELY_UNREACHABLE，并且被移动到 Unreachable链表中。
	如果对象的gc_ref不为0，那么这个对象就会被标记为 GC_REACHABLE。当发现有一个节点是可达的时候，会递归的将从该节点出发可以到达的所有节点标记为为 GC_REACHABLE，如果该节点在 Unreachable链表中的话，还需要将其移回到 root 链表中。
	将存在于Unreachable链表中的对象释放掉。
	
优点：
	解决了引用计数无法解决的循环引用的问题
缺点：
  过程需要中断程序
  产生大量零碎的空闲空间碎片
```



3.分代回收

```
对象存在时间越长，越不可能是垃圾，应该越少去收集。这样在执行标记-清除算法时可以有效减少遍历的对象数，从而提高垃圾回收的速度。

Python gc 给对象定义了三种 generation （zero，one，two），每一个新生对象在 generation zero 中，如果它在一轮 gc 扫描中活了下来，那么它将被移至 generation one，在那里他将较少的被扫描，如果他有活过了一轮 gc，它将被移至 generation two，在那里他被扫描的次数将会更少。

gc的扫描在什么时候触发呢？当某一 generation 中被分配的对象与被释放的对象之差达到某一阈值的时候，就会触发对该 generation 的扫描（同时也会扫描比该 generation 年轻的 generation ）。

优点：
	改善 GC 所花费的时间，提高 GC 效率
缺点：
	额外的空间开销
```



#### 魔法函数 _*call*_怎么使用?

 _*call*_ 可以把类实例当做函数调用。 使用示例如下

```python
class Duck:
    def __call__(self, *args, **kwargs):
        print('I am a duck!')

d = Duck()
d()
```



#### 如何判断一个对象是函数还是方法？

```python
from types import MethodType, FunctionType

class Bar:
    def foo(self):
        print('method')

def foo():
    print('function')

print(isinstance(Bar().foo, MethodType))
print(isinstance(Bar().foo, FunctionType))
print(isinstance(foo, MethodType))
print(isinstance(foo, FunctionType))
```



#### @classmethod 和 @staticmethod 用法和区别

相同之处：@staticmethod 和@classmethod 都可以直接类名.方法名()来调用，不用在示例化一个类。

@classmethod

```python
def iget_no_of_instance(ins_obj)：
    return ins_obj.__class__.no_inst


class Kls(object)：
    no_inst = 0

    def __init__(self)：
        Kls.no_inst = Kls.no_inst + 1


ik1 = Kls()
ik2 = Kls()
print(iget_no_of_instance(ik1))
```

@staticmethod 经常有一些跟类有关系的功能但在运行时又不需要实例和类参与的情况下需要用到静态方法

```python
IND = 'ON'


class Kls(object)：
    def __init__(self, data)：
        self.data = data

    @staticmethod
    def check_ind()：
        return (IND == 'ON')

    def do_reset(self)：
        if self.check_ind()：
            print('Reset done for：', self.data)

    def set_db(self)：
        if self.check_ind()：
            self.db = 'New db connection'
        print('DB connection made for： ', self.data)


ik1 = Kls(12)
ik1.do_reset()
ik1.set_db()
```



#### Python 中的接口如何实现？

接口提取了一群类共同的函数，可以把接口当做一个函数的集合，然后让子类去实现接口中的函数。但是在 Python 中根本就没有一个叫做 interface 的关键字，如果非要去模仿接口的概念，可以使用抽象类来实现。抽象类是一个特殊的类，它的特殊之处在于只能被继承，不能被实例化。使用 abc 模块来实现抽象类。



#### metaclass 作用？以及应用场景？

metaclass 即元类，metaclass 是类似创建类的模板，所有的类都是通过他来 create 的(调用**new**)，这使得你可以自由的控制创建类的那个过程，实现你所需要的功能。 我们可以使用元类创建单例模式和实现 ORM 模式。



#### hasattr()、getattr()、setattr() 的用法

这三个方法属于 Python 的反射机制里面的，hasattr 可以判断一个对象是否含有某个属性，getattr 可以充当 get 获取对象属性的作用。而 setattr 可以充当 person.name = "liming"的赋值操作.



#### 请列举你知道的 Python 的魔法方法及用途。

```bash
1 __init__：
类的初始化方法。它获取任何传给构造器的参数（比如我们调用 x = SomeClass(10, ‘foo’) ， __init__就会接到参数 10 和 ‘foo’ 。 __init__在 Python 的类定义中用的最多。

2 __new__：
__new__是对象实例化时第一个调用的方法，它只取下 cls 参数，并把其他参数传给 __init__ 。 __new__很少使用，但是也有它适合的场景，尤其是当类继承自一个像元组或者字符串这样不经常改变的类型的时候.

3 __del__：
__new__和 __init__是对象的构造器， __del__是对象的销毁器。它并非实现了语句 del x (因此该语句不等同于 x.__del__())。而是定义了当对象被垃圾回收时的行为。 当对象需要在销毁时做一些处理的时候这个方法很有用，比如 socket 对象、文件对象。但是需要注意的是，当 Python 解释器退出但对象仍然存活的时候，__del__并不会 执行。 所以养成一个手工清理的好习惯是很重要的，比如及时关闭连接。
```



#### 如何知道一个 Python 对象的类型？

type(object)



#### Python 的传参是传值还是传址？

Python 中的传参即不是传值也不是传地址，传的是对象的引用。



#### Python 中的元类 (metaclass) 使用举例

可以使用元类实现一个单例模式，代码如下

```python
class Singleton(type):
    def __init__(self, *args, **kwargs):
        print("in init")
        self.__instance = None
        super(Singleton, self).__init__(*args, **kwargs)

    def __call__(self, *args, **kwargs):
        print("in call")
        if self.__instance is None:
            self.__instance = super(Singleton, self).__call__(*args, **kwargs)
        print(self.__instance)
        return self.__instance

class Foo(metaclass=Singleton):
    pass

f1 = Foo()
f2 = Foo()
print(f1 is f2)

```



#### 简述 any() 和 all() 方法

any(x)：判断 x 对象是否为空对象，如果都为空、0、false，则返回 false，如果不都为空、0、false，则返回 true。 all(x)：如果 all(x) 参数 x 对象的所有元素不为 0、''、False 或者 x 为空对象，则返回 True，否则返回 False。



#### filter 方法求出列表所有奇数并构造新列表，a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

print(list(filter(lambda num: num % 2 > 0, a)))
```



#### 什么是猴子补丁？

猴子补丁（monkey patching)：在运行时动态修改模块、类或函数，通常是添加功能或修正缺陷。猴子补丁在代码运行时内存中）发挥作用，不会修改源码，因此只对当前运行的程序实例有效。因为猴子补丁破坏了封装，而且容易导致程序与补丁代码的实现细节紧密耦合，所以被视为临时的变通方案，不是集成代码的推荐方式。



#### 在 Python 中是如何管理内存的？

垃圾回收：Python 不像 C++，Java 等语言一样，他们可以不用事先声明变量类型而直接对变量进行赋值。对 Python 语言来讲，对象的类型和内存都是在运行时确定的。这也是为什么我们称 Python 语言为动态类型的原因（这里我们把动态类型可以简单的归结为对变量内存地址的分配是在运行时自动判断变量类型并对变量进行赋值）。

引用计数：Python 采用了类似 Windows 内核对象一样的方式来对内存进行管理。每一个对象，都维护这一个对指向该对对象的引用的计数。当变量被绑定在一个对象上的时候，该变量的引用计数就是 1，(还有另外一些情况也会导致变量引用计数的增加)，系统会自动维护这些标签，并定时扫描，当某标签的引用计数变为 0 的时候，该对就会被回收。

- 内存池机制 Python 的内存机制以金字塔行，1、2 层主要有操作系统进行操作
- 第 0 层是 C 中的 malloc，free 等内存分配和释放函数进行操作
- 第 1 层和第 2 层是内存池，有 Python 的接口函数 PyMem_Malloc 函数实现，当对象小于 256K 时有该层直接分配内存
- 第 3 层是最上层，也就是我们对 Python 对象的直接操作
- 在 C 中如果频繁的调用 malloc 与 free 时,是会产生性能问题的.再加上频繁的分配与释放小块的内存会产生内存碎片。Python 在这里主要干的工作有：
  - 如果请求分配的内存在 1~256 字节之间就使用自己的内存管理系统,否则直接使用 malloc。
  - 这里还是会调用 malloc 分配内存，但每次会分配一块大小为 256k 的大块内存。
- 经由内存池登记的内存到最后还是会回收到内存池，并不会调用 C 的 free 释放掉以便下次使用。对于简单的 Python 对象，例如数值、字符串，元组（tuple 不允许被更改)采用的是复制的方式(深拷贝?)，也就是说当将另一个变量 B 赋值给变量 A 时，虽然 A 和 B 的内存空间仍然相同，但当 A 的值发生变化时，会重新给 A 分配空间，A 和 B 的地址变得不再相同。



#### 当退出 Python 时是否释放所有内存分配？

不是的，循环引用其他对象或引用自全局命名空间的对象的模块，在 Python 退出时并非完全释放。

另外，也不会释放 c 库保留的内存部分



### 正则表达式

#### 1）使用正则表达式匹配出`<html><h1\>www.baidu.com</h1></html>`中的地址（2）a="张明 98 分"，用 re.sub，将 98 替换为 100

(1)

```python
import re

s = "<html><h1>www.baidu.com</h1></html>"
print(re.findall(".*?<h1>(.*?)</h1></html>", s))
```

(2)

```python
import re

a="张明 98 分"
print(re.sub("\d+", "100", a))
```



#### 正则表达式匹配中(.*)和(.*?)匹配区别？

(.*) 为贪婪模式极可能多的匹配内容 ,(.*?) 为非贪婪模式又叫懒惰模式，一般匹配到结果就好，匹配字符的少为主



#### 写一段匹配邮箱的正则表达式

```python
import re

a = "example_001@gmail.com"

pat = re.compile("^[a-zA-Z0-9]+[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$")
print(pat.findall(a))

# 通用邮箱正则匹配
"""(?：[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?：\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?：[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?：(?：[a-z0-9](?：[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?：[a-z0-9-]*[a-z0-9])?|\[(?：(?：25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?：25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]：(?：[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])"""

```



### 其他内容

#### 解释一下 Python 中 pass 语句的作用？

pass 实际上就是一个占位符，在写一个函数但是不确定里面写啥的时候，这个时候可以使用 pass。



####  Python 中的作用域

Python 中，一个变量的作用域总是由在代码中被赋值的地方所决定 当 Python 遇到一个变量的话它会按照这的顺序进行搜索 本地作用域(Local)--->当前作用域被嵌入的本地作用域(Enclosing locals)--->全局/模块作用域(Global)--->内置作用域(Built-**in**)



#### 三元运算写法和应用场景？

Python 中的三元运算又称三目运算，是对简单的条件语句的简写。 是一种比较 Pythonic 的学法，形式为：val = 1 if 条件成立 else 2 代码示例如下：

```python
def run(val):
    return True if val > 60 else False

print(run(100))
```



#### 了解 enumerate 么？

enumerate 可以在迭代一个对象的时候，同时获取当前对象的索引和值。 代码示例如下

```python
from string import ascii_lowercase

for idx, i in enumerate(ascii_lowercase):
    print(idx, i)

```



#### 列举 5 个 Python 中的标准模块

pathlib：路径操作模块，比 os 模块拼接方便。 urllib：网络请求模块，包括对 url 的结构解析。 asyncio： Python 的异步库，基于事件循环的协程模块。 re：正则表达式模块。 itertools：提供了操作生成器的一些模块。



#### 如何在函数中设置一个全局变量

```python
count = 0

def run():
    global count
    count = count + 1

    print(count)

run()
```



#### pathlib 的用法举例

具体已收录在印象笔记里



#### Python 中的异常处理，写一个简单的应用场景

```python
try:
    1 + ''
except Exception as e:
    print(e)
```



#### Python 中递归的最大次数，那如何突破呢？

默认最大1000次，突破方法

```python
import sys
sys.setrecursionlimit(1500)
```

另外需要注意的是 sys.setrecursionlimit() 只是修改解释器在解释时允许的最大递归次数，此外，限制最大递归次数的还和操作系统有关。



#### 什么是面向对象的 mro

Python 是支持面向对象编程的，同时也是支持多重继承的。一般我们通过调用类对象的 mro()方法获取其继承关系。



#### isinstance 作用以及应用场景？

isinstance 是判断一个对象是否为另一个对象的子类的，例如我们知道在 Python3 中 bool 类型其实是 int 的子类，所以我们可以对其检测。



#### 什么是断言？应用场景？

在 Python 中是断言语句 assert 实现此功能，一般在表达式为 True 的情况下，程序才能通过。



#### lambda 表达式格式以及应用场景？

lambda 表达式其实就是一个匿名函数,在函数编程中经常作为参数使用。



#### 新式类和旧式类的区别

Python 2.x 中默认都是经典类，只有显式继承了 object 才是新式类，Python 3.x 中默认都是新式类，经典类被移除，不必显式的继承 object。 新式类都从 object 继承，经典类不需要。 新式类的 MRO(method resolution order 基类搜索顺序)算法采用 C3 算法广度优先搜索，而旧式类的 MRO 算法是采用深度优先搜索。 新式类相同父类只执行一次构造函数，经典类重复执行多次。



#### dir()是干什么用的？

当在使用某一个对象不知道有哪些属性或者方法可以使用时，此时可以通过 dir() 方法进行查看。



#### 一个包里有三个模块，demo1.py、demo2.py、demo3.py，但使用 from tools import *导入模块时，如何保证只有 demo1、demo3 被导入了。

```python
# 在__init__.py文件中加入
__all__ = ["demo1.py", "demo3.py"]
```



#### 列举 5 个 Python 中的异常类型以及其含义

AttributeError 对象没有这个属性 

NotImplementedError 尚未实现的方法

 **StopIteration** 迭代器没有更多的值

 **TypeError** 对类型无效的操作

 IndentationError 缩进错误



#### copy 和 deepcopy 的区别是什么？

copy.copy()浅拷贝，只拷贝父对象，不会拷贝对象的内部的子对象。 copy.deepcopy()深拷贝，拷贝对象及其子对象。



#### 代码中经常遇到的*args, **kwargs 含义及用法。

在函数定义中使用 *args 和**kwargs 传递可变长参数。 *args 用来将参数打包成 tuple 给函数体调用。 **kwargs 打包关键字参数成 dict 给函数体调用。



#### Python 中会有函数或成员变量包含单下划线前缀和结尾，和双下划线前缀结尾，区别是什么?

"单下划线" 开始的成员变量叫做保护变量，意思是只有类对象和子类对象自己能访问到这些变量； "双下划线" 开始的是私有成员，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据。

以单下划线开头（_foo）的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用“from xxx import *”而导入；以双下划线开头的（__foo）代表类的私有成员；

以双下划线开头和结尾的（_*foo**）代表 Python 里特殊方法专用的标识，如 _*****init**（）代表类的构造函数。

```python
class Person：
    """docstring for ClassName"""
    def __init__(self)：
       self.__age = 12
       self._sex = 12
    def _sex(self)：
        return "男"
    def set_age(self,age)：
        self.__age = age

    def get_age(self)：
        return self.__age   

if __name__ == '__main__'：
    p=Person()
    print(p._sex)
    #print(p.__age)
    #Python 自动将__age 解释成 _Person__age,于是我们用 _Person__age 访问，这次成功。
    print(p._Person__age)
```



#### w、a+、wb 文件写入模式的区别

 w 表示写模式支持写入字符串，如果文件存在则覆盖。 a+ 和 w 的功能类型不过如果文件存在的话内容不会覆盖而是追加。 wb 是写入二进制字节类型的数据。



#### 举例 sort 和 sorted 的区别

相同之处 sort 和 sorted 都可以对列表元素排序，sort() 与 sorted() 的不同在于，sort 是在原位重新排列列表，而 sorted() 是产生一个新的列表。 sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。

list 的 sort 方法返回的是对已经存在的列表进行操作，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。



#### 什么是负索引？

负索引一般表示的是从后面取元素。



#### pprint 模块是干什么的？

pprint 是 print 函数的美化版，可以通过 import pprint 导入。



#### 解释一下 Python 中的赋值运算符

```python
# 通过下面的代码列举出所有的赋值运算符
a=7
a+=1
print(a)
a-=1
print(a)
a*=2
print(a)
a/=2
print(a)
a**=2
print(a)
a//=3
print(a)
a%=4
print(a)
```



#### 解释一下 Python 中的逻辑运算符

Python 中有三个逻辑运算符：and、or、not



#### 在 Python 中如何使用多进制数字？

我们在 Python 中，除十进制外还可以使用二进制、八进制和十六进制

- 二进制数字由 0 和 1 组成，我们使用 0b 或 0B 前缀表示二进制数

  ```python
  print(0b10101)
  ```

- 使用 bin()函数将一个数字转换为它的二进制形式

  ```python
  print(bin(0xf))
  ```

- 八进制数由数字 0-7 组成，用前缀 0o 或 0O 表示 8 进制数

  ```python
  print(oct(8))
  ```

- 十六进数由数字 0-15 组成，用前缀 0x 或者 0X 表示 16 进制数

  ```python
  print(hex(16))
  ```

  

#### 怎样声明多个变量并赋值？

```python
a, b, c = 1, 2, 3
```



### 算法和数据结构

#### 已知：

```python
AList = [1,2,3]
BSet = {1,2,3}
```

(1) 从 AList 和 BSet 中 查找 4，最坏时间复杂度哪个大？ (2) 从 AList 和 BSet 中 插入 4，最坏时间复杂度哪个大？

(1) 对于查找，列表和集合的最坏时间复杂度都是 O(n)，所以一样的。 (2) 列表操作插入的最坏时间复杂度为 o(n),集合为 o(1)，所以 Alist 大。 set 是哈希表所以操作的复杂度基本上都是 o(1)。



#### 用 Python 实现一个二分查找的函数

```python
def binary_search(arr, target):
    left = 0
    right = len(arr) - 1

    while left <= right:
      	# (n-m)/2 + m = (n + m)/2
        mid = (left + right) // 2
        if arr[mid] < target:
            left = mid + 1
        elif arr[mid] > target:
            right = mid - 1
        else:
            print(f"index: {mid}, value: {arr[mid]}")
            return True
    return False

arr = [1,3,4,5,6,7,8]

binary_search(arr, 8)
```



#### Python 单例模式的实现方法

```python
class Book:
    def __new__(cls, title):
        if not hasattr(cls, "_ins"):
            cls._ins = super().__new__(cls)
        print('in __new__')
        return cls._ins

    def __init__(self, title):
        print("in __init__")
        super().__init__()
        self.title = title

if __name__ == '__main__':
    b = Book('The Spider Book')
    b2 = Book('The Flask Book')
    print(id(b))
    print(id(b2))
    print(b.title)
    print(b2.title)
```

线程安全单例模式

```python
import threading

def synchronized(func):
    func.__lock__ = threading.Lock()

    def lock_func(*args, **kwargs):
        with func.__lock__:
            return func(*args, **kwargs)

    return lock_func

class Singleton(object):
    instance = None

    @synchronized
    def __new__(cls, *args, **kwargs):
        """
        :type kwargs: object
        """
        if cls.instance is None:
            cls.instance = super().__new__(cls)
        return cls.instance

    def __init__(self, num):
        self.a = num + 5

    def printf(self):
        print(self.a)
```



#### 使用 Python 实现一个斐波那契数列

斐波那契数列：数列从第 3 项开始，每一项都等于前两项之和。

```python
def fibonacci(num):
    a, b = 0, 1
    output = []
    for i in range(num):
        a, b = b, a + b
        output.append(b)
    print(output)

fibonacci(5)
```



#### 找出列表中的重复数字

```python
arr = [1, 2, -3, 4, 4, 95, 95, 5, 2, 2, -3, 7, 7, 5]

def duplicate(arr):

    if arr is None or len(arr) <= 1:
        return {}

    num_set = set()
    duplication = {}

    for idx, i in enumerate(arr):
        if i not in num_set:
            num_set.add(i)
        else:
            duplication[idx] = i

    return duplication

print(duplicate(arr))
```



#### 找出列表中的单个数字

使用位运算做遍历异或操作

```python
arr = [1,2,3,4,5,6,2,3,4,5,6]
def find_single(arr):

    result = 0
    for i in arr:
        result ^= i

    if result == 0:
        print("没有落单元素")
    else:
        print(f"落单元素 {result}")

find_single(arr)
```



#### 根据前序遍历和中序遍历的结果，请重建出该二叉树

```python
pre_order = [1, 2, 4, 7, 3, 5, 6, 8]
mid_order = [4, 7, 2, 1, 5, 3, 8, 6]

class Node:
  def __init__(self, data, left, right):
    self.data = data
    self.left = left
    self.right = right

def construct_tree(pre_order, mid_order):
    if not pre_order:
        return

    # 根据先序遍历确定当前树根节点
    root_data = pre_order[0]

    # 在中序遍历中定位当前树的根节点位置
    for i in range(len(mid_order)):
        if mid_order[i] == root_data:
            break

    left = construct_tree(pre_order[1: 1+i], mid_order[:i])
    right = construct_tree(pre_order[1+i:], mid_order[i+1:])

    return Node(root_data, left, right)
```



#### 写一个冒泡排序

```python
arr = [1, 2, 3, 4, 5, 55, 6, 3, 4, 5, 6]

def bubble_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        for j in range(n - i -1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
        print(arr)

    print(arr)

bubble_sort(arr)
```



#### 写一个快速排序

```python
def quick_sort(arrays, l, r):
    if l < r:
        q = partitions(arrays, l, r)
        quick_sort(arrays, l, q - 1)
        quick_sort(arrays, q + 1, r)

def partitions(arrays, l, r):
    val = arrays[r]
    idx = l

    for i in range(l, r):
        if arrays[i] < val:
            arrays[idx], arrays[i] = arrays[i], arrays[idx]
            idx += 1

    arrays[idx],arrays[r] = arrays[r], arrays[idx]

    return idx
```



#### 写一个拓扑排序

#### Python 实现一个二进制计算

```python
def binary_add(a, b):
    result = bin(int(a, 2) + int(b, 2))
    return result[2:]

if __name__ == '__main__':
    a = input("a: ")
    b = input("a: ")
    print(binary_add(a, b))
```



#### 有一组“+”和“-”符号，要求将“+”排到左边，“-”排到右边，写出具体的实现方法。

```python
s = "++++++----+++----"

def func1(s):

    ns = s.replace('+', "0").replace('-', '1')

    return ''.join(sorted(ns)).replace('0', "+").replace('1', '-')

print(func1(s))
```

#### 单链表反转

```python
class Node:
    def __init__(self, val=None):
        self.val = val
        self.next = None

n1 = Node(1)
n2 = Node(2)
n3 = Node(3)
n4 = Node(4)
n5 = Node(5)

n1.next = n2
n2.next = n3
n3.next = n4
n4.next = n5

root = n1

class SingleLinkList:
    def __init__(self, root):
        self.head = root

    def output_link(self):
        current = self.head
        while current:
            print(current.val)
            current = current.next

    def reverse(self):
        """
        单链表反转
        :return:
        """
        pre = None
        current = self.head
        while current:
            tmp = current.next
            current.next = pre
            pre = current
            current = tmp
        self.head = pre

        self.output_link()
```

#### 交叉链表求交点

cur1、cur2，2 个指针的初始位置是链表 headA、headB 头结点，cur1、cur2 两个指针一直往后遍历。 直到 cur1 指针走到链表的末尾，然后 cur1 指向 headB； 直到 cur2 指针走到链表的末尾，然后 cur2 指向 headA； 然后再继续遍历； 每次 cur1、cur2 指向 None，则将 cur1、cur2 分别指向 headB、headA。 循环的次数越多，cur1、cur2 的距离越接近，直到 cur1 等于 cur2。则是两个链表的相交点。

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

n1 = Node(1)
n2 = Node(2)
n3 = Node(3)
n4 = Node(4)
n5 = Node(5)
n6 = Node(6)
n7 = Node(7)

n1.next = n7

n2.next = n3
n3.next = n4
n4.next = n5
n5.next = n6
n6.next = n7

head1 = n1
head2 = n2

def getIntersectionNode(head1, head2):
    cur1 = head1
    cur2 = head2

    while cur1.val != cur2.val:
        cur1 = cur1.next if cur1.next else head1
        cur2 = cur2.next if cur2.next else head2

    print(cur1.val)
```

#### 用队列实现栈

一个队列实现

```python
from queue import Queue

# 1个队列实现栈
class MyStack:
    def __init__(self):
        self._q = Queue()

    def push(self, val):
        """
        入栈
        :param val:
        :return:
        """
        self._q.put(val)

    def pop(self):
        """
        出栈
        :return:
        """
        count = self._q.qsize()
        if count == 0:
            return False

        while count > 1:
            self._q.put(self._q.get())
            count -= 1

        print(self._q.get())
```

两个队列实现

```python
from queue import Queue

# 2个队列实现栈
class MyStack:
    def __init__(self):
        self.q1 = Queue()
        self.q2 = Queue()

    def push(self, val):
        """
        入栈
        :param val:
        :return:
        """
        self.q1.put(val)

    def pop(self):
        """
        出栈
        :return:
        """
        if self.q1.qsize() == 0:
            return False

        while self.q1.qsize() > 1:
            self.q2.put(self.q1.get())
        if self.q1.qsize() == 1:
            res = self.q1.get()
            self.q1,self.q2 = self.q2, self.q1
            print(res)
```

#### 找出数据流的中位数

对于一个升序排序的数组，中位数为左半部分的最大值，右半部分的最小值，而左右两部分可以是无需的，只要保证左半部分的数均小于右半部分即可。因此，左右两半部分分别可用最大堆、最小堆实现。

如果有奇数个数，则中位数放在左半部分；如果有偶数个数，则取左半部分的最大值、右边部分的最小值之平均值。

分两种情况讨论： 当目前有偶数个数字时，数字先插入最小堆，然后选择最小堆的最小值插入最大堆（第一个数字插入左半部分的最小堆）。

当目前有奇数个数字时，数字先插入最大堆，然后选择最大堆的最大值插入最小堆。 最大堆：根结点的键值是所有堆结点键值中最大者，且每个结点的值都比其孩子的值大。 最小堆：根结点的键值是所有堆结点键值中最小者，且每个结点的值都比其孩子的值小。

```python
from heapq import *

class Solution:
    def __init__(self):
        self.maxheap = []
        self.minheap = []

    def Insert(self, num):
        if (len(self.maxheap) + len(self.minheap)) & 0x1:
            # 总数为奇数
            if len(self.minheap) > 0:
                if num > self.minheap[0]:
                    heappush(self.minheap, num)
                    heappush(self.maxheap, -self.minheap[0])
                    heappop(self.minheap)
                else:
                    heappush(self.maxheap, -num)
            else:
                heappush(self.maxheap, -num)
        else:
            if len(self.maxheap) > 0:
                if num < -self.maxheap[0]:
                    heappush(self.maxheap, -num)
                    heappush(self.minheap, -self.maxheap[0])
                    heappop(self.maxheap)
                else:
                    heappush(self.minheap, num)
            else:
                heappush(self.minheap, num)

    def GetMedian(self):
        if (len(self.maxheap) + len(self.minheap)) & 0x1:
            mid = self.minheap[0]
        else:
            mid = (self.minheap[0] - self.maxheap[0]) / 2
        return mid
```

#### 二叉搜索树中第 K 小的元素

```python
class TreeNode：
    def __init__(self, x)：
        self.val = x
        self.left = None
        self.right = None


class Solution：
    count = 0
    nodeVal = 0

    def kthSmallest(self, root, k)：
        """
        ：type root： TreeNode
        ：type k： int
        ：rtype： int
        """
        self.dfs(root, k)
        return self.nodeVal

    def dfs(self, node, k)：
        if node != None：
            self.dfs(node.left, k)
            self.count = self.count + 1
            if self.count == k：
                self.nodeVal = node.val
                # 将该节点的左右子树置为 None,来结束递归，减少时间复杂度
                node.left = None
                node.right = None
            self.dfs(node.right, k)
```

### 爬虫相关

#### 简要写一下 lxml 模块的使用方法框架

```python
from lxml import html

source='''
<div class="nam"><span>中国</span></div>
'''
root = html.fromstring(source)

_content = root.xpath("string(//div[@class='nam'])")

if _content and isinstance(_content, list):
     content = _content[0]
elif isinstance(_content, str):
    content = _content

print(_content)
```

#### 说一说 scrapy 的工作流程

1. spider 把百度需要下载的第一个 url：www.baidu.com 交给引擎。
2. 引擎把 url 交给调度器排序入队处理。
3. 调度器把处理好的 request 返回给引擎。
4. 通过引擎调动下载器，按照下载中间件的设置下载这个 request。
5. 下载器下载完毕结果返回给引擎（如果失败：不好意思，这个 request 下载失败，然后引擎告诉调度器，这个 request 下载失败了，你记录一下，我们待会儿再下载。）
6. 引擎调度 spider，把按照 Spider 中间件处理过了的请求，交给 spider 处理。
7. spider 把处理好的 url 和 item 传给引擎。
8. 引擎根据不同的类型调度不同的模块，调度 Item Pipeline 处理 item。
9. 把 url 交给调度器。 然后从第 4 步开始循环，直到获取到你需要的信息

#### scrapy 的去重原理

crapy 本身自带一个去重中间件，scrapy 源码中可以找到一个 dupefilters.py 去重器。里面有个方法叫做 request_seen，它在 scheduler(发起请求的第一时间)的时候被调用。它代码里面调用了 request_fingerprint 方法（就是给 request 生成一个指纹）。

就是给每一个传递过来的 url 生成一个固定长度的唯一的哈希值。但是这种量级千万到亿的级别内存是可以应付的。

#### scrapy 中间件有几种类，你用过哪些中间件

scrapy 的中间件理论上有三种(Schduler Middleware,Spider Middleware,Downloader Middleware)。在应用上一般有以下两种

1. 爬虫中间件 Spider Middleware：主要功能是在爬虫运行过程中进行一些处理。
2. 下载器中间件 Downloader Middleware：这个中间件可以实现修改 User-Agent 等 headers 信息，处理重定向，设置代理，失败重试，设置 cookies 等功能。

#### 列出你知道 header 的内容以及信息

User-Agent：User-Agent 的内容包含发出请求的用户信息。

Accept：指定客户端能够接收的内容类型。

Accept-Encoding：指定浏览器可以支持的 web 服务器返回内容压缩编码类型。 

Accept-Language：浏览器可接受的语言。 

Connection：表示是否需要持久连接。（HTTP 1.1 默认进行持久连接）。 

Content-Length：请求的内容长度。 

If-Modified-Since：如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回 304 代码。

Referer：先前网页的地址，当前请求网页紧随其后，即来路。

#### 说一说打开浏览器访问 www.baidu.com 获取到结果，整个流程。

浏览器向 DNS 服务器发送 baidu.com 域名解析请求。 DNS 服务器返回解析后的 ip 给客户端浏览器，浏览器想该 ip 发送页面请求。 DNS 服务器接收到请求后，查询该页面，并将页面发送给客户端浏览器。 客户端浏览器接收到页面后，解析页面中的引用，并再次向服务器发送引用资源请求。 服务器接收到资源请求后，查找并返回资源给客户端。 客户端浏览器接收到资源后，渲染，输出页面展现给用户。

#### 写爬虫是用多进程好？还是多线程好？ 为什么？

多线程，因为爬虫是对网络操作属于 io 密集型操作适合使用多线程或者协程。

#### 使用最多的数据库（mysql，mongodb，redis 等），对他的理解？

MySQL 数据库：开源免费的关系型数据库，需要实现创建数据库、数据表和表的字段，表与表之间可以进行关联（一对多、多对多），是持久化存储。

mongodb 数据库：是非关系型数据库，数据库的三元素是，数据库、集合、文档，可以进行持久化存储，也可作为内存数据库，存储数据不需要事先设定格式，数据以键值对的形式存储。

redis 数据库：非关系型数据库，使用前可以不用设置格式，以键值对的方式保存，文件格式相对自由，主要用与缓存数据库，也可以进行持久化存储。



### 网络

#### TCP的可靠连接怎么保证

校验和：发送的数据包的二进制相加然后取反，**目的是检测数据在传输过程中的任何变化**。如果收到段的检验和有差错，TCP将丢弃这个报文段和不确认收到此报文段。

确认应答+序列号：TCP给发送的**每一个包进行编号**，接收方***\*对数据包进行排序\****，把有序数据传送给应用层。

超时重传：当TCP**发出一个段后，它启动一个定时器**，**等待目的端确认收到这个报文段**。**如果不能及时收到一个确认，将重发这个报文段**。 

流量控制：**TCP连接的每一方都有固定大小的缓冲空间**，TCP的**接收端只允许发送端发送接收端缓冲区能接纳的数据**。

拥塞控制：网络拥塞时，减少数据的发送。



#### tcp和udp的区别

1. 基于连接与无连接
2. TCP要求系统资源较多，UDP较少
3. UDP程序结构较简单
4. 流模式（TCP）与数据报模式(UDP);
5. TCP保证数据正确性，UDP可能丢包
6. TCP保证数据顺序，UDP不保证



#### 怎么能够让UDP实现可靠呢？

类似http3的QUIC协议

QUIC协议是基于UDP协议实现的，在一条链接上可以有多个流，流与流之间是互不影响的，当一个流出现丢包影响范围非常小，从而解决队头阻塞问题。

QUIC核心特性：

1. 连接建立延时低：传输层 0RTT 就能建立连接、加密层 0RTT 就能建立加密连接

2. 改进的拥塞控制：

   可拔插：应用程序层面就能实现不同的拥塞控制算法，不需要操作系统，不需要内核支持。即使是单个应用程序的不同连接也能支持配置不同的拥塞控制。应用程序不需要停机和升级就能实现拥塞控制的变更，我们在服务端只需要修改一下配置，reload 一下，完全不需要停止服务就能实现拥塞控制的切换。

3. 单调递增的 Packet Number：

   TCP 为了保证可靠性，使用了基于字节序号的 Sequence Number 及 Ack 来确认消息的有序到达。

   UDP使用 Packet Number 代替了 TCP 的 sequence number，并且每个 Packet Number 都严格递增，也就是说就算 Packet N 丢失了，重传的 Packet N 的 Packet Number 已经不是 N，而是一个比 N 大的值。

   但是单纯依靠严格递增的 Packet Number 肯定是无法保证数据的顺序性和可靠性。QUIC 又引入了一个 Stream Offset 的概念。

   即一个 Stream 可以经过多个 Packet 传输，Packet Number 严格递增，没有依赖。但是 Packet 里的 Payload 如果是 Stream 的话，就需要依靠 Stream 的 Offset 来保证应用数据的顺序。如错误! 未找到引用源。所示，发送端先后发送了 Pakcet N 和 Pakcet N+1，Stream 的 Offset 分别是 x 和 x+y。

   假设 Packet N 丢失了，发起重传，重传的 Packet Number 是 N+2，但是它的 Stream 的 Offset 依然是 x，这样就算 Packet N + 2 是后到的，依然可以将 Stream x 和 Stream x+y 按照顺序组织起来，交给应用程序处理。

4. 更多的 Ack 块：Quic Ack Frame 可以同时提供 256 个 Ack Block，在丢包率比较高的网络下，更多的 Sack Block 可以提升网络的恢复速度，减少重传量。

5. 基于 stream 和 connecton 级别的流量控制：

   因为 QUIC 支持多路复用，所以在 Connection 和 Stream 级别提供了两种流量控制。Stream 可以认为就是一条 HTTP 请求。Connection 可以类比一条 TCP 连接。多路复用意味着在一条 Connetion 上会同时存在多条 Stream。既需要对单个 Stream 进行控制，又需要针对所有 Stream 进行总体控制。

   原理：

   通过 window_update 帧告诉对端自己可以接收的字节数，这样发送方就不会发送超过这个数量的数据。

   通过 BlockFrame 告诉对端由于流量控制被阻塞了，无法发送数据。

   QUIC 的流量控制和 TCP 有点区别，TCP 为了保证可靠性，窗口左边沿向右滑动时的长度取决于已经确认的字节数。如果中间出现丢包，就算接收到了更大序号的 Segment，窗口也无法超过这个序列号。

   但 **QUIC 不同，就算此前有些 packet 没有接收到，它的滑动只取决于接收到的最大偏移字节数**。

6. 没有队头阻塞的多路复用：

   在一条 QUIC 连接上可以并发发送多个 HTTP 请求 (stream)。

   QUIC 一个连接上的多个 stream 之间没有依赖。这样假如 stream2 丢了一个 udp packet，也只会影响 stream2 的处理。不会影响 stream2 之前及之后的 stream 的处理。



#### 网络七层模型

- 物理层：
  物理层负责最后将信息编码成电流脉冲或其它信号用于网上传输；
  `eg：RJ45等将数据转化成0和1；`
- 数据链路层:
  数据链路层通过物理网络链路􏰁供数据传输。不同的数据链路层定义了不同的网络和协 议特征,其中包括物理编址、网络拓扑结构、错误校验、数据帧序列以及流控;
  `可以简单的理解为：规定了0和1的分包形式，确定了网络数据包的形式；`
- 网络层
  网络层负责在源和终点之间建立连接;
  `可以理解为，此处需要确定计算机的位置，怎么确定？IPv4，IPv6！`
- 传输层
  传输层向高层􏰁提供可靠的端到端的网络数据流服务。
  `可以理解为：每一个应用程序都会在网卡注册一个端口号，该层就是端口与端口的通信！常用的（TCP／IP）协议；`
- 会话层
  会话层建立、管理和终止表示层与实体之间的通信会话；
  `建立一个连接（自动的手机信息、自动的网络寻址）;`
- 表示层:
  表示层􏰁供多种功能用于应用层数据编码和转化,以确保以一个系统应用层发送的信息 可以被另一个系统应用层识别;
  `可以理解为：解决不同系统之间的通信，eg：Linux下的QQ和Windows下的QQ可以通信；`
- 应用层:
  OSI 的应用层协议包括文件的传输、访问及管理协议(FTAM) ,以及文件虚拟终端协议(VIP)和公用管理系统信息(CMIP)等;
  `规定数据的传输协议；`



#### http报文结构

HTTP报文由报文首部和报文主体构成，中间由一个空行分隔。 报文首部是客户端或服务器端需处理的请求或响应的内容及属性， 可以传递额外的重要信息。报文首部包括请求行和请求头部，报文主体主要包含应被发送的数据。通常，不一定有报文主体。



#### http请求行有哪些

请求行结构为：请求方法字段、URL字段和HTTP协议版本

请求方法包括：GET、POST、HEAD、PUT、DELETE、OPTIONS、TRACE、CONNECT

http协议版本包括： http1.1 http2 http3



#### TCP通信的过程

**建立连接、传输数据、断开连接**



### 并发

#### 说一说多线程，多进程和协程的区别

进程与线程比较

```
1) 地址空间：线程是进程内的一个执行单元，进程内至少有一个线程，它们共享进程的地址空间，
而进程有自己独立的地址空间
2) 资源拥有：进程是资源分配和拥有的单位,同一个进程内的线程共享进程的资源
3) 线程是处理器调度的基本单位,但进程不是
4) 二者均可并发执行
5) 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口，
但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制
```

协程与线程进行比较

```
1) 一个线程可以多个协程，一个进程也可以单独拥有多个协程，这样 Python 中则能使用多核 CPU。
2) 线程进程都是同步机制，而协程则是异步
3) 协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态
```

#### IO 多路复用的作用？

阻塞 I/O 只能阻塞一个 I/O 操作，而 I/O 复用模型能够阻塞多个 I/O 操作，所以才叫做多路复用。

I/O 多路复用是用于提升效率，单个进程可以同时监听多个网络连接 IO。 在 IO 密集型的系统中， 相对于线程切换的开销问题，IO 多路复用可以极大的提升系统效率。



#### 怎么来保证线程之间的安全运行

使用互斥锁来实现同步，避免资源竞争问题发生。

除了使用互斥锁可以保证线程同步外，还有其他方式可以实现同步，解决线程安全，如通过队列来实现同步，因为队列是串行的，底层封装了锁。



#### 死锁

在线程间共享多个资源的时候，如果两个线程分别占有一部分资源并且同时等待对方的资源，就会造成死锁。

为了避免死锁一直阻塞下去，可以在其中一方添加超时时间，如果超时了，就跳过。



#### 线程池中的线程的状态，状态之间的转换关系

**1、新建状态(New)**：新创建了一个线程对象。

**2、就绪状态(Runnable)**：线程对象创建后，其他线程调用了该对象的start()方法。该状态的线程位于“**可运行线程池**”中，变得可运行，只**等待获取[CPU](http://product.it168.com/list/b/0217_1.shtml)的使用权**。**即在就绪状态的进程除CPU之外，其它的运行所需资源都已全部获得。**

**3、运行状态(Running)：**就绪状态的线程获取了CPU，执行程序代码。

**4、阻塞状态(Blocked)：**阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。

阻塞分为：

1、等待阻塞 2、同步阻塞 3、其他阻塞



#### 内存泄漏与内存溢出

内存泄漏：你使用malloc或new向 内存申请了一块内存空间,但没有用free以及delete对该块内存进行释放,造成程序失去了对该块内存的控制.

内存溢出：你申请了10个字节的内存,但写入了大于10个字节的数据

内存溢出原因：
1.内存中加载的数据量过于庞大，如一次从数据库取出过多数据；
2.集合类中有对对象的引用，使用完后未清空，产生了堆积，使得JVM不能回收；
3.代码中存在死循环或循环产生过多重复的对象实体；
4.使用的第三方软件中的BUG；
5.启动参数内存值设定的过小

内存溢出的解决方案：
第一步，修改JVM启动参数，直接增加内存。(-Xms，-Xmx参数一定不要忘记加。)

第二步，检查错误日志，查看“OutOfMemory”错误前是否有其 它异常或错误。

第三步，对代码进行走查和分析，找出可能发生内存溢出的位置。



#### 堆栈溢出

以递归为例说明堆栈溢出解决方案：

方案一：

手动设置系统调用栈大小

方案二：

使用“尾递归+生成器”彻底解决堆栈溢出

尾递归是指在返回时，仅调用自身，不包含其他运算式，如加减乘除等，同时使用“yield”关键字返回生成器对象

```python
import types

def my_recursion(cur_i, tail_num=1):
    if cur_i == 1:
        yield tail_num

    yield my_recursion(cur_i-1, cur_i + tail_num)

def my_recursion_wrapper(generator, i):
    gen = generator(i)

    while isinstance(gen, types.GeneratorType):
        gen = gen.__next__()

    return gen

print(my_recursion_wrapper(my_recursion, 100000))
```



#### 核心线程与非核心线程

核心线程 ：固定线程数 可闲置 不会被销毁 ThreadPoolExecutor的allowCoreThreadTimeOut属性设置为true时，keepAliveTime同样会作用于核心线程

非核心线程数：非核心线程闲置时的超时时长，超过这个时长，非核心线程就会被回收



#### 什么是消息队列？什么场景需要他？用了会出现什么问题？

消息从某一端发出后，首先进入一个容器进行临时存储，当达到某种条件后，再由这个容器发送给另一端。 这个容器的一种具体实现就是***消息队列***。

应用在**异步处理，应用解耦，流量削锋和消息通讯**四个场景。

会出现的问题：消息**重复消费**、**消息丢失**、**消息的顺序消费**



### 数据库

#### 数据库事务的四大特性以及事务的隔离级别

四大特性（ACID）

原子性：事务包含的所有操作要么全部成功，要么全部失败回滚。

一致性：一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。

隔离性：事务之间不会相互干扰。

持久性：持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。



四种隔离级别及解决的问题

Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。

Repeatable read (可重复读)：可避免脏读、不可重复读的发生。

Read committed (读已提交)：可避免脏读的发生。

Read uncommitted (读未提交)：最低级别，任何情况都无法保证。



#### mysql的引擎，介绍下InnoDB

innodb、MyISAM、MEMORY、Archive



innodb简单介绍

innodb通过使用MVCC来获取高并发性，并且实现sql标准的4种隔离级别，同时使用一种被称成next-key locking的策略来避免换读(phantom)现象。除此之外innodb引擎还提供了插入缓存(insert buffer)、二次写(double write)、自适应哈西索引(adaptive hash index)、预读(read ahead)等高性能技术。

特点：支持外键、行锁、非锁定读(默认情况下读取不会产生锁)。

内存：由缓冲池(buffer pool),重做日志缓存(redo log buffer),额外的内存池(additional memory pool)组成。

工作方式：将数据文件按页(每页16K)读入InnoDBbuffer pool，然后按最近最少使用算法(LRU)保留缓存数据，最后通过一定频率将脏页刷新到文件。



#### MySQL索引及查询优化

索引(Index)是帮助`MySQL`高效获取数据的数据结构。类似于图书馆的目录索引结构。

B-树索引和哈希索引



#### 怎么建立索引来进行查询优化

索引创建原则

- 最左前缀匹配原则，非常重要的原则 
- 尽量选择区分度高的列作为索引 
- =和in可以乱序 
- 索引列不能参与计算，保持列“干净” 
- 尽量的扩展索引，不要新建索引 
- 使用短索引 
- 索引列排序 
- like语句操作



#### 乐观锁悲观锁

悲观锁（Pessimistic Lock），顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。

乐观锁（Optimistic Lock），顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在提交更新的时候会判断一下在此期间别人有没有去更新这个数据。乐观锁适用于读多写少的应用场景，这样可以提高吞吐量。



#### mysql联合索引，最左匹配原则，最左匹配原则只是一个约束，为什么要这样约束呢

**最左前缀匹配原则：**在MySQL建立联合索引时会遵守最左前缀匹配原则，即最左优先，在检索数据时从联合索引的最左边开始匹配。

要想理解联合索引的最左匹配原则，先来理解下索引的底层原理。索引的底层是一颗B+树，那么联合索引的底层也就是一颗B+树，只不过联合索引的B+树节点中存储的是键值。由于构建一棵B+树只能根据一个值来确定索引关系，所以数据库依赖联合索引最左的字段来构建。

优化查询效率。



#### B树和B+树的区别

B树索引

`BTree`又叫多路平衡查找树，其特性如下：

- 树中每个节点最多包含m个孩子。
- 除根节点与叶子节点外，每个节点至少有[ceil(m/2)]个孩子（ceil()为向上取整）。
- 若根节点不是叶子节点，则至少有两个孩子。
- 所有的叶子节点都在同一层。
- 每个非叶子节点由n个key与n+1个指针组成，其中[ceil(m/2)-1] <= n <= m-1 。

B+Tree索引

`B+Tree`是在`B-Tree`基础上的一种优化，使其更适合实现外存储索引结构。在B+Tree中，所有数据记录节点都是按照键值大小顺序存放在同一层的叶子节点上，而非叶子节点上只存储key值信息，这样可以大大加大每个节点存储的key值数量，降低B+Tree的高度。



B+Tree相对于B-Tree有几点不同：

非叶子节点只存储键值信息， 数据记录都存放在叶子节点中， 将上一节中的B-Tree优化，由于B+Tree的非叶子节点只存储键值信息，所以B+Tree的高度可以被压缩到特别的低。

B+Tree上通常有两个头指针，一个指向根节点，另一个指向关键字最小的叶子节点，而且所有叶子节点（即数据节点）之间是一种链式环结构。所以我们除了可以对B+Tree进行主键的范围查找和分页查找，还可以从根节点开始，进行随机查找。



#### 数据库锁有哪些

共享锁、排它锁、行锁、意向锁



#### Redis中的持久化机制

RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘。

也是默认的持久化方式，这种方式是就是将内存中数据以快照的方式写入到二进制文件中,默认的文件名为dump.rdb。

优势：

- 一旦采用该方式，那么你的整个Redis数据库将只包含一个文件，这样非常方便进行备份。比如你可能打算没1天归档一些数据。
- 方便备份，我们可以很容易的将一个一个RDB文件移动到其他的存储介质上
- RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。
- RDB 可以最大化 Redis 的性能：父进程在保存 RDB 文件时唯一要做的就是 fork 出一个子进程，然后这个子进程就会处理接下来的所有保存工作，父进程无须执行任何磁盘 I/O 操作。

劣势：

- 如果你需要尽量避免在服务器故障时丢失数据，那么 RDB 不适合你。 虽然 Redis 允许你设置不同的保存点（save point）来控制保存 RDB 文件的频率， 但是， 因为RDB 文件需要保存整个数据集的状态， 所以它并不是一个轻松的操作。 因此你可能会至少 5 分钟才保存一次 RDB 文件。 在这种情况下， 一旦发生故障停机， 你就可能会丢失好几分钟的数据。
- 每次保存 RDB 的时候，Redis 都要 fork() 出一个子进程，并由子进程来进行实际的持久化工作。 在数据集比较庞大时， fork() 可能会非常耗时，造成服务器在某某毫秒内停止处理客户端； 如果数据集非常巨大，并且 CPU 时间非常紧张的话，那么这种停止时间甚至可能会长达整整一秒。 虽然 AOF 重写也需要进行 fork() ，但无论 AOF 重写的执行间隔有多长，数据的耐久性都不会有任何损失。



AOF持久化是redis会将每一个收到的写命令都通过write函数追加到文件中(默认是 appendonly.aof)。

当redis重启时会通过重新执行文件中保存的写命令来在内存中重建整个数据库的内容。当然由于os会在内核中缓存 write做的修改，所以可能不是立即写到磁盘上。这样aof方式的持久化也还是有可能会丢失部分修改。不过我们可以通过配置文件告诉redis我们想要 通过fsync函数强制os写入到磁盘的时机。

优势：

- 使用 AOF 持久化会让 Redis 变得非常耐久（much more durable）：你可以设置不同的 fsync 策略，比如无 fsync ，每秒钟一次 fsync ，或者每次执行写入命令时 fsync 。 AOF 的默认策略为每秒钟 fsync 一次，在这种配置下，Redis 仍然可以保持良好的性能，并且就算发生故障停机，也最多只会丢失一秒钟的数据（ fsync 会在后台线程执行，所以主线程可以继续努力地处理命令请求）。
- AOF 文件是一个只进行追加操作的日志文件（append only log）， 因此对 AOF 文件的写入不需要进行 seek ， 即使日志因为某些原因而包含了未写入完整的命令（比如写入时磁盘已满，写入中途停机，等等）， redis-check-aof 工具也可以轻易地修复这种问题。
  Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。
- AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。



劣势：

- 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
- 根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 。 在一般情况下， 每秒 fsync 的性能依然非常高， 而关闭 fsync 可以让 AOF 的速度和 RDB 一样快， 即使在高负荷之下也是如此。 不过在处理巨大的写入载入时，RDB 可以提供更有保证的最大延迟时间（latency）。
- AOF 在过去曾经发生过这样的 bug ： 因为个别命令的原因，导致 AOF 文件在重新载入时，无法将数据集恢复成保存时的原样。 （举个例子，阻塞命令 BRPOPLPUSH 就曾经引起过这样的 bug 。） 测试套件里为这种情况添加了测试： 它们会自动生成随机的、复杂的数据集， 并通过重新载入这些数据来确保一切正常。 虽然这种 bug 在 AOF 文件中并不常见， 但是对比来说， RDB 几乎是不可能出现这种 bug 的。



#### Redis的集群

可以支撑多个master和多个slave，也可以支撑海量数据的存储，实现高并发与高可用。

集群中一个master挂掉以后，它对应的slave就会顶替成为新的master，

Redis集群中各节点之间可以相互进行通信。

Redis集群中各节点之间都相互知道对方的全部信息，客户端与集群进行通信的时候，只需要与集群中的某一个结点建立连接即可。

redis集群中有多台redis服务器不可避免会有服务器挂掉。redis集群服务器之间通过互相的ping-pong判断是否节点可以连接上。如果有一半以上的节点去ping一个节点的时候没有回应，集群就认为这个节点宕机了（客观下线），如果挂的是主节点，会进行主节点切换。



#### Redis为什么快

- 完全基于内存，时间复杂度为O(1)
- 数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的
- 采用单线程，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗
- 使用多路I/O复用模型，非阻塞IO
- 使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样，Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求



#### Redis6.0 之前为什么不使用多线程

使用了单线程后，可维护性高。多线程模型虽然在某些方面表现优异，但是它却引入了程序执行顺序的不确定性，带来了并发读写的一系列问题，增加了系统复杂度、同时可能存在线程切换、甚至加锁解锁、死锁造成的性能损耗。

Redis 通过 AE 事件模型以及 IO 多路复用等技术，处理性能非常高，因此没有必要使用多线程。

单线程机制使得 Redis 内部实现的复杂度大大降低，Hash 的惰性 Rehash、Lpush 等等 “线程不安全” 的命令都可以无锁进行。



#### Redis 6.0 为什么要引入多线程呢

- 提高网络 IO 性能，典型的实现比如使用 DPDK 来替代内核网络栈的方式
- 使用多线程充分利用多核，典型的实现比如 Memcached



#### redis网络模型

多路IO复用－非阻塞同步IO模型



#### redis 的zset

zset叫做有序集合。zset的每一个成员都有一个分数与之对应，并且分数是可以重复的。

有序集合的增删改由于有啦排序，执行效率就是非常快速的，即便是访问集合中间的数据也是非常高效的。