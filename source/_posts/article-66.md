---
title: AST学习-scope常用方法
date: 2020-06-17 10:40:27
tags:
- AST
categories:
- 技术
- AST
---

scope常用方法

<!-- more -->

### 作用域 Scope

`@Babel`解析出来的语法树节点对象会包含作用域信息，这个信息会作为节点`Node`对象的一个属性保存
这个属性本身是一个`Scope`对象，其定义位于`node_modules/@babel/traverse/lib/scope/index.js`中



示例代码如下：

```javascript
const parser = require("@babel/parser");
const traverse = require("@babel/traverse").default;
const type = require("@babel/types");
const generator = require("@babel/generator").default;

jscode = 'function squire(i){\n' +
    '    return i * i * i;\n' +
    '};\n' +
    '\n' +
    'function i()\n' +
    '{\n' +
    '    var i = 123;\n' +
    '    i += 2;\n' +
    '    return 123;\n' +
    '};'

var visitor = {
    FunctionDeclaration(path){
        console.log("\n\nhere", path.node.id.name + "()")
        path.scope.dump()
    }
}

let ast = parser.parse(jscode);
traverse(ast, visitor);
let {code} = generator(ast)
console.log(code)
```

输出如下

```text
here squire()
------------------------------------------------------------
# FunctionDeclaration
 - i { constant: true, references: 3, violations: 0, kind: 'param' }
# Program
 - squire { constant: true, references: 0, violations: 0, kind: 'hoisted' }
 - i { constant: true, references: 0, violations: 0, kind: 'hoisted' }
------------------------------------------------------------


here i()
------------------------------------------------------------
# FunctionDeclaration
 - i { constant: false, references: 0, violations: 1, kind: 'var' }
# Program
 - squire { constant: true, references: 0, violations: 0, kind: 'hoisted' }
 - i { constant: true, references: 0, violations: 0, kind: 'hoisted' }
------------------------------------------------------------
function squire(i) {
  return i * i * i;
}

function i() {
  var i = 123;
  i += 2;
  return 123;
}
```



#### 输出查看方法

- 每一个作用域都以`#`标识输出
- 每一个绑定都以`-`标识输出
- 对于单次输出，都是自底向上的;先输出当前作用域，再输出父级作用域，再输出父级的父级作用域……
- 对于单个绑定`Binding`，会输出4种信息
  - constant 声明后，是否会被修改
  - references 被引用次数
  - violations 被重新定义的次数
  - kind 函数声明类型。param 参数, hoisted 提升，var 变量， local 内部





