---
title: AST学习-还原简单的 CallExpression 类型
date: 2020-06-19 16:46:06
tags:
- AST
categories:
- 技术
- AST
---

#### 目标

```javascript
var Xor = function (p,q)
{
  return p ^ q;
}

let a = Xor(996,250);
```

转化为如下形式

```javascript
var Xor = function (p,q)
{
  return p ^ q;
}

let a = 996 ^ 250;
```

#### 思路

遍历 ***VariableDeclarator*** 路径，对参数及函数主体进行判断校验，筛选出符合条件的节点

遍历函数定义作用域，当 ***CallExpression*** 与函数名相同时执行替换

#### 完整代码

```javascript
// 将 JS 源码转换成语法树的函数
const parser = require("@babel/parser")
// 遍历 AST 的函数
const traverse = require("@babel/traverse").default
// 操作节点的函数
const t = require("@babel/types")
// 将语法书转换成源代码的函数
const generator = require("@babel/generator").default

jscode = "var Xor = function (p,q)\n" +
    "{\n" +
    "  return p ^ q;\n" +
    "}\n" +
    "\n" +
    "let a = Xor(996,250);"

function call2express(path){
    // 获取函数名及初始化节点
    const {init, id} = path.node;
    const name = id.name;
    const params = init.params;

    // 判断行参是否为两个
    if (!params.length === 2) return;

    // 获取行参名
    let first_name = params[0].name;
    let second_name = params[1].name;

    let func_body = init.body
    // 校验函数主体内容
    if (!func_body.body || !func_body.body.length === 1) return;

    let return_body = func_body.body[0];
    let arguments = return_body.argument;
    let {operator, left, right} = arguments;

    // 判断ReturnStatement节点内容
    if (!t.isReturnStatement(return_body) || !t.isBinaryExpression(arguments)) return;

    // 判断return语句和行参是否一致
    if (!t.isIdentifier(left, {name:first_name}) || !t.isIdentifier(right, {name:second_name})) return;

    // 校验完毕，开始执行替换
    // 获取函数定义作用域
    const scope = path.scope;

    traverse(scope.block, {
        // CallExpression: function (_path) {
        //     node = _path.node;
        //     if (!t.isIdentifier(node.callee, {name:name})) return;
        //     args = node.arguments;
        //
        //     _path.replaceWith(t.BinaryExpression(operator, args[0], args[1]));
        // }
        CallExpression:{
            enter: [
                function (_path) {
                    node = _path.node;
                    if (!t.isIdentifier(node.callee, {name:name})) return;
                    args = node.arguments;

                    _path.replaceWith(t.BinaryExpression(operator, args[0], args[1]));
                }
            ]
        }
    })

}

const vistor = {
    VariableDeclarator(path){
        let init = path.get('init');
        if (init.isFunctionExpression()){
            call2express(path);
        }
    }
}

const ast = parser.parse(jscode)
traverse(ast, vistor)

let {code} = generator(ast);
console.log(code)

```

查看执行结果

![](WX20200619-165529.png)