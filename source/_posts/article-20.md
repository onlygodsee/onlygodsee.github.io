---
title: AST学习-拆分一个Literal类型的节点
date: 2020-06-17 16:37:18
tags:
- AST
categories:
- 技术
- AST
---

拆分一个Literal类型的节点

<!-- more -->

### 拆分NumericLiteral节点类型

示例代码

```javascript
var a = 12345678;
```

#### 了解异或操作

```javascript
# 生成随机八位负数
const first = 0 - Math.floor(Math.random() * 10000000 + 10000000)
# 与a做异或操作
const second = a ^ first;
# 将first 与 second 进行异或操作
const res = first ^ second
>>>12345678
```

可以发现输出结果为 ***12345678***,  ***res = first ^ second = first ^ first ^ a = 0 ^ a = a***

了解了这个概念就可以在 AST 中实现了

#### 在 AST 中实现

![](WX20200617-164925.png)

通过 AST-TREE查看可知异或表达式属于 ***BinaryExpression*** 节点，所以我们需要构造这个节点

```javascript
const t = require("@babel/types")
t.BinaryExpression('^', t.NumericLiteral(first), t.NumericLiteral(second))
```

然后使用该节点替换原来的 ***NumericLiteral*** 节点就好了，代码如下

```javascript
const vistor = {
    NumericLiteral(path){
        const node = path.node;
        const value = node.value;
        const first = 0 - Math.floor(Math.random() * 10000000 + 10000000);
        const second = value ^ first;

        path.replaceWith(t.BinaryExpression('^', t.NumericLiteral(first), t.NumericLiteral(second)));
        path.stop();
    }
}
```

查看结果

![](WX20200617-165333.png)