---
title: AST学习-将 BinaryExpression 类型转换为 CallExpression 类型
date: 2020-06-18 15:34:28
tags:
- AST
categories:
- 技术
- AST
---

将 BinaryExpression 类型转换为 CallExpression 类型

<!-- more -->

### 目标

```javascript
var a = 996 | 250;
```

转换为

```javascript
var a = function (s, h) {
  return s | h;
}(996, 250);
```

### 分析

![](WX20200618-154437.png)

对比发现差别在于 ***VariableDeclarator*** 节点的初始化中，所以转换思路就是将原来的 ***BinaryExpression*** 节点替换为 ***CallExpression*** 节点

### 实现

#### 修改BinaryExpression节点

代码如下

```javascript
// 将 JS 源码转换成语法树的函数
const parser = require("@babel/parser")
// 遍历 AST 的函数
const traverse = require("@babel/traverse").default
// 操作节点的函数
const t = require("@babel/types")
// 将语法书转换成源代码的函数
const generator = require("@babel/generator").default
// 操作文件的函数
const fs = require("fs")

jscode = "var a = 996 | 250;"

function binary2func(path){
    init_path = path.get('init');
    if (!init_path.isBinaryExpression) return;

    // 将type改为 CallExpression
    init_path.node.type = "CallExpression";

    // 添加 arguments 节点
    let {operator, left, right} = init_path.node;
    init_path.node.arguments = [left, right];

    // 添加 callee 节点
    // 先构造 params
    let s = t.Identifier('s');
    let h = t.Identifier('h');
    let params = [s, h];

    // 构造 BlockStatement->ReturnStatement->BinaryExpression
    let binaryExpress = t.BinaryExpression(operator, s, h);
    let returnStmt = t.ReturnStatement(binaryExpress);
    let body = t.BlockStatement([returnStmt]);

    let id = null
    const fuuncExpress = t.FunctionExpression(id, params, body);

    init_path.node.callee = fuuncExpress;
}

const vistor = {
    VariableDeclarator:{
        enter: [binary2func]
    }
}

const ast = parser.parse(jscode);
traverse(ast, vistor)

let {code} = generator(ast);
console.log(code)
```



#### 构造 CallExpression 替换 BinaryExpression

```javascript
// 将 JS 源码转换成语法树的函数
const parser = require("@babel/parser")
// 遍历 AST 的函数
const traverse = require("@babel/traverse").default
// 操作节点的函数
const t = require("@babel/types")
// 将语法书转换成源代码的函数
const generator = require("@babel/generator").default
// 操作文件的函数
const fs = require("fs")

jscode = "var a = 996 | 250;"

function binary2func(path) {
    init_path = path.get('init');

    if (!init_path.isBinaryExpression()) return;

    let id = null;
    let {operator, left, right} = init_path.node;

    // 构造 arguments 节点
    const arguments = [left, right];

    // 构造 FunctionExpression 节点
    let s = t.Identifier('s');
    let h = t.Identifier('h');
    let params = [s, h]
    let binaryExpress = t.BinaryExpression(operator, s, h);
    let returnStmt = t.ReturnStatement(binaryExpress);
    let blockStmt = t.BlockStatement([returnStmt]);
    const funcExpress = t.FunctionExpression(id, params,blockStmt)

    init_path.replaceWith(t.CallExpression(funcExpress, arguments));

}

const vistor = {
    VariableDeclarator:{
        enter: [binary2func]
    }
}

const ast = parser.parse(jscode);
traverse(ast, vistor)

let {code} = generator(ast);
console.log(code)
```

打印结果

![](WX20200618-155056.png)

### 总结

注意本文中节点的修改和替换操作都是由底层向顶层的。