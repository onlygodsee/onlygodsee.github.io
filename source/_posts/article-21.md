---
title: AST学习-合并Literal类型的计算表达式
date: 2020-06-18 11:46:34
tags:
- AST
categories:
- 技术
- AST
---



将如下示例代码进行合并

````javascript
var a = !![], b = 'Hey ' + 'Rach' + '!',c = 8 + 3 * 40,d = Math.abs(-911) % 19,e = true ? 996:250;
````

### 使用babel库中的evaluate方法

源代码路径

```bash
node_modules/@babel/traverse/lib/path/evaluation.js
```

查看 AST-Tree 

![](WX20200618-115653.png)

发现这五个变量的初始化节点类型有三种 ***UnaryExpression***, ***BinaryExpression***, ***ConditionalExpression***

然后通过代码遍历查看结果

#### 优先顶层遍历

```javascript
// 优先顶层遍历
const vistor = {
    "UnaryExpression|BinaryExpression|ConditionalExpression"(path){
        const {value} = path.evaluate();
        console.log(path.toString(), value);
    }
}
```

查看结果

![](WX20200618-120552.png)

这里出现了结果，但是遍历是从顶层开始的

#### 优先底层遍历

```javascript
// 从底层遍历
const vistor = {
    "UnaryExpression|BinaryExpression|ConditionalExpression":{
        exit: function(path){
            const {value} = path.evaluate();
            console.log(path.toString(), value)
        }
    }
}
```

查看结果

![](WX20200618-120824.png)

**enter：默认值，顶层优先**

**exit：底层优先**

#### 通用完整代码

```javascript
// 将 JS 源码转换成语法树的函数
const parser = require("@babel/parser")
// 遍历 AST 的函数
const traverse = require("@babel/traverse").default
// 操作节点的函数
const t = require("@babel/types")
// 将语法书转换成源代码的函数
const generator = require("@babel/generator").default

var jscode = "var a = !![], b = 'Hey ' + 'Rach' + '!',c = 8 + 3 * 40,d = Math.abs(-911) % 19,e = true ? 996:250;";

// 根据value类型替换相对应节点
function PathToLiteral(path, value){
    switch (typeof value) {
        case "boolean":
            path.replaceWith(t.BooleanLiteral(value));
            break;
        case "string":
            path.replaceWith(t.StringLiteral(value));
            break;
        case "number":
            path.replaceWith(t.NumericLiteral(value));
            break;
        default:
            break;
    }
}

// 执行替换入口函数
function do_replace(path){
    const {value} = path.evaluate();
    PathToLiteral(path, value);
}

// 注意这里使用enter而非exit
const vistor = {
    "UnaryExpression|BinaryExpression|ConditionalExpression":{
        enter: [do_replace]
    }
}

const ast = parser.parse(jscode);
traverse(ast, vistor)

let {code} = generator(ast)
console.log(code)
```

查看结果

![](WX20200618-121458.png)

#### 总结

尽量多阅读源码，应有尽有。