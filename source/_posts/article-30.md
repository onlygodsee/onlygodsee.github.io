---
title: article-30
date: 2020-07-30 11:00:50
tags:
---

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

1.引用计数机制 2.标记-清除 3.分代回收



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

