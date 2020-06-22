---
title: AST学习-删除全部注释
date: 2020-06-22 11:44:31
tags:
- AST
categories:
- 技术
- AST
---

### 目标

删除示例代码中的注释：

```javascript
var a = 123; //this is single line comment
/*

This is a  multiline comments;

test

test

test

*/

var b = 456;
```

### 分析

![](WX20200622-114717.png)

但使用插件遍历发现如下异常

![](WX20200622-115102.png)

这时使用generator直接操作，将comments参数声明为false，就可以去掉注释了

![](WX20200622-115311.png)

### 代码

```javascript
const ast = parser.parse(jscode);
// traverse(ast, vistor)

let {code} = generator(ast, opts={comments:false})
console.log(code)
```

查看结果

![](WX20200622-115557.png)