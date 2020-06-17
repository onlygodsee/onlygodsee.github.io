---
title: AST学习-插入节点
date: 2020-06-17 14:33:43
tags:
- AST
categories:
- 技术
- AST
---

### 变量后面插入一个未被初始化的变量

示例代码

```javascript
var a = 996;
```

#### 新构造一个 **Identifier** 类型的节点，然后直接插入

遍历 ***VariableDeclarator*** 节点

```javascript
const vistor = {
    VariableDeclarator(path){
        path.insertAfter(t.Identifier('b'));
    }
}
```

查看输出结果

![](/Users/woo/Desktop/onlygodsee_git/onlygodsee.github.io/source/_posts/article-19/WX20200617-144220.png)

#### 使用表达式语句

遍历 ***VariableDeclaration*** 节点

```javascript
// var a = 996;var b = 250;
const vistor = {
    VariableDeclaration(path){
        const operator = "=";
        const left = t.Identifier('var b');
        const right = t.NumericLiteral(250);

        const new_assign = t.AssignmentExpression(operator, left, right);
        const new_express = t.ExpressionStatement(new_assign);

        path.insertAfter(new_express);
    }
}
```

查看结果

![](/Users/woo/Desktop/onlygodsee_git/onlygodsee.github.io/source/_posts/article-19/WX20200617-144713.png)

