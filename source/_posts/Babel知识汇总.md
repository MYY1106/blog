---
title: Babel知识汇总
date: 2022-4-2 16:30:59
categories: 
  - [前端, Webpack]
tags:
  - Babel
  - 学习笔记
  - Webpack
top_img: https://css-tricks.com/wp-content/uploads/2020/05/babel-js.png
cover: https://babeljs.io/img/ogImage.png
---

# Babel知识汇总

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203160943162.png" alt="image-20220316094312063" style="zoom:40%;" />

**Babel使用方法：**

总共存在三种方式：

1. 使用单体文件 (standalone script)
2. 命令行 (cli)
3. 构建工具的插件 (webpack 的 babel-loader, rollup 的 rollup-plugin-babel)。

## Babel原理

babel是如何做到将我们的一段代码（ES6、TypeScript、React）转成另外一段代码（ES5）的呢？

**通过编译器**。编译器是**从一种源代码（原生语言）转换成另一种源代码（目标语言）**。事实上我们可以将babel看成 JavaScript 编译器，更确切地说是源码到源码的编译器，通常也叫做“转换编译器（transpiler）”。 意思是说你为 Babel 提供一些 JavaScript 代码，Babel 更改这些代码，然后返回给你新生成的代码。

### 工作流程

Babel也拥有编译器的工作流程：

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203170930020.png" alt="image-20220317093020913" style="zoom:50%;" />

上面只是一个简化版的编译器工具流程，在每个阶段又会有自己具体的工作：

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203170931305.png" alt="image-20220317093107194" style="zoom:50%;" />

#### 解析阶段（Parsing）

将代码解析成抽象语法树（AST），每个JS引擎（比如Chrome浏览器中的V8引擎）都有自己的AST解析器，而Babel是通过@babel/parser实现的。

在解析过程中有两个阶段：**词法分析**和**语法分析**，词法分析阶段把字符串形式的代码转换为**令牌**（tokens）流，令牌类似于AST中节点；而语法分析阶段则会把一个令牌流转换成 AST的形式，同时这个阶段会把令牌中的信息转换成AST的表述结构。

##### 词法解析(Lexical Analysis)

**词法解析器**(Tokenizer)在这个阶段将字符串形式的代码转换为**Tokens**(令牌)，**Tokens** 可以视作是一些语法片段组成的数组

例如

```js
for (const item of items) {}
```

词法解析后的结果如下:

 <img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/2/16d8d0cd40b123d1~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="img" style="zoom:77%;" />

从上图可以看，每个 Token 中包含了语法片段、位置信息、以及一些类型信息。这些信息有助于后续的语法分析。

