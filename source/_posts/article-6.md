---
title: JS逆向学习-某厂藏某阁
date: 2020-05-12 09:49:58
tags:
- JS逆向
categories:
- 技术
- JS逆向
---

首先感谢咸鱼大佬的分享 [文章地址](https://mp.weixin.qq.com/s/5pp1vd00O-JHeAf6loaYfg)

懂得都懂：aHR0cHM6Ly9kd3ouY24vS0VGT01qOGg=

<!-- more -->

### 网站分析

#### 调试

![](WX20200512-095031.png)

通过查看html页面发现目标信息在id为 ***equip_desc_panel*** 的标签下，然后全局搜索这个id

![](WX20200512-095918.png)

发现这里是空的，但发现了另外一个有意思的点，下面 ***textarea*** 标签中的内容像极了加密内容，然后再查看其他位置

![](WX20200511-185128.png)

柳暗花明又一村，继续查找 ***get_equip_desc***

![](WX20200511-185217.png)

看来是在 ***decode_desc*** 中执行的加密，但是这个函数就搜不到了，然后在这里打断点

![](WX20200511-190108.png)

成功断点并跟进调试后发现是通过这样的方式设置了 ***decode_desc*** 函数，并且确认这里就是解密的位置

#### 逆向

然后就到了扣代码的环节

通过前面的断点调试得知只需要 ***function g()*** 的流程，通过node.js运行发现运行结果不符合预期，那就开始补坑

![](WX20200511-191211.png)



![](WX20200511-191030.png)



![](WX20200511-191007.png)



![](WX20200511-191101.png)



![](WX20200511-191116.png)

上面截图中标注的内容都是需要去替换的，但要注意的是 **window.atob** 在 **node** 中不可用，要这样写：

```javascript
Buffer.from(_0x1c0cdf,"base64").toString()
```



### 测试

修改后的js代码如下：

```javascript
var _0x3a8e = function(_0xc40c11, _0x32bbb2) {
    _0xc40c11 = _0xc40c11 - 0x0;
    var _0x4e269a = _0x3012[_0xc40c11];
    return _0x4e269a;
};

var _0x3012 = ['\x73\x75\x62\x73\x74\x72\x69\x6e\x67', '\x61\x74\x6f\x62', '\x63\x68\x61\x72\x43\x6f\x64\x65\x41\x74', '\x70\x75\x73\x68', '\x74\x65\x73\x74'];
(function(_0x3ed35c, _0x48b8fe) {
    var _0x1ad9d9 = function(_0x8eeda7) {
        while (--_0x8eeda7) {
            _0x3ed35c['push'](_0x3ed35c['shift']());
        }
    };
    _0x1ad9d9(++_0x48b8fe);
}(_0x3012, 0x153));

function g(_0x1c0cdf) {
    if (_0x1c0cdf = _0x1c0cdf['\x72\x65\x70\x6c\x61\x63\x65'](/^\s+|\s+$/g, ''),
        !/^@[\s\S]*@$/[_0x3a8e('0x0')](_0x1c0cdf))
        return _0x1c0cdf;
    var _0x36ab38 = '';
    if (_0x1c0cdf = _0x1c0cdf['\x72\x65\x70\x6c\x61\x63\x65'](/^@|@$/g, ''),
        /^[^@]+@[\s\S]+/['\x74\x65\x73\x74'](_0x1c0cdf)) {
        var _0x33c80e = _0x1c0cdf['\x69\x6e\x64\x65\x78\x4f\x66']('\x40');
        _0x36ab38 = _0x1c0cdf[_0x3a8e('0x1')](0x0, _0x33c80e),
            _0x1c0cdf = _0x1c0cdf['\x73\x75\x62\x73\x74\x72\x69\x6e\x67'](_0x33c80e + 0x1);
    }
    var _0x1b3f48 = function s(_0x1c0cdf) {
        try {
            return eval('\x28' + _0x1c0cdf + '\x29');
        } catch (_0x40b9c3) {
            return null;
        }
    }(_0x1c0cdf = Buffer.from(_0x1c0cdf,"base64").toString());
    _0x1b3f48 && '\x6f\x62\x6a\x65\x63\x74' == typeof _0x1b3f48 && _0x1b3f48['\x64'] && (_0x1b3f48 = _0x1b3f48['\x64']);
    for (var _0x20b9fa = [], _0x10503c = 0x0, _0x1a524d = 0x0; _0x1a524d < _0x1b3f48['\x6c\x65\x6e\x67\x74\x68']; _0x1a524d++) {
        var _0x3641ed = _0x1b3f48['\x63\x68\x61\x72\x43\x6f\x64\x65\x41\x74'](_0x1a524d)
            , _0x341952 = _0x36ab38[_0x3a8e('0x3')](_0x10503c % _0x36ab38['\x6c\x65\x6e\x67\x74\x68']);
        _0x10503c += 0x1,
            _0x3641ed = 0x1 * _0x3641ed ^ _0x341952,
            _0x20b9fa[_0x3a8e('0x4')](_0x3641ed['\x74\x6f\x53\x74\x72\x69\x6e\x67'](0x2));
    }
    return function d(_0x1c0cdf) {
        for (var _0x36ab38 = [], _0x33c80e = 0x0; _0x33c80e < _0x1c0cdf['\x6c\x65\x6e\x67\x74\x68']; _0x33c80e++)
            // _0x36ab38['\x70\x75\x73\x68'](_0xcbc80b['\x53\x74\x72\x69\x6e\x67']['\x66\x72\x6f\x6d\x43\x68\x61\x72\x43\x6f\x64\x65'](_0xcbc80b['\x70\x61\x72\x73\x65\x49\x6e\x74'](_0x1c0cdf[_0x33c80e], 0x2)));
            _0x36ab38['\x70\x75\x73\x68'](String.fromCharCode(parseInt(_0x1c0cdf[_0x33c80e], 0x2)));
        return _0x36ab38['\x6a\x6f\x69\x6e']('');
    }(_0x20b9fa);
}

var a = "@ggRp9MYui9tDX6Fq@eyJkIjogIkRcdTAwMTVcdTdiMWJcdTdlZDdcdTAwMTl8a0VJXHUwMDE5XHU0ZWUwXHU4ODA4eFx1NmMwMmVcdTAwMDNEXHUwMDE1XHU0Zjc2XHU1YmMzXHUwMDE5ZmpMX1x1MDAxOVx1NTQwOVx1NGU2OXhcdTAwMWRwSVdEIFx1ODA2MFx1NGU3Y1x1NWVlYnlHWVx1MDAwZVRkXHU0ZmI2XHU3NDMwXHU1OTc3XHU4ZDU0R1ZcdTZiNzNTS1x1OTU3Nlx1NzBlNVx1N2IzY1x1N2VjZVx1MDAxOUV1eFx1MDAxNlx1OTUzMFx1NWQzZFx1NWJmYVx1Nzc5NHJcdTU5NWFcdTk2MGFcdTc3YmVcdTMwNThVXHU3ZWNiXHU3M2EyXHU3NDJkZ1x1MDAwMVx1MDAxNTRSXHUwMDA0U1x1MDAxNjJ4XHUwMDBibVx1NzIwY1x1NjUyMVx1ZmYyM1cnbHJcdTAwMDQwIVNcdTY1YjJcdTdlZDdcdTUyMTJcdTk2MWRcdTUyNmZWMFx1MDAxYVx1MDAwNmdcdTAwMWZcdTVmMzZcdThmOTZcdTViMjVcdTY1MTdcdWZmN2RmXHU1YjI0XHUwMDE2eVx1NWIwZFVBXHU1M2Y1QFx1NWIxMHFcdTAwMTVcdTAwMDFSXHUwMDE1XHU3YjQxXHU3N2ExSlx1MDAxOVx1NGYxZVx1OGQ3MVVCXGJUXHU0ZjYwXHU1YmViXHUwMDE2bUBJUnFcdTAwMWVcdTAwMWFcbnpcdTAwMDdcdTdiNGZcdTc3Y2FOZFx1NTJjM1x1OTFmOWZaVkdcdTZjNDZcdTg4MzBcdTAwMTlmaEVKV1dcdTAwMDN7RFx1N2I2MFx1Nzc4Ml1HXHU1MmM5XHU5MWJmXHUwMDE5ZmhVXHU2YzdkXHU4ODc5VG9pXHUwMDA2ZVx1MDAxZkQgcVx1MDAwMlx1N2IxZlx1NzdiZWNVXHU2YzdkXHU4ODc5VG9pXHUwMDAzZlx1OTA2ZVx1NWVjMUd5QVx1MDAxN3h6XHUwMDFiSn5XNlx1NjY0N1x1NGY3Ylx1ZmY1Y1x1OTA2ZVx1NWVjMUd5Qlx1MDAxYSN6MkpLXHU2NjZiXHU3NmJjXHU0ZWNhXHU1NDNlXHVmZjVjXHU0ZjIyXHU4ZDRmR3lCXHUwMDFhP3pcdTAwMTYsfEx2XHUwMDFkc1x1N2I2MFx1Nzc4Mlx1N2VhM1x1NTQ2ZmhQXHU1OWJiXHU2MTQyXHU5MTg4XHU3YmY4XHU3YjRmXHU3N2NhVzZcdTk1YjBcdTZkMDhcdTY3MjdcdTRlODdcdWZmN2RcdTgyZDZcdTY3Y2VcdTVjMDFcdTAwMTluK1x1OTA5ZFx1NGYyNFx1Njc1OFx1NGU4Mlx1ZmY1ZVx1NmIzZVx1NTY1ZWZSXHUwMDE1XHU1OGY5XHU1MmYyXHU5NTk4XHU2ZDA3XHU2MmNkXHU4MGE0XHU1OWY3XHU2MTY2XHU5MWU4XHU3YmY5XHU3YjBkXHU3ZWZmXHUwMDAyXHU3ZWUxUj5EIFNuXHU1MjdiXHU5MDc5XHU4MDcwXHVmZjczXHUzMDY5XHU2MzZlXHU2MDgxXHU4ZDQ1XHU4YmNjXHUwMGY2Uj5HciJ9@";

console.log(g(a))
```

运行这个文件查看结果

![](WX20200512-102800.png)

我们得到了想要的一切



### 总结

这次逆向的重点是修改原来的js内容，其中有很多坑，比如base64实现的差异以及对混淆的处理，需要耐心调试才行。