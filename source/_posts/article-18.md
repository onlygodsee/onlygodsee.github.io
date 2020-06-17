---
title: AST学习-十六进制还原十进制
date: 2020-06-17 10:47:27
tags:
- AST
categories:
- 技术
- AST
---

```javascript
var a = "\x48\x65\x6c\x6c\x6f\x2c\x4e\x69\x67\x68\x74\x54\x65\x61\x6d\x21";
```



#### AST-TREE分析

![](WX20200617-115051.png)

由上图可知影响结果的节点是 ***extra.raw*** ，接下来对这个节点进行操作



#### 方式一：遍历VariableDeclarator下init节点，符合条件后将extra.raw重新赋值。

```javascript
const vistor = {
    VariableDeclarator(path){
        const init = path.get('init');
        if (!init.isStringLiteral()) return;
        const node = init.node;
        let {value, extra} = node;
        extra.raw = '"' + value + ':)"';
    }
}
```



#### 方式二：直接遍历type为StringLiteral的路径。

```javascript
const vistor = {
    StringLiteral(path){
        let {value, extra} = path.node;
        extra.raw = '"' + value + ':)"';
    }
}
```



#### 方式三：使用新建节点替换

```javascript
const vistor = {
    StringLiteral(path){
        let {value} = path.node;
        path.replaceWith(t.StringLiteral(value + ":("));
        // 因为新生成的节点类型也是StringLiteral，所以会一直遍历下去，最终导致栈溢出
        // 解决方案为使用path.stop()方法，让其遍历后立即停止。
        path.stop();
    }
}
```



#### 方式四：删除raw节点

```javascript
const vistor = {
    StringLiteral(path){
        let {extra} = path.node;
        delete extra.raw;
    }
}
```



#### 通用步骤 (十六进制数值/字符串还原十进制)

```javascript
const vistor = {
    "StringLiteral|NumericLiteral"(path){
        // 删除extra节点
        delete path.node.extra
    }
}
```

