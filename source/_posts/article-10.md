---
title: article-10
date: 2020-06-04 14:23:05
tags:
---

aHR0cHM6Ly9wYXNzcG9ydC41OC5jb20vbG9naW4vP3BhdGg9aHR0cHMlM0EvL2Z6LjU4LmNvbS8mUEdUSUQ9MGQxMDAwMDAtMDAxMy0wMjk0LTFjZWItYjU3NTBiZDIwNmU5JkNsaWNrSUQ9Mg

### 分析请求

![](WX20200604-142559.png)

登陆请求操作中需要对上述标记的四个参数进行分析

首先看 ***token***，全局搜索一下发现是一个请求返回的

![](WX20200604-142857.png)

![](WX20200604-143121.png)

该请求的参数貌似是定值，再全局搜索，发现是登陆页返回的

![](WX20200604-143415.png)

接下来定位 ***password*** ，继续全局搜索

![](WX20200604-143742.png)

在这里打断点，然后进行调试，发现了加密位置

![](WX20200604-143933.png)

![](WX20200604-144158.png)

还需要两个参数，然后万能的全局查询安排

![](WX20200604-144432.png)

好了，这个也搞定了，然后找其他的

全局搜索 ***fingerprint*** ，发现是在cookie中获取的

![](WX20200604-165705.png)

![](WX20200604-165848.png)

顺势定位 ***finger2*** 参数

![](WX20200604-170312.png)

![](WX20200604-170435.png)

这里就是指纹加密的地方

### 总结

需要注意的是浏览器指纹信息的构建，但总体来说不难