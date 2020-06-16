---
title: 认识path和node
date: 2020-06-16 18:32:15
tags:
- AST
catgories:
- 技术
- AST
---

### path(路径)常用操作

#### 获取当前路径所对应的源代码

```javascript
// 使用toString()
const vistor = {
        VariableDeclaration(path) {
            console.log(path.toString())
        }
}
```



#### 判断path是什么type

```javascript
// 使用 path.isXXX()
const vistor = {
   VariableDeclarator(path){
       console.log(path.isVariableDeclarator())
   }
}
```



#### 获取path的上一级路径

```javascript
let parent = path.parentPath;
console.log(parent.node)
```



#### 获取path的子路径

```javascript
// 使用 get()
path.get('id');
```



#### 删除path

```javascript
// remove()
path.remove()
```



#### 替换path

```javascript
const t = require("@babel/types")
// 单路径可以使用replaceWith方法，多路径则使用replaceWithMultiple方法
const vistor = {
    VariableDeclarator(path){
        // 替换时注意路径结构
        path.get('init').replaceWith(t.NumericLiteral(996));
    }
}
```



#### path源码

```bash
@babel\traverse\lib\path
```



### node(节点)常用操作



#### 获取当前节点所对应的源代码

```javascript
const generator = require("@babel/generator").default;
let {code} = generator(node);
```



#### 删除节点

```javascript
delete path.node.init
```



#### 访问子节点

```javascript
const vistor = {
    VariableDeclarator(path){
        // 打印节点初始化的值
        console.log(path.node.init.value)

        // 与 JSON 之间转换
        console.log(JSON.stringify(path.node))
    }
}

var ast = parser.parse(jscode);
traverse(ast, vistor)
```

