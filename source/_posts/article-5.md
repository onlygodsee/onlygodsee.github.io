---
title: JS逆向学习-淘**
date: 2020-05-11 11:56:49
tags:
- JS逆向
categories:
- 技术
- JS逆向
---

懂得都懂：aHR0cHM6Ly90YW9kYXhpYW5nLmNvbS9jcmVkaXQy

<!-- more -->

### 网站分析

#### 调试

打开网站按F12开始调试，发现该网站有无限debugger：

![](WX20200511-121718.png)

在对应行右键选择 “Never pause here”, 然后点击屏幕上的箭头便可绕过

然后查看请求，在XHR中发现了目标信息和加密参数：

![](WX20200511-122949.png)

#### 定位加密位置

通过搜索关键字“sign”可以发现这几个位置：

![](WX20200511-123230.png)

再搜索 ***_0x5b56f8*** 便可定位到加密位置，打上断点进行调试：

![](WX20200511-123557.png)

![](WX20200511-124307.png)

*由此可知最终的加密操作是在 **case 0** 中完成的，通过断点调试会发现是对* ***一个特殊字符串*** 进行了md5加密，接下来我们分析这个字符串是怎么得出的

在 ***case 5*** 中同样对 ***_0x5b56f8*** 进行了操作，并且确定这里就是生成被加密字符串的位置

![](WX20200511-140037.png)

在分析这段代可知是对 ***_0x8c9894*** 和 ***_0x4ea2ce***进行了一些列算数操作，接下来我们在分析这两个参数

分析 ***case 3*** 中发现 ***_0x4ea2ce*** 是固定值

![](WX20200511-140550.png)

分析 ***case 2*** 可知这里又做了一个md5加密，被加密字符串的组成结构是 account +  ***_0x4ea2ce*** + account + type值

![](WX20200511-140843.png)

至此我们已经找到获取加密参数的所有元素

### 逆向

结合上面的逻辑，我们开始进行逆向测试

首先获取 ***_0x8c9894*** 的值

```python
from hashlib import md5

_0x4ea2ce= '7176a337dffebf0ff2d30d65fda5af78'

def md5value(s):
    a = md5(s.encode()).hexdigest()
    return a

s = '%s%s%s0' %('这个帐号只是传说', _0x4ea2ce, '这个帐号只是传说')

_0x8c9894 = md5value(s) + ' '
print(_0x8c9894)
>>> f81765c208bcc1a6892863af77bb4fae 
```

这里要注意 **最后的结果还要加一个空格** 应该是 ***“f81765c208bcc1a6892863af77bb4fae ”***

接下来获取 ***_0x5b56f8*** 的值

```python
# 方法映射
_0x36294a = {
    "VIRpO": lambda a, b: a < b,
    "PRedm": lambda a, b: a % b,
    "TTSVu": lambda a, b: a + b,
    "rOuyL": lambda a, b: a % b,
    "YJYZC": lambda a, b: a * b,
    "LrwDB": lambda a, b: a * b,
}

_0x5b56f8 = ''

for _0x1af38d in range(32):
    _0x5b56f8 += str(_0x36294a["PRedm"](_0x36294a["TTSVu"](_0x36294a["TTSVu"](_0x36294a["TTSVu"](ord(_0x4ea2ce[_0x1af38d]), _0x36294a["rOuyL"](_0x36294a["YJYZC"](ord(_0x4ea2ce[_0x1af38d]), ord(_0x4ea2ce[_0x1af38d])), 0x20)), ord(_0x8c9894[_0x1af38d])), _0x36294a["LrwDB"](_0x1af38d, _0x1af38d)), 0x9))

print(_0x5b56f8)
>>>38856360307430874440750784857572
```

这里看到已经成功获取了md5加密前的字符串，然后我们对其加密并进行比对

![](WX20200511-151435.png)

发现与前面目标请求的 ***sign*** 相同，至此已经成功获取了加密参数

### 代码实现

```python
import aiohttp
import asyncio
from hashlib import md5

url = 'aHR0cHM6Ly90YW9kYXhpYW5nLmNvbS9jcmVkaXQy'

# 固定加密字符串
_0x4ea2ce= '7176a337dffebf0ff2d30d65fda5af78'

# sign 加密要用到的方法映射（模拟网站JS代码）
_0x36294a = {
    "VIRpO": lambda a, b: a < b,
    "PRedm": lambda a, b: a % b,
    "TTSVu": lambda a, b: a + b,
    "rOuyL": lambda a, b: a % b,
    "YJYZC": lambda a, b: a * b,
    "LrwDB": lambda a, b: a * b,
}

def get_md5(s):
    """
    md5 加密
    :param s:
    :return:
    """
    result = md5(s.encode()).hexdigest()
    return result

def set_data(account, typ):
    """
    构造请求参数
    :param account:
    :param typ:
    :return:
    """
    # 设置 _0x8c9894 加密规则
    s = f"{account}{_0x4ea2ce}{account}{typ}"

    _0x8c9894 = get_md5(s) + ' '

    _0x5b56f8 = ''

    # 获取 sign 值
    for _0x1af38d in range(32):
        _0x5b56f8 += str(_0x36294a["PRedm"](_0x36294a["TTSVu"](_0x36294a["TTSVu"](
            _0x36294a["TTSVu"](ord(_0x4ea2ce[_0x1af38d]), _0x36294a["rOuyL"](
                _0x36294a["YJYZC"](ord(_0x4ea2ce[_0x1af38d]), ord(_0x4ea2ce[_0x1af38d])), 0x20)),
            ord(_0x8c9894[_0x1af38d])), _0x36294a["LrwDB"](_0x1af38d, _0x1af38d)), 0x9))

    output = {
        "account": account,
        "type": "0",
        "sign": _0x5b56f8
    }

    return output

async def get_info(url, data):
    # ssl证书设置
    conn = aiohttp.TCPConnector(ssl=False)
    async with aiohttp.ClientSession(connector=conn) as session:
        async with session.post(url, data=data, proxy=proxy) as resp:
            if resp.status:
                info = await resp.json(content_type='text/html',encoding='utf-8')
                return info

if __name__ == '__main__':
    accounts = [account1, account2]
    event_loop = asyncio.get_event_loop()
    tasks = [get_info(url, set_data(account, '0')) for account in accounts]
    event_loop.run_until_complete(asyncio.wait(tasks))
    event_loop.close()
```



### 总结

该网站的逆向并不难，通过全局搜索关键字就可以定位到加密位置，关键在于绕过无限Debugger和对混淆代码的分析