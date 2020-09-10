---
title: AST学习-删除空行和空语句
date: 2020-06-22 12:13:40
tags:
- AST
categories:
- 技术
- AST
---

删除空行和空语句

<!-- more -->

### 目标

示例代码：

```javascript
var a = 123;


;


var b = 456;
```

转换为

```javascript
var a = 123;
var b = 456;
```

### 分析

![](WX20200622-121701.png)

空语句的节点type是 ***EmptyStatement*** ，空行不会被解析，所以使用generator方法处理

### 代码

```javascript
// 将 JS 源码转换成语法树的函数
const parser = require("@babel/parser")
// 遍历 AST 的函数
const traverse = require("@babel/traverse").default
// 操作节点的函数
const t = require("@babel/types")
// 将语法书转换成源代码的函数
const generator = require("@babel/generator").default

var jscode = 'var a = 123;\n' +
    '\n' +
    '\n' +
    '\n' +
    ';\n' +
    '\n' +
    '\n' +
    '\n' +
    'var b = 456;'

// 删除空语句
const vistor = {
    EmptyStatement(path){
        path.remove();
    }
}

const ast = parser.parse(jscode);
traverse(ast, vistor)

// 删除空行
options = {
    readlines: true
}

let {code} = generator(ast, opts=options);
console.log(code)
```

查看结果

![](WX20200622-122351.png)

