---
title: JS逆向学习-某某猫企业信息查询参数分析
date: 2020-06-10 15:18:17
tags:
- JS逆向
categories:
- 技术
- JS逆向
---

aHR0cHM6Ly93d3cucWljaGFtYW8uY29tLw==



### 分析请求

![](WX20200610-153511.png)

这个就是结果的响应页面，其中只有一个参数 ***mfccode*** ,全局搜索定位到了这里

![](WX20200610-153913.png)

在这里打上断点，继续调试

![](WX20200610-154318.png)

发现成功断上了，然后继续跟进调试

![](WX20200610-154503.png)

***window.__qzmcf*** 是 ***dc*** 函数，另外是否发现标注的这个url很眼熟，我们回头看下第一张图片

![](WX20200610-154839.png)

chrome无法显示内容，新建窗口打开看下

![](WX20200610-155001.png)

格式化后查看

```javascript
(function (w) {
    var s = function (a) {
            if (!a) {
                return '';
            }
            var al = a.length,
                ret = new Array(al),
                i = 0,
                j = i;
            while (al--) {
                ret[j++] = String.fromCharCode(a[i++])
            }
            return ret.join('');
        },
        ex = function (x, y) {
            for (n in y) {
                x[n] = y[n]
            }
        },
        ck = function (sn) {
            var ac = w.document[s([99, 111, 111, 107, 105, 101])].split('; ');
            for (var i = 0; i < ac.length; i++) {
                var aCrumb = ac[i].split('=');
                if (sn == aCrumb[0]) {
                    if (aCrumb[1] != null && aCrumb[1] != typeof undefined) {
                        return unescape(aCrumb[1]);
                    };
                    return '';
                }
            };
            return typeof undefined;
        },
        le = s([108, 101, 110, 103, 116, 104]),
        pc = s([99, 104, 97, 114, 67, 111, 100, 101, 65, 116]),
        id = ck(s([113, 122, 110, 101, 119, 115, 105, 116, 101, 46, 117, 105, 100])),
        mov = function () {
            var dyt1 = function (d5, y4) {
                    return d5 >> y4
                },
                kac5 = function (k7, a4) {
                    return k7 << a4
                },
                vau2 = function (v8, a6) {
                    return v8 - a6
                },
                fwc3 = function (f4, w0) {
                    return f4 + w0
                },
                ymx2 = function (y1, m8) {
                    return y1 | m8
                },
                kdf8 = function (k0, d0) {
                    return k0 & d0
                },
                moh1 = function (m4, o2) {
                    return m4 * o2
                },
                __x = 178;
            return kac5(fwc3(moh1(vau2(kac5(fwc3(ymx2(kac5(kdf8(vau2(vau2(kac5(kac5(fwc3(fwc3(kac5(kdf8(dyt1(kdf8(kac5(fwc3(kac5(moh1(dyt1(moh1(dyt1(fwc3(kac5(dyt1(fwc3(vau2(vau2(kac5(dyt1(kdf8(moh1(kdf8(moh1(kac5(moh1(dyt1(kac5(dyt1(kdf8(kdf8(fwc3(dyt1(ymx2(kdf8(ymx2(moh1(dyt1(kdf8(kac5(kac5(kac5(ymx2(kac5(kac5(kac5(kdf8(vau2(ymx2(kac5(kac5(dyt1(kac5(fwc3(fwc3(vau2(ymx2(kdf8(ymx2(kac5(kac5(vau2(dyt1(dyt1(kdf8(fwc3(moh1(ymx2(moh1(vau2(dyt1(moh1(vau2(moh1(kac5(fwc3(dyt1(vau2(kac5(ymx2(ymx2(ymx2(kac5(kdf8(moh1(moh1(dyt1(fwc3(fwc3(kdf8(dyt1(dyt1(kdf8(kac5(ymx2(kac5(ymx2(moh1(kac5(vau2(moh1(kac5(dyt1(kac5(fwc3(dyt1(dyt1(vau2(vau2(fwc3(kac5(moh1(fwc3(ymx2(kdf8(dyt1(kac5(dyt1(ymx2(dyt1(fwc3(dyt1(kdf8(dyt1(dyt1(kdf8(dyt1(kac5(fwc3(ymx2(kdf8(dyt1(kac5(vau2(ymx2(kac5(kdf8(vau2(vau2(kac5(dyt1(vau2(kdf8(ymx2(vau2(dyt1(ymx2(ymx2(dyt1(kac5(vau2(ymx2(fwc3(dyt1(fwc3(kdf8(fwc3(kac5(ymx2(kac5(vau2(moh1(dyt1(ymx2(moh1(vau2(dyt1(fwc3(kdf8(kac5(kac5(kdf8(ymx2(kac5(ymx2(dyt1(kdf8(kac5(moh1(dyt1(kdf8(vau2(ymx2(dyt1(ymx2(moh1(ymx2(kac5(dyt1(kdf8(ymx2(vau2(fwc3(kac5(kdf8(moh1(moh1(dyt1(dyt1(moh1(moh1(kdf8(dyt1(ymx2(kdf8(moh1(vau2(vau2(ymx2(vau2(kdf8(kac5(moh1(kdf8(kac5(kdf8(dyt1(kac5(ymx2(ymx2(dyt1(dyt1(fwc3(vau2(vau2(moh1(kdf8(dyt1(kdf8(kdf8(fwc3(vau2(kac5(ymx2(vau2(fwc3(moh1(fwc3(moh1(moh1(ymx2(kac5(kac5(dyt1(kac5(kac5(dyt1(dyt1(kac5(kac5(dyt1(ymx2(dyt1(dyt1(ymx2(moh1(vau2(moh1(ymx2(ymx2(fwc3(vau2(fwc3(moh1(dyt1(fwc3(kac5(moh1(kac5(vau2(moh1(kac5(kac5(kac5(moh1(vau2(vau2(vau2(dyt1(ymx2(kac5(moh1(vau2(ymx2(dyt1(ymx2(moh1(dyt1(ymx2(kdf8(vau2(kdf8(kdf8(fwc3(kdf8(dyt1(moh1(kdf8(moh1(fwc3(vau2(dyt1(dyt1(ymx2(moh1(moh1(moh1(kdf8(moh1(fwc3(kac5(fwc3(ymx2(moh1(moh1(kac5(dyt1(vau2(vau2(kac5(ymx2(moh1(dyt1(fwc3(kdf8(moh1(ymx2(kdf8(ymx2(kac5(kdf8(ymx2(moh1(moh1(kac5(ymx2(vau2(moh1(vau2(kdf8(moh1(moh1(vau2(vau2(kdf8(ymx2(dyt1(dyt1(dyt1(ymx2(fwc3(dyt1(vau2(dyt1(fwc3(moh1(ymx2(dyt1(fwc3(fwc3(kdf8(fwc3(vau2(fwc3(dyt1(ymx2(fwc3(moh1(moh1(ymx2(ymx2(dyt1(vau2(kdf8(vau2(kdf8(kac5(dyt1(dyt1(ymx2(dyt1(ymx2(kac5(fwc3(dyt1(kac5(vau2(vau2(fwc3(vau2(kdf8(kdf8(ymx2(kac5(kdf8(vau2(ymx2(kdf8(vau2(kac5(ymx2(vau2(vau2(vau2(kdf8(dyt1(kdf8(kdf8(moh1(vau2(ymx2(fwc3(moh1(fwc3(vau2(ymx2(vau2(fwc3(moh1(fwc3(kdf8(moh1(vau2(moh1(dyt1(kdf8(vau2(dyt1(kac5(moh1(fwc3(kdf8(vau2(kdf8(fwc3(kac5(ymx2(kac5(dyt1(kdf8(moh1(ymx2(dyt1(kac5(ymx2(moh1(dyt1(vau2(vau2(vau2(kdf8(moh1(fwc3(ymx2(kdf8(moh1(vau2(vau2(kac5(moh1(ymx2(kac5(dyt1(moh1(fwc3(kac5(ymx2(vau2(dyt1(fwc3(vau2(kdf8(fwc3(dyt1(fwc3(dyt1(ymx2(kdf8(moh1(vau2(ymx2(dyt1(fwc3(ymx2(moh1(kdf8(ymx2(vau2(ymx2(dyt1(dyt1(vau2(kdf8(kac5(ymx2(kac5(kdf8(fwc3(__x, 1406), 21212), 1), 1185), 2), 42417), 1564), 3), 2), 1683), 1406), 3335), 21459), 3), 3846), 598), 4), 882), -102), 1), 55540), 2275), 2), 153), 2), -3991), 61019), -258), -3883), 1), 3411), 1614), 1), -1238), 2), 4), 3), 1895), 1), 2), -643), -2185), 1), 42090), 1191), -2914), 4), 58000), -1195), -1979), 2367), 1), 1), 710), 4), 1), 951), 3), 27051), 1), 2), 3079), 2), 948), 47143), 898), 10855), -1182), 1), 3), 3), 2618), 4782), 3), 1), 2922), 1), 53151), 2494), 3), 4017), 1211), 3693), 389), 592), 3), 2065), 3200), 3039), 2), 21493), 49244), 2), 51499), -1454), -3464), 1221), 2217), 3), 2815), 61805), 1372), -156), 10772), 1), 1128), 39679), 53694), 328), -1620), -282), 1944), 4), 2), -2659), 3), 842), 4), 2030), 1), 3), 1), 5956), 690), 55923), 2700), 4), 1440), 3897), 2), 2), -2604), 407), 3), 3520), 2257), -3658), 35189), -3268), -1766), 4), 1245), 3), 2606), 1), 1221), 1), -2134), 1145), 3), 3), 4), 2166), 32254), -2529), 440), 3), 1), 50152), -3151), 3), 2917), 2408), 1), 2), 4), 1589), 59244), 3), 1810), 40854), 1269), 3), 52930), -3553), 4), 2), 3255), 3), -3498), 1891), 3), 2), 3), 1), 372), 3071), 2), 3572), 1), 34481), 4), 4), 1), 282), 3), 3), -3580), -3905), 3), 13000), 4), 2), 8826), -1575), 33443), 15121), 1749), 41336), 1515), 3), 4), 3580), 3), 1248), -3954), 2), 3), 4076), 2), 2384), -1252), -4046), 2), 2), 2), 3), 4), 4054), 3), 1), 4), 1826), 2), 3), 2031), -2693), 2272), 2468), 2450), 1), -2497), 4), 1686), 2), 3), 3539), 1), 2), 3), 3), 4), 3), 1), 1), 3), 4), 3016), 1), 1), -3156), 3), 18), -1557), 432), 1), -3388), -2011), 34081), 26569), 2), 21141), 4), 1925), 2762), -2549), 1), 2), 3628), 3597), 1), 2), 14065), 2), 32077), 2), 3), 39399), 1187), 1204), -544), 2278), 1), 13726), 2747), 4), 62605), 2), 2), 4), 4), 3), 3), 64237), 4), -3653), -392), 3980), 52493), 3), 1), 804), 1), 1901), 2), 3674), 3154), 22167), 2), 1), 4), 46515), 3), 1549), 4), 755), 59834), 1), 4), 58435), -989), 1), -2055), 2), 1473), 2), 2), 1029), 4), 1786), 2), 385), 36470), -2102), 3), 1551), 4048), -4024), 4), 2), 1481), 347), 1), -95), 3041), 22582), -894), 3), 4), 1858), 1310), 5151), 4), 3819), -2540), 3), 2), 5996), 500), 85), 3), 4), 47445), 2), 2), 31521), 3), 3587), 3), 3026), 2), 3), 1), 52848), 2189), -2227), 4), 3), -4016), 861), -852), 3), 4), 2789), 4), 4), 2), 1), -3959), 3), 3), 3902), 2), 3190), 2), 34242), 1), 1), 43598), 808), 3359), 3), 1), 3), 27918), 1), 2346), 2327), 2272), 4), -1725), 4), -3771), 1), 4), 812), 4), 1), 3909), 1), 425), 4), 1680), 62779), 4), 2), -3592), 4), 3), 3840), 43141), 3825), 2051), 3021), 864), 3), 2), 1), 3), 1578), -222), 62225), 4), 4), 3), 2247), 1), 4), 1), 28192), 4), 4), 3034), 4560), 792), 3), -1362), 22378), 49452), 1), 1), 2), 4), 3), 2), 20282), 2), 16828), 4), 3), -1414), -1766), 3703), 4), 1), 996), 2), 2), 3), 4), 1), -1706), 2), 52552), 3), 60561), 4), -3376), -2784), 4), 2), 2875), -2245), 29289), 2), 487), 347), 4), 3513), 4), 1463), 4);
        },
        sk = [-34, -82, -80, 79, -23, 100, 77, -39, 76, -45, 89, 93, 0, -59, 85, -35, 65, 120, -89, 110, 83, -12, -7, -42, 63, 52, 120, -72, 41, -52, 103, -16, -18, -17, 70, -76, -101, -17, -114, -36, 91, 66, 24, 76, -68, 123, 85, 94, 61, -28, 46, -76, 16, 31, 100, -37, 70, 130, 124, 114, -12, -63, -69, -106, -92, -42, 14, 29, -68, 72, 127, -32, 62, 16, -34, 15, -48, -58, 91, -115, 54, 11, 123, 68, 58, 85, 65, -41, 44, -61, 91, -90, 33, -98, 48, 60, 73, 106, -93, -86, -7, 61, 88, -44, 76, 2, -56, 116, 28, 149, -6, 110, 110, 142, -14, -54, -108, 122, 1, -44, 32, 119, 28, 0, -63, 52, -63, -95, 121, -104, -32, -64, -93, 41, 10, -34, 77, -50, 151, -81, 44, -76, -57, 91, 37, 23, 47, -106, 7, -1, -16, -43, 31, 68, 12, -41, 79, -11, -97, -76, 87, 59, 50, -55, 3, 114, -100, -37, 93, 16, 19, -62, 108, 101, 16, 106, 106, 69, -2, -86, -108, 4, 101, -32, 59, 127, -62, 25, -23, 104, -96, 93, -8, 133, 145, -43, 83, 17, 64, -39, 43, 2, -64, 40, 64, 9, -40, -17, 107, -83, 22, 55, 77, 110, 29, -44, 87, -16, 55, -42, 28, 87, -31, 14, 63, 14, -39, -1, 41, -86, -83, -38, 40, 52, 135, 23, -86, 127, 111, -107, 112, 85, 122, 145, 118, 132, -9, -37, 47, -106, 92, -77, 137, 13, 22, 5, 60, 74, -100, 107, 56, 37, 67, -31, 94, -21, -66, -14, -45, 114, -72, 56, 54, 34, 72, -44, 118, 131, -96, -46, 36, 114, 0, 86, -45, 113, 138, 20, 93, 25, -73, -30, -26, -48, 97, -35, 73, 11, -55, -94, 11, 135, 104, 91, -96, -84, 137, 142, -65, 66, -115, -35, 40, -75, -8, 24, 39, -39, 69, 40, 29, 25, 32, -19, 114, 119, -16, -43, 52, 85, 112, 50, 52, 87, 106, 17, 96, 64, -10, 100, 21, 84, -93, -35, 85, 8, -2, -27, 19, 144, 62, -29, 85, 64, -39, -74, 101, 121, 108, -31, 70, -73, 109, 111, 111, 109, 63, 35, 105, 29, -25, 84, 89, -21, -20, -46, 77, -67, -23, 35, -11, -80, -82, -82, 10, -70, -108, 0, -46, 145, -23, -33, 73, -94, -60, -25, 118, -42, 76, -65, 98, -17, 93, 98, 40, 130, 114, -41, 49, 17, 4, -38, 0, -58, -48, 132, 57, 0, 96, -38, 122, -89, 51, -45, 60, -11, -63, -75, 74, 13, 128, -9, 26, -34, -71, 38, 81, -108, 135, -46, 60, 63, -37, -13, -43, -9, 91, -11, -74, 83, 47, 36, 104, -81, 92, -35, 41, 91, 112, -104, -45, -46, -106, -65, 68, 70, -66, -79, -57, 134, -115, -43, 41, -111, 118, -90, -56, -34, 133, -104, 141, 42, 6, 96, 36, 56, 123, -31, 77, 80, 37, 8, 59, -54, -1, 104, 126, -12, 113, -14, -97, -107, -18, -46, 81, 55, 153, -84, 19, 100, -104, 124, 19],
        m = function (a, k, i) {
            return (a + k[pc](k[le] % i))
        },
        dc = function () {
            var sl = sk[le] - 1,
                av = sk[sl],
                i = 0,
                j = i,
                ret = i,
                a, b;
            while (sl--) {
                i = j;
                b = m(sk[j++], id, (j > 0 && j % 0x10 == 0) ? 0x10 : j % 0x10);
                if (i % 2 == 0) {
                    ret -= (b * (i % 3) - av)
                } else {
                    ret += (b + 87)
                }
            }
            return ret | mov(1)
        };
    w[s([95, 95, 113, 122, 109, 99, 102])] = dc
})(window);
```

![](WX20200610-160618.png)

现在已经确定这就是加密的代码块了，扣下来在webstorm中执行老样子缺什么补什么，最后发现了这样一个错误

![](WX20200610-160918@2x.png)

提示我们 ***cookie*** 没有定义，在控制台看下

![](WX20200610-161200.png)

再结合代码得知这里是将 ***qznewsite.uid*** 做了处理

所以我们在js中增加如下定义，再执行代码就发现成功了

```javascript
var window = {document: {cookie: "qznewsite.uid=nzaxo1a4zrhqc34sn214bbyz"}}
```

### 逆向

前面已经修改好了js代码，然后使用python调用并请求后发现会跳到登陆页，这是因为该js是动态返回的，对比下两次得到的js

![](WX20200610-161952.png)

所以就需要动态获取了，上代码

```python
import time
import random
import execjs
import requests
from urllib.parse import quote

headers = {}
headers["User-Agent"] = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.61 Safari/537.36"
headers["Referer"] = "https://www.qichamao.com/"

js_url = f"https://www.qichamao.com/home/GetJsVerfyCode?t={random.random()}&_={int(time.time())}"

js_result = requests.get(js_url, verify=False, proxies=proxies, headers=headers)

# 获取
uid = js_result.headers.get("Set-Cookie", "").split(";")[0]
# 设置uid
cookies = uid

# 更新cookie
headers["Cookie"] = cookies

js_code = js_result.text

# 构建可执行的 js
_js_code = 'var window = {document: {cookie: "%s"}};!' %cookies + js_code

ctx = execjs.compile(_js_code)
mfccode = ctx.eval("window.__qzmcf()")

url = f'https://www.qichamao.com/search/all/{quote("小米")}?mfccode={mfccode}'

result = requests.get(url, headers=headers, proxies=proxies, verify=False)
print(result.text)

```

![](WX20200610-162446@2x.png)



### 总结

一个很好的使用动态js调试案例，没什么难度，之前没接触过这类情况，需要耐心。