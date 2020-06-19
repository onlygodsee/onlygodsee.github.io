---
title: AST学习-将a["length"]转变为a.length
date: 2020-06-19 18:43:46
tags:
- AST
categories:
- 技术
- AST
---

### 目标

```javascript
var a = 'hello world!';
var b = a['length'];
```

转化为

```javascript
var a = 'hello world!';
var b = a.length;
```

### 分析

![](WX20200619-184840.png)

经AST-Tree比较获知差距在于 ***property*** 和 ***computed*** 这两个路径，替换掉就好了

### 核心代码

```javascript
function member2identify(path){
    const init = path.get('init');

    if (!init.isMemberExpression()) return;

    const member_node = init.node;
    const member_property = init.get('property');

    member_node.computed = false;
    let value = member_node.property.value;
    member_property.replaceWith(t.Identifier(value));
}

const vistor = {
    VariableDeclarator:{
        enter: [member2identify]
    }
}
```

查看结果

![](WX20200619-185204.png)