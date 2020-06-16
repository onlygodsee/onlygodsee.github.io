---
title: AST学习-认识AST结构
date: 2020-06-16 17:16:41
tags:
- AST
categories:
- 技术
- AST
---

环境：Mac  Node.js  babel



## AST基本结构

### 变量声明 VariableDeclaration

![](WX20200616-172515.png)

#### declarations 基本说明

type 表示节点类型，变量声明就是：VariableDeclaration

start end 表示节点的起始位置

kind 变量声明关键字 分为 var let const

***declarations*** 需要重点看下

![](WX20200616-173414.png)

declarations 是一个数组结构，里边存放了变量的声明信息，声明了几个变量，就有几个 ***VariableDeclarator***，注意只要声明了就会有这个节点，不管是否赋值

![](WX20200616-173817.png)

#### VariableDeclarator 说明

![](WX20200616-175248.png)

除了前面提到过的节点，还出现了两个新节点 ***id*** ***init*** 

***id*** : 对变量名的描述，包含类型、位置和名称

***init*** : 表示变量初始化的情况，包含类型、位置和value

### 其他类型表示

1+2，b+c 这种是 BinaryExpression
函数为 FunctionExpression
对象 ObjectExpression
new Object() 为 NewExpression
[] 数组为 ArrayExpression
Math.sin() 为 CallExpression

### 使用babel库操作AST

#### 打印整个AST

```javascript
// 下面的导入不一定会全用，但是要知道
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

var jscode = "var a = 123;";
let ast = parser.parse(jscode);

fs.writeFile('ast.json', JSON.stringify(ast, null, '\t'), (err)=>{});
```

结果：

```json
{
	"type": "File",
	"start": 0,
	"end": 12,
	"loc": {
		"start": {
			"line": 1,
			"column": 0
		},
		"end": {
			"line": 1,
			"column": 12
		}
	},
	"errors": [],
	"program": {
		"type": "Program",
		"start": 0,
		"end": 12,
		"loc": {
			"start": {
				"line": 1,
				"column": 0
			},
			"end": {
				"line": 1,
				"column": 12
			}
		},
		"sourceType": "script",
		"interpreter": null,
		"body": [
			{
				"type": "VariableDeclaration",
				"start": 0,
				"end": 12,
				"loc": {
					"start": {
						"line": 1,
						"column": 0
					},
					"end": {
						"line": 1,
						"column": 12
					}
				},
				"declarations": [
					{
						"type": "VariableDeclarator",
						"start": 4,
						"end": 11,
						"loc": {
							"start": {
								"line": 1,
								"column": 4
							},
							"end": {
								"line": 1,
								"column": 11
							}
						},
						"id": {
							"type": "Identifier",
							"start": 4,
							"end": 5,
							"loc": {
								"start": {
									"line": 1,
									"column": 4
								},
								"end": {
									"line": 1,
									"column": 5
								},
								"identifierName": "a"
							},
							"name": "a"
						},
						"init": {
							"type": "NumericLiteral",
							"start": 8,
							"end": 11,
							"loc": {
								"start": {
									"line": 1,
									"column": 8
								},
								"end": {
									"line": 1,
									"column": 11
								}
							},
							"extra": {
								"rawValue": 123,
								"raw": "123"
							},
							"value": 123
						}
					}
				],
				"kind": "var"
			}
		],
		"directives": []
	},
	"comments": []
}
```

#### 访问AST节点内容

```javascript
// 访问编译后的ast结构

const parser = require("@babel/parser");
const traverse = require("@babel/traverse").default;

var js_code = "var a = 123;function b(){debugger;}";

// const vistor = {
//     VariableDeclaration(path) {
//         console.log(path.type);
//         console.log(path.toString());
//     },
// }

// 多路径查看，使用 "|"
const vistor = {
    "VariableDeclaration|VariableDeclarator"(path){
    // "FunctionDeclaration|BlockStatement"(path){
        console.log(path.type);
        console.log(path.toString());
    }
}

let ast = parser.parse(js_code);

traverse(ast, vistor)
```