关于如何实现一个简易的词法解析器可以看[这里](https://juejin.cn/post/6844903849442934798#heading-6)

##### 语法解析(Syntactic Analysis)

这个阶段语法**解析器**(Parser)会把**Tokens**转换为**抽象语法树**(Abstract Syntax Tree，AST)

 <img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/2/16d8d0cd66ac5d80~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="img" style="zoom:77%;" />

`Program`、`CallExpression`、`Identifier` **这些都是节点的类型，每个节点都是一个有意义的语法单元**。 这些节点类型定义了一些属性来描述节点的信息。

**AST 是 Babel 转译的核心数据结构，后续的操作都依赖于 AST**

#### 转换阶段（Transformation）

在这个阶段，Babel接受得到AST并通过babel-traverse对其进行深度优先遍历，在此过程中对节点进行添加、更新及移除操作。这部分也是Babel插件介入工作的部分。

#### 生成阶段（Code Generation）

将经过转换得到的**新的AST**通过babel-generator再转换成js代码，过程就是深度优先遍历整个AST，然后构建可以表示转换后代码的字符串。

----
<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/2/16d8d0cd559c7e1e~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="img" style="zoom: 80%;" />

#### 示例

**原生源代码**：

```js
const name = "coderwhy";
const foo = (name) => console.log(name);
foo(name);
```

经过**词法分析**得到**token数组**：

```json
[
    {
        "type": "Keyword",
        "value": "const"
    },
    {
        "type": "Identifier",
        "value": "foo"
    },
    {
        "type": "Punctuator",
        "value": "="
    },
    {
        "type": "Punctuator",
        "value": "("
    },
    {
        "type": "Identifier",
        "value": "name"
    },
    {
        "type": "Punctuator",
        "value": ")"
    },
    {
        "type": "Punctuator",
        "value": "=>"
    },
    {
        "type": "Identifier",
        "value": "console"
    },
    {
        "type": "Punctuator",
        "value": "."
    },
    {
        "type": "Identifier",
        "value": "log"
    },
    {
        "type": "Punctuator",
        "value": "("
    },
    {
        "type": "Identifier",
        "value": "name"
    },
    {
        "type": "Punctuator",
        "value": ")"
    },
    {
        "type": "Punctuator",
        "value": ";"
    },
    {
        "type": "Identifier",
        "value": "foo"
    },
    {
        "type": "Punctuator",
        "value": "("
    },
    {
        "type": "String",
        "value": "\"coderwhy\""
    },
    {
        "type": "Punctuator",
        "value": ")"
    },
    {
        "type": "Punctuator",
        "value": ";"
    }
]
```

通过**语法分析**得到**AST**：

```js
{
  "type": "Program",
  "body": [
    {
      "type": "VariableDeclaration",
      "declarations": [
        {
          "type": "VariableDeclarator",
          "id": {
            "type": "Identifier",
            "name": "foo"
          },
          "init": {
            "type": "ArrowFunctionExpression",
            "id": null,
            "params": [
              {
                "type": "Identifier",
                "name": "name"
              }
            ],
            "body": {
              "type": "CallExpression",
              "callee": {
                "type": "MemberExpression",
                "computed": false,
                "object": {
                  "type": "Identifier",
                  "name": "console"
                },
                "property": {
                  "type": "Identifier",
                  "name": "log"
                }
              },
              "arguments": [
                {
                  "type": "Identifier",
                  "name": "name"
                }
              ]
            },
            "generator": false,
            "expression": true,
            "async": false
          }
        }
      ],
      "kind": "const"
    },
    {
      "type": "ExpressionStatement",
      "expression": {
        "type": "CallExpression",
        "callee": {
          "type": "Identifier",
          "name": "foo"
        },
        "arguments": [
          {
            "type": "Literal",
            "value": "coderwhy",
            "raw": "\"coderwhy\""
          }
        ]
      }
    }
  ],
  "sourceType": "script"
}
```

经过**遍历**、**访问**、**应用所需的插件**，生成**新的AST**：

```js
{
  "type": "Program",
  "body": [
    {
      "type": "VariableDeclaration",
      "declarations": [
        {
          "type": "VariableDeclarator",
          "id": {
            "type": "Identifier",
            "name": "foo"
          },
          "init": {
            "type": "FunctionExpression",
            "id": {
              "type": "Identifier",
              "name": "foo"
            },
            "params": [
              {
                "type": "Identifier",
                "name": "name"
              }
            ],
            "body": {
              "type": "BlockStatement",
              "body": [
                {
                  "type": "ReturnStatement",
                  "argument": {
                    "type": "CallExpression",
                    "callee": {
                      "type": "MemberExpression",
                      "computed": false,
                      "object": {
                        "type": "Identifier",
                        "name": "console"
                      },
                      "property": {
                        "type": "Identifier",
                        "name": "log"
                      }
                    },
                    "arguments": [
                      {
                        "type": "Identifier",
                        "name": "name"
                      }
                    ]
                  }
                }
              ]
            },
            "generator": false,
            "expression": false,
            "async": false
          }
        }
      ],
      "kind": "var"
    },
    {
      "type": "ExpressionStatement",
      "expression": {
        "type": "CallExpression",
        "callee": {
          "type": "Identifier",
          "name": "foo"
        },
        "arguments": [
          {
            "type": "Literal",
            "value": "coderwhy",
            "raw": "\"coderwhy\""
          }
        ]
      }
    }
  ],
  "sourceType": "script"
}
```

最后得到目标源代码：

```js
var name = "coderwhy";

var foo = function (name) {
  return console.log(name);
};

foo(name);
```

### 架构

`Babel` 和 `Webpack` 为了适应复杂的定制需求和频繁的功能变化，都使用了[微内核](https://juejin.cn/post/6844903943068205064#heading-10)的架构风格。**也就是说它们的核心非常小，大部分功能都是通过插件扩展实现的**。

具体可参考[《透过现象看本质: 常见的前端架构风格和案例🔥》](https://juejin.cn/post/6844903943068205064)

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/2/16d8d0cd5a3f3a0c~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)

#### 核心

`@babel/core` 这也是上面说的**微内核**架构中的**内核**。对于Babel来说，这个内核主要干这些事情：

- 加载和处理配置(config)
- 加载插件
- 调用 `Parser` 进行语法解析，生成 `AST`
- 调用 `Traverser` 遍历AST，并使用`访问者模式`应用**插件**对 AST 进行转换
- 生成代码，包括SourceMap转换和源代码生成

#### 核心周边支撑

- **Parser(`@babel/parser`)**： 将源代码解析为 AST 就靠它了。 它已经内置支持很多语法. 例如 JSX、Typescript、Flow、以及最新的ECMAScript规范。目前为了执行效率，parser是[不支持扩展的](https://link.juejin.cn?target=https%3A%2F%2Fbabeljs.io%2Fdocs%2Fen%2Fbabel-parser%23faq)，由官方进行维护。如果你要支持自定义语法，可以 fork 它，不过这种场景非常少。
- **Traverser(`@babel/traverse`)**：  实现了`访问者模式`，对 AST 进行遍历，**转换插件** 会通过它获取感兴趣的AST节点，对节点继续操作, 下面会详细介绍`访问器模式`
- **Generator(`@babel/generator`)**： 将 AST 转换为源代码，支持 `SourceMap`

#### 插件

打开 Babel 的源代码，会发现有好几种类型的`插件`

- **语法插件(`@babel/plugin-syntax-*`)**：上面说了 `@babel/parser` 已经支持了很多 JavaScript 语法特性，Parser也不支持扩展. **因此`plugin-syntax-\*`实际上只是用于开启或者配置Parser的某个功能特性**。

  一般用户不需要关心这个，Transform 插件里面已经包含了相关的`plugin-syntax-*`插件了。用户也可以通过[`parserOpts`](https://link.juejin.cn?target=https%3A%2F%2Fbabeljs.io%2Fdocs%2Fen%2Foptions%23parseropts)配置项来直接配置 Parser

- **转换插件**： 用于对 AST 进行转换, 实现转换为ES5代码、压缩、功能增强等目的. Babel仓库将转换插件划分为两种(只是命名上的区别)：

  - `@babel/plugin-transform-*`： 普通的转换插件
  - `@babel/plugin-proposal-*`： 还在'提议阶段'(非正式)的语言特性, 目前有[这些](https://link.juejin.cn?target=https%3A%2F%2Fbabeljs.io%2Fdocs%2Fen%2Fnext%2Fplugins%23experimental)

- **预定义集合(`@babel/presets-*`)**： 插件集合或者分组，主要方便用户对插件进行管理和使用。比如`preset-env`含括所有的标准的最新特性; 再比如`preset-react`含括所有react相关的插件.

#### 插件开发辅助

- `@babel/template`： 某些场景直接操作AST太麻烦，就比如我们直接操作DOM一样，所以Babel实现了这么一个简单的模板引擎，可以将字符串代码转换为AST。比如在生成一些辅助代码(helper)时会用到这个库
- `@babel/types`： AST 节点构造器和断言. 插件开发时使用很频繁
- `@babel/helper-*`： 一些辅助器，用于辅助插件开发，例如简化AST操作
- `@babel/helper`： 辅助代码，单纯的语法转换可能无法让代码运行起来，比如低版本浏览器无法识别class关键字，这时候需要添加辅助代码，对class进行模拟。

#### 工具

- `@babel/node`： Node.js CLI, 通过它直接运行需要 Babel 处理的JavaScript文件
- `@babel/register`： Patch NodeJs 的require方法，支持导入需要Babel处理的JavaScript模块
- `@babel/cli`： CLI工具

#### 访问者模式

转换器会遍历 AST 树，找出自己感兴趣的节点类型, 再进行转换操作. 这个过程和我们操作`DOM`树差不多，只不过目的不太一样。AST 遍历和转换一般会使用[`访问者模式`](https://link.juejin.cn?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2F1f1049d0a0f4)。

想象一下，Babel 有那么多插件，如果每个插件自己去遍历AST，对不同的节点进行不同的操作，维护自己的状态。这样子不仅低效，它们的逻辑分散在各处，会让整个系统变得难以理解和调试， 最后插件之间关系就纠缠不清，乱成一锅粥。

**所以转换器操作 AST 一般都是使用`访问器模式`，由这个`访问者(Visitor)`来 **

**① 进行统一的遍历操作，**

**② 提供节点的操作方法，**

**③ 响应式维护节点之间的关系；**

**而插件(设计模式中称为‘具体访问者’)只需要定义自己感兴趣的节点类型，当访问者访问到对应节点时，就调用插件的访问(visit)方法**。

##### 节点的遍历

假设我们的代码如下:

```js
function hello(v) {
  console.log('hello' + v + '!')
}
```

[解析后的 AST 结构](https://astexplorer.net/#/gist/04f2bad7b1a9bd7f4c1b3c354fb8811f/b009645a68cbe4cd1a788e4af98cd4fe8c0e553d)如下:

```json
{
  "type": "Program",
  "start": 0,
  "end": 58,
  "body": [
    {
      "type": "FunctionDeclaration",
      "start": 0,
      "end": 58,
      "id": {
        "type": "Identifier",
        "start": 9,
        "end": 14,
        "name": "hello"
      },
      "expression": false,
      "generator": false,
      "async": false,
      "params": [
        {
          "type": "Identifier",
          "start": 15,
          "end": 16,
          "name": "v"
        }
      ],
      "body": {
        "type": "BlockStatement",
        "start": 18,
        "end": 58,
        "body": [
          {
            "type": "ExpressionStatement",
            "start": 24,
            "end": 54,
            "expression": {
              "type": "CallExpression",
              "start": 24,
              "end": 54,
              "callee": {
                "type": "MemberExpression",
                "start": 24,
                "end": 35,
                "object": {
                  "type": "Identifier",
                  "start": 24,
                  "end": 31,
                  "name": "console"
                },
                "property": {
                  "type": "Identifier",
                  "start": 32,
                  "end": 35,
                  "name": "log"
                },
                "computed": false,
                "optional": false
              },
              "arguments": [
                {
                  "type": "BinaryExpression",
                  "start": 36,
                  "end": 53,
                  "left": {
                    "type": "BinaryExpression",
                    "start": 36,
                    "end": 47,
                    "left": {
                      "type": "Literal",
                      "start": 36,
                      "end": 43,
                      "value": "hello",
                      "raw": "'hello'"
                    },
                    "operator": "+",
                    "right": {
                      "type": "Identifier",
                      "start": 46,
                      "end": 47,
                      "name": "v"
                    }
                  },
                  "operator": "+",
                  "right": {
                    "type": "Literal",
                    "start": 50,
                    "end": 53,
                    "value": "!",
                    "raw": "'!'"
                  }
                }
              ],
              "optional": false
            }
          }
        ]
      }
    }
  ],
  "sourceType": "module"
}
```

还可以利用这个[来看](https://resources.jointjs.com/demos/rappid/apps/Ast/index.html) (这个看起来比较方便)

访问者会以**深度优先**的顺序, 或者说递归地对 AST 进行遍历，其调用顺序如下图所示:

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/2/16d8d0cd95a22af7~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="img" style="zoom:70%;" />

上图中`绿线`表示进入该节点，`红线`表示离开该节点。所以当创建访问者时你实际上有两次机会来访问一个节点。

下面写一个超简单的'具体访问者'来还原上面的遍历过程:

```js
const babel = require('@babel/core')
const traverse = require('@babel/traverse').default

const code = `
function hello(v) {
    console.log('hello' + v + '!')
  }
`

const ast = babel.parseSync(code)
let depth = 0
traverse(ast, {
    enter (path) {
        console.log(`${printSpace(depth)}enter ${path.type}(${path.key})`)
        depth++
    },
    exit (path) {
        depth--
        console.log(`${printSpace(depth)}exit ${path.type}(${path.key})`)
    }
})

function printSpace (depth) {
    space = ''
    for (let i = 0; i < depth; i++)
        space += '  '
    return space
}
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203312303230.png" alt="image-20220331230328091" style="zoom:50%;" />

当访问者进入一个节点时就会调用 `enter` 方法，反之离开该节点时会调用 `exit` 方法

 一般情况下，插件不会像上面那样直接使用`enter`方法，只会关注少数几个节点类型，所以具体访问者也可以这样声明访问方法:

```JS
traverse(ast, {
  // 访问标识符
  Identifier(path) {
    console.log(`enter Identifier`)
  },
  // 访问调用表达式
  CallExpression(path) {
    console.log(`enter CallExpression`)
  },
  
  // 上面是enter的简写，如果要处理exit，也可以这样
  // 二元操作符
  BinaryExpression: {
    enter(path) {},
    exit(path) {},
  },
    
  // 更高级的, 使用同一个方法访问多种类型的节点
  "ExportNamedDeclaration|Flow"(path) {}
})
```

**那么 Babel 插件是怎么被应用的呢？**

Babel 会按照插件定义的顺序来应用访问方法，比如你注册了多个插件，babel-core 最后传递给访问器的数据结构大概长这样：

```json
{
  Identifier: {
    enter: [plugin-xx, plugin-yy,] // 数组形式
  }
}
```

当进入一个节点时，这些插件会按照注册的顺序被执行。大部分插件是不需要开发者关心定义的顺序的，有少数的情况需要稍微注意以下，例如`plugin-proposal-decorators`:

```json
{
  "plugins": [
    "@babel/plugin-proposal-decorators",     // 必须在plugin-proposal-class-properties之前
    "@babel/plugin-proposal-class-properties"
  ]
}
```

所有插件定义的顺序，按照惯例，应该是新的或者说实验性的插件在前面，老的插件定义在后面。因为可能需要新的插件将 AST 转换后，老的插件才能识别语法（向后兼容）。下面是官方配置例子, 为了确保先后兼容，`stage-*`阶段的插件先执行:

```json
{
  "presets": ["es2015", "react", "stage-2"]
}
```

##### 节点的上下文

访问者在访问一个节点时, 会无差别地调用 `enter` 方法，我们怎么知道这个节点在什么位置以及和其他节点的关联关系呢？

通过上面的代码，读者应该可以猜出几分，每个`visit`方法都接收一个 `Path` 对象, 你可以将它当做一个‘上下文’对象，类似于`JQuery`的 `JQuery`(`const $el = $('.el')`) 对象，这里面包含了很多信息：

- 当前节点信息
- 节点的关联信息。父节点、子节点、兄弟节点等等
- 作用域信息
- 上下文信息
- 节点操作方法。节点增删查改
- 断言方法。isXXX, assertXXX

下面是它的主要结构:

```typescript
export class NodePath<T = Node> {
    constructor(hub: Hub, parent: Node);
    parent: Node;
    hub: Hub;
    contexts: TraversalContext[];
    data: object;
    shouldSkip: boolean;
    shouldStop: boolean;
    removed: boolean;
    state: any;
    opts: object;
    skipKeys: object;
    parentPath: NodePath;
    context: TraversalContext;
    container: object | object[];
    listKey: string; // 如果节点在一个数组中，这个就是节点数组的键
    inList: boolean;
    parentKey: string;
    key: string | number; // 节点所在的键或索引
    node: T;  // 🔴 当前节点
    scope: Scope; // 🔴当前节点所在的作用域
    type: T extends undefined | null ? string | null : string; // 🔴节点类型
    typeAnnotation: object;
    // ... 还有很多方法，实现增删查改
}
```

##### 副作用的处理

实际上访问者的工作比我们想象的要复杂的多，上面示范的是简单的静态 AST 的遍历过程。而 AST 转换本身是有副作用的，比如插件将旧的节点替换了，那么访问者就没有必要再向下访问旧节点了，而是继续访问新的节点

代码如下：

```js
traverse(ast, {
  ExpressionStatement(path) {
    // 将 `console.log('hello' + v + '!')` 替换为 `return ‘hello’ + v`
    const rtn = t.returnStatement(t.binaryExpression('+', t.stringLiteral('hello'), t.identifier('v')))
    path.replaceWith(rtn)
  },
}
```

上面的代码, 将`console.log('hello' + v + '!')`语句替换为`return "hello" + v;`, 下图是遍历的过程：

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/2/16d8d0cdaa67a3b1~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="img" style="zoom:60%;" />

我们可以对 AST 进行任意的操作，比如删除父节点的兄弟节点、删除第一个子节点、新增兄弟节点... **当这些操作'污染'了 AST 树后，访问者需要记录这些状态，响应式(Reactive)更新 Path 对象的关联关系, 保证正确的遍历顺序，从而获得正确的转译结果**。

## 实现一个Babel插件

### 预览

Before:

```js
const result = 1 + 2 + 3 + 4 + 5;
```

After:

```js
const result = 15;
```

以上的例子可能大家不会经常遇到，因为傻x才会这么写，但是有可能你会这么写

```js
setTimeout(function(){
  // do something
}, 1000 * 2) // 插件要做的事，就是把 1000 * 2 替换成 2000
```

### 代码

```js
const babel = require("@babel/core");
const traverse = require("@babel/traverse").default;
const t = require("@babel/types");
const generate = require("@babel/generator").default;

const code = `const result = 100 + 2 * (10 + 10) - 50`;
const ast = babel.parseSync(code);

traverse(ast, {
  BinaryExpression: {
    exit(path) {
      const node = path.node;
      let result;

      /** @description 运算 */
      console.log(t.isNumericLiteral(node.left));
      console.log(t.isNumericLiteral(node.right));
      if (t.isNumericLiteral(node.left) && t.isNumericLiteral(node.right)) {
        switch (node.operator) {
          case "+":
            result = node.left.value + node.right.value;
            break;
          case "-":
            result = node.left.value - node.right.value;
            break;
          case "*":
            result = node.left.value * node.right.value;
            break;
          case "/":
            result = node.left.value / node.right.value;
            break;
          case "**":
            let i = node.right.value;
            while (--i) {
              result = result || node.left.value;
              result = result * node.left.value;
            }
            break;
          default:
        }

        /** @description 如果上面的运算有结果的话 */
        if (result !== undefined) {
          /**
           * @description 把表达式节点替换成number字面量
           * @example 1+2 -> 3
           */
          path.replaceWith(t.numericLiteral(result));
        }
      }
    },
  },
});

console.log(generate(ast).code);
```

![image-20220402154650394](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204021546468.png)

当code为

```js
const code = `setTimeout(function() {
    // do something
  }, 1000 * 2);`;
```

![image-20220402163537131](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204021635204.png)

也是可以成功实现的

### 坑

首先这里有一个坑：

如果不是在exit对节点进行操作，而是在enter的话，结果就是

![image-20220402161637209](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204021616282.png)

这是因为

你会发现Babel解析成表达式里面再嵌套表达式：

```
表达式( 表达式( 100 + 表达式( 2 * 表达式( 10 + 10 ) ) ) - 50 )
```

而我们的判断条件并不符合所有的，只符合`10 + 10`

```js
// 判断表达式两边，是否都是数字
if (t.isNumericLiteral(node.left) && t.isNumericLiteral(node.right)) {}
```

所以我们在exit对其节点进行操作：

第一次计算`10 + 10`之后，我们会得到这样的表达式

```
-> 表达式( 表达式( 100 + 表达式( 2 * 20 ) ) - 50 
```

其中 `2 * 20`又符合了我们的条件， 我们通过向上递归的方式遍历父级节点

又转换成这样:

```
-> 表达式( 表达式( 100 + 40 ) - 50 )
-> 表达式( 140 - 50 )
-> 90
```

### 拓展

如果转换这样呢: `const result = 0.1 + 0.2;`

预期肯定是`0.3`, 但是实际上，Javascript有浮点计算误差，得出的结果是`0.30000000000000004`

那是不是这个插件就没卵用？

这就需要你去矫正浮点运算误差了，可以使用[Big.js](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FMikeMcl%2Fbig.js);

比如: `result = node.left.value + node.right.value;` 改成 `result = +new Big(node.left.value).plus(node.right.value);`

你以为完了吗? 这个插件还可以做很多

比如: `Math.PI * 2` >>> `6.283185307179586`

比如: `Math.pow(2, 2)` >>> `4`

所以，有时间的话，可以继续拓展，成为一个能对数字进行一些处理的Babel插件

### ref

https://juejin.cn/post/6844903566809759758

## 使用单体文件 (standalone script)

具体参考：https://babeljs.io/docs/en/babel-standalone

这种方式也用得比较少

一个简单的示例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div id="output"></div>
    <!-- Load Babel -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <!-- Your custom script here -->
    <script type="text/babel">
        const getMessage = () => "Hello World";
        document.getElementById("output").innerHTML = getMessage();
    </script>
</body>

</html>
```

## 命令行使用Babel

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203160944201.png" alt="image-20220316094429120" style="zoom:40%;" />

目前的项目目录

![image-20220316094630402](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203160946667.png)

```js
// /src/index.js

const msg = 'Hello World'

const foo = (info) => {
    console.log(info);
}

foo(msg)
```

```bash
npx babel src --out-dir result
```

```js
// result/index.js

const msg = 'Hello World';

const foo = info => {
  console.log(info);
};

foo(msg);
```

但是可以看到，转换结果与原结果没有什么不同

如果想要转换箭头函数和const的话，是需要安装插件的，分别是 `@babel/plugin-transform-arrow-functions` 和 `@babel/plugin-transform-block-scoping` 

```bash
npx babel src --out-dir result --plugins=@babel/plugin-transform-arrow-functions,@babel/plugin-transform-block-scoping
```

```js
// /result/index.js
var msg = 'Hello World';

var foo = function (info) {
  console.log(info);
};

foo(msg);
```

可以看到转换成功了

但是如果要转换的内容过多，一个个安装设置插件是比较麻烦的，我们可以使用预设（preset），即插件 `@babel/preset-env` ：

```bash
npx babel src --out-dir dist --presets=@babel/preset-env
```

```js
// /result/index.js
var msg = 'Hello World';

var foo = function (info) {
  console.log(info);
};

foo(msg);
```

可以看到，转换成功了

## 构建工具的插件 (webpack 的 babel-loader等)

在实际开发中，我们通常会在构建工具中通过配置babel来对其进行使用的，比如在webpack中。

那么我们就需要去安装相关的依赖：

```powershell
npm install babel-loader @babel/core
```

我们可以设置一个规则，在加载js文件时，使用我们的babel，同时还需指定使用的插件才会生效：

```js
module.exports = {
    ...
    module: {
        rules: [
            {
                test: /\.js$/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        plugins: [
                            '@babel/plugin-transform-arrow-functions',
                            '@babel/plugin-transform-block-scoping'
                        ]
                    }
                }
            }
        ]
    },
    ...
}
```

----

如果我们一个个去安装使用插件，**那么需要手动来管理大量的babel插件**，同时我们也不太清楚我们**想要兼容的浏览器到底需要使用哪些插件**，这里我们可以直接给webpack提供一个 **preset**，**webpack会根据我们的预设来加载对应的插件列表，并且将其传递给babel**。
常见的预设有三个：

- **env**

  安装@babel/preset-env

  然后设置：

  ```js
  module.exports = {
      ...
      module: {
          rules: [
              {
                  test: /\.js$/,
                  use: {
                      loader: 'babel-loader',
                      options: {
                          presets: [
                              ["@babel/preset-env"]
                          ]
                      }
                  }
              }
          ]
      },
      ...
  }
  ```

  打包后：

  ![image-20220317133430230](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171334453.png)

  可以看到转换成功

  需要注意，这里没有安装`@babel/plugin-transform-arrow-functions` 和 `@babel/plugin-transform-block-scoping` 等其他插件，只安装了 `@babel/core` 和 `@babel/preset-env` 

  这里需要注意，@babel/preset-env会根据我们的 **browserslist** 的设置去兼容浏览器，如果我们设置**browserslist**为

  ```js
  "browserslist": [
      "chrome 99"
  ]
  ```

  打包后可以发现，箭头函数和const并没有被转换

  ![image-20220317134557694](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171345919.png)

- **react**

  

- **TypeScript**

  

- **Stage-X**

   <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171352107.png" alt="image-20220317135239011" style="zoom:38%;" />
  
   <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171354941.png" alt="image-20220317135404879" style="zoom: 38%;" />

### ployfill

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171433819.png" alt="image-20220317143358747" style="zoom:45%;" />

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171435175.png" alt="image-20220317143524072" style="zoom:45%;" />

接着在babel.config.js里面设置：

```js
module.exports = {
    presets: [
        ["@babel/preset-env", {
            useBuiltIns: 'usage', // 设置以什么样的方式来使用polyfill,详细见下文
            corejs: 3 // 设置使用版本，如果使用3是必须设置的，因为默认版本是2
        }]
    ]
}
```

```js
module.exports = {
    ...
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/, // 不对node_modules内的js文件使用babel-loader
                use: ['babel-loader']
            }
        ]
    },
    ...
}	
```

![image-20220317145433089](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171454324.png)

可以看到使用了ployfill之后，一个promise就多了三千行代码😓

#### useBuiltIns

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171439265.png" alt="image-20220317143924195" style="zoom:45%;" />

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171452738.png" alt="image-20220317145200649" style="zoom:50%;" />

### plugin-transform-runtime

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171505416.png" alt="image-20220317150526281" style="zoom:40%;" />

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171511428.png" alt="image-20220317151134302" style="zoom:40%;" />

![image-20220317151222022](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171512272.png)

### React的jsx支持

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171519523.png" alt="image-20220317151954431" style="zoom:40%;" />

### TypeScript的支持

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171548656.png" alt="image-20220317154831538" style="zoom:45%;" />

----

如果我们希望在webpack中使用TypeScript，有两种方式

#### ts-loader

那么我们可以使用**ts-loader**来处理ts文件：

安装ts-loader后

```js
module.exports = {
    ...
    entry: './src/index.ts',
    module: {
        rules: [
            {
                test: /\.ts$/,
                exclude:''
                use:'ts-loader'
            }
        ]
    },
    ...
}
```

```ts
// /src/index.ts
const a: string = 'hello';
console.log(a);
```

![image-20220317155749214](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171557548.png)

可以看到使用成功

#### babel-loader

还可以直接使用babel去支持ts

```js
module.exports = {
    ...
    entry: './src/index.ts', // 可以是相对路径，也可以是绝对路径
    module: {
        rules: [
            {
                test: /\.ts$/,
                exclude: /node_modules/, // 不对node_modules内的ts文件使用babel-loader
                use: ['babel-loader']
            }
        ]
    },
    ...
}
```

```js
// babel.config.js
module.exports = {
    presets: [
        ["@babel/preset-env", {
            useBuiltIns: 'usage',
            corejs: 3
        }],
        ["@babel/preset-typescript"]
    ]
}
```

![image-20220317164202936](https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171642255.png)

#### 选择哪个？

那么我们在开发中应该选择ts-loader还是babel-loader呢？

- 使用ts-loader（TypeScript Compiler）
  - 直接编译TypeScript，那么只能将ts转换成js；
  - 如果我们还希望在这个过程中添加对应的polyfill，那么ts-loader是无能为力的；
  - 我们需要借助于babel来完成polyfill的填充功能；

- 使用babel-loader（Babel）
  - 来直接编译TypeScript，也可以将ts转换成js，并且可以实现polyfill的功能；
  - 但是babel-loader在编译的过程中，不会对类型错误进行检测；

那么在开发中，我们该如何选择呢？

https://www.typescriptlang.org/docs/handbook/babel-with-typescript.html

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202203171706026.png" alt="202203171703382" style="zoom:50%;" />

当然，使用vsc的话，只会直接在代码提示报错的，但是有些IDE不会，所以vsc👍

## ref

[Babel 插件手册](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md)

[深入浅出 Babel 上篇：架构和原理 + 实战](https://juejin.cn/post/6844903956905197576)

[深入Babel，这一篇就够了](https://juejin.cn/post/6844903746804137991)

[面试官(7): 聊一聊 Babel?](https://juejin.cn/post/6844903849442934798)

[AST 节点类型对照表](https://github.com/NightTeam/JavaScriptAST#ast-%E8%8A%82%E7%82%B9%E7%B1%BB%E5%9E%8B%E5%AF%B9%E7%85%A7%E8%A1%A8)

## tool

[astexplorer](https://astexplorer.net/)

[jointjsast](https://resources.jointjs.com/demos/rappid/apps/Ast/index.html)

