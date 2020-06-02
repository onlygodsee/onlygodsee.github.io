---
title: JS逆向学习-某DJ音乐网
date: 2020-04-30 14:23:35
tags:
- JS逆向
categories:
- 技术
- JS逆向
---

网站地址：aHR0cDovL3d3dy52dnZkai5jb20v

### 网站分析

#### 调试

随便点击一首音乐的链接，并进行抓包，发现了这样的几个请求：

![](image-20200430111802994.png)

再看下m3u8请求的响应体：

![](image-20200429185725469.png)

发现上面标出的ts流请求出现在了m3u8请求的响应体中，关于对m3u8的介绍看这里 [m3u8 文件格式详解](https://www.jianshu.com/p/e97f6555a070)

由此可以得出只要找到m3u8地址就可以获取到音频资源

#### 逆向

这是最关键的一步，查看详情页源代码会发现如下图一段代码：

![](image-20200430102253924.png)

进行断点调试

![](image-20200430102841176.png)

发现这就是目标m3u8地址

### 代码实现

#### 根据上面的逆向结果获取ts流地址

```python
import re
import os
import execjs
import aiohttp
import asyncio

async def get_indexPage(musicId):
    url = f'aHR0cDovL3d3dy52dnZkai5jb20v/play/{musicId}.html'
    ts_file = f"mp4/{musicId}/ts/"
    async with aiohttp.ClientSession() as session:
        async with session.get(url, proxy=proxy) as resp:
            if resp.status:
                page_info = await resp.text()
                arg1, arg2 = re.search("playurl=x.*?\('(.*?)'\);", page_info).group(1).split("','")
                with open('qingfeng_new.js', 'r') as f:
                    ctx = execjs.compile(f.read())

                # 解密m3u8地址
                playurl = ctx.call('get_url', arg1, arg2)

                base_url = playurl.split(f"{musicId}.m3u8")[0]

                resp_ts = await get_m3u8list(playurl)

                 # 根据m3u8文件构造获取ts流对象列表
                ts_lists = [download_ts(base_url + i, ts_file, musicId) for i in re.findall("({}.*?)\n".format(musicId), resp_ts)]

                # 并发获取ts流
                await asyncio.gather(*ts_lists)

                await save_mp4(ts_file, musicId)

async def get_m3u8list(url):
    """
    读取 m3u8 文件
    :param url:
    :return:
    """
    async with aiohttp.ClientSession() as session:
        async with session.get(url, proxy=proxy) as resp:
            if resp.status:
                resp = await resp.text()
                return resp

async def download_ts(url, ts_file, musicId):
    async with aiohttp.ClientSession() as session:
        async with session.get(url, proxy=proxy) as resp:
            if resp.status:
                num = re.search('{}-(\d\d\d?)\.ts'.format(musicId), url).group(1)
                if not os.path.exists(ts_file):
                    os.makedirs(ts_file)
                r = await resp.read()
                with open(f'{ts_file}/{num}.ts', 'wb') as f:
                    f.write(r)
```

得到ts流内容后还需要将其整合为mp4格式，如下：

```python
async def save_mp4(ts_file, musicId):
    """
    利用 ffmpeg 整合 ts流文件
    :param ts_file: 
    :param musicId: 
    :return: 
    """

    path_lists = os.listdir(ts_file)
    path_lists.sort()
    li = [os.path.join(ts_file,filename) for filename in path_lists]
    tsfiles = '|'.join(li)
    save_path = f'mp4/{musicId}/{musicId}.mp4'

    cmd = 'ffmpeg -i "concat:%s" -acodec copy -vcodec copy -absf aac_adtstoasc %s'%    (tsfiles, save_path)
    os.system(cmd)
```

然后运行该代码

```python
event_loop = asyncio.get_event_loop()
tasks = [get_indexPage(musicId) for musicId in ['193270']]
event_loop.run_until_complete(asyncio.wait(tasks))
event_loop.close()
```

调用的js文件内容：

```javascript
function DeCode() {
       this.OO0O00OO00OO = function (a, b) {
           return b > 0 ? a.substring(0, b) : null;
       }, this.OO00OO0O00O0 = function (a, b) {
           return a.length - b >= 0 && a.length >= 0 && a.length - b <= a.length ? a.substring(a.length - b, a.length) : null;
       }, this.O0000OO0OO00O0 = function (a, b) {
           var c, d, e, f, g, h, i, j, k = "";
           for (c = 0; c < b.length; c++) {
               k += b.charCodeAt(c).toString();
           }
           for (d = Math.floor(k.length / 5), e = parseInt(k.charAt(d) + k.charAt(2 * d) + k.charAt(3 * d) + k.charAt(4 * d) + k.charAt(5 * d)),
                    f = Math.round(b.length / 2), g = Math.pow(2, 31) - 1, h = parseInt(a.substring(a.length - 8, a.length), 16),
                    a = a.substring(0, a.length - 8), k += h; k.length > 10; ) {
               k = (parseInt(k.substring(0, 10)) + parseInt(k.substring(10, k.length))).toString();
           }
           for (k = (e * k + f) % g, i = "", j = "", c = 0; c < a.length; c += 2) {
               i = parseInt(parseInt(a.substring(c, c + 2), 16) ^ Math.floor(255 * (k / g))), j += String.fromCharCode(i),
                   k = (e * k + f) % g;
           }
           return j;
       }, this.O0000OO0OO00O = function (a, b, c) {
           return a.length >= 0 ? a.substr(b, c) : null;
       }, this.O0O000000O0O0 = function (a) {
           return a.length;
       }, this.O000O0OO0O0OO = function (a,b) {
           var h, i, j, k, l, m, n, o, p, c = b, d = this.O0O000000O0O0(c), e = d, f = new Array(), g = new Array();
           for (l = 1; d >= l; l++) {
               f[l] = this.O0000OO0OO00O(c, l - 1, 1).charCodeAt(0), g[e] = f[l], e -= 1;
           }
           for (h = "", i = a, m = this.OO0O00OO00OO(i, 2), i = this.OO00OO0O00O0(i, this.O0O000000O0O0(i) - 2),
                    l = 0; l < this.O0O000000O0O0(i); l += 4) {
               j = this.O0000OO0OO00O(i, l, 4), "" != j && (b = this.OO0O00OO00OO(j, 1), k = (parseInt(this.OO00OO0O00O0(j, 3)) - 100) / 3,
                   m == this.O0000OO0OO00O0("a9ab044c634a", "O0000OO0OO00O") ? (n = 2 * parseInt(b.charCodeAt(0)),
                       o = parseInt(f[k]), p = n - o, h += String.fromCharCode(p)) : (n = 2 * parseInt(b.charCodeAt(0)),
                       o = parseInt(g[k]), p = n - o, h += String.fromCharCode(p)));
           }
           return h;
       };
   }

   var x=new DeCode();

function get_url(a, b) {
    return x.O000O0OO0O0OO(a, b)
}
```