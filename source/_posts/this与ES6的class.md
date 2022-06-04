---
title: this 与 ES6的class
date: 2022-4-16 14:03:59
updated: 2022-4-16 22:03:59
categories: 
  - redrock
tags:
  - JavaScript
  - redrock
top_img: https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204162206282.png
cover: https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204161445375.jpeg
---
# this 与 ES6的class

## this

this这节课好像是在我那节课要讲的知识，但是由于时间问题，就没有讲了

因为不知道大家对它的理解如何，而且这一块应该是JS基础中的难点，所以今天就再给大家复习（or预习）一下

----

`js`中的`this`是很容易让人觉得困惑的地方，这篇文章打算说一下`this`绑定的几种情况，相信可以解决大部分关于`this`的疑惑。 

`this`是在**运行的时候绑定**的，不是在**编写的时候绑定**的，函数调用的方式不同，就可能使`this`所绑定的对象不同。 

### 一些前置知识

#### 浏览器环境

- **this \=\=\= window：**

  ```js
  console.log(this); // window
  ```

  ```js
  'use strict';
  console.log(this) // window
  ```

- var在全局作用域声明的变量有一种行为会挂载在`window对象`上，它会创建一个新的全局变量作为全局对象的属性，这种行为说不定会覆盖到window对象上的某个属性，而`let`、`const`声明的变量则不会有这一行为：

  ```js
  var a = 1;
  console.log(window.a); // 1
  
  const b = 2;
  console.log(window.b); // undefined
  
  let c = 3;
  console.log(window.c); // undefined
  ```

#### node环境

- this \=\=\= module.exports \=\=\= module：

  ```js
  console.log(this === module.exports); // true
  console.log(this === exports); // true
  console.log(exports === module.exports); // true
  ```

  这个就是与模块化有关的知识了

- node环境下，使用`node`命令来执行一个文件，他会把这个文件当成一个模块，import或者require进来的文件也是模块，而模块不是全局对象，每个模块都是**同一个全局对象**，所以需要通过global声明：

  ```js
  var a = 1;
  console.log(global.a); // undefined
  
  const b = 2;
  console.log(global.b); // undefined
  
  let c = 3;
  console.log(global.c); // undefined
  
  global.d = 4;
  console.log(global.d); // 4
  
  e = 5;
  console.log(global.e); // 5
  ```

  也就是说还可以这样：

  ```js
  // a.js
  global.a = 1;
  console.log('I\'m a.js')
  require('./b.js')
  ```

  ```js
  // b.js
  console.log('I\'m b.js')
  console.log(global.a);
  ```

  运行a.js：

  ```
  PS C:\Users\MYY\Desktop\demo> node "c:\Users\MYY\Desktop\demo\a.js"
  I'm a.js
  I'm b.js
  1
  ```

  因为每个模块都是**同一个全局对象**，即a的global \=\=\= b的global

### 几种绑定规则

> 这里我们暂时不考虑[严格模式的this指向](https://juejin.cn/post/6844903874478735373)，但严格模式只是要注意几个细节而已，本质上还是基于下面几种绑定规则

函数调用的位置对`this`的指向有着很大的影响，但却不是完全取决于它。下面是几种`this`的绑定规则（若无说明，运行环境均在浏览器）： 

#### 默认绑定

大家来说下下面这段代码输出结果是什么？

```js
function foo () {
    var a = 3;
    console.log(this.a); // 2
}
var a = 2;
foo();
```

**默认规则的意思就是在一般情况下，如果没有别的规则出现，就将`this`绑定到全局对象上**

上面的代码中，`this`是被默认绑定到了全局对象上，所以`this.a`得到的是`2`。我们如何判断这里应用了默认绑定呢？`foo`在调用的时候直接使用不带任何修饰的函数引用，只能使用默认绑定。有人会误认为结果是`3`，`this`常有的几种错误理解之一就是认为`this`指向当前函数的词法作用域，`this`与词法作用域以及作用域对象是完全不同的东西，作用域对象是在引擎内部的，`js`代码是无法访问的。

若开启严格模式，这里的`this`会绑定到`undefined`

#### 隐式绑定

大家来说下下面这段代码输出结果是什么？

```js
function foo () {
    console.log(this.a);
}
const obj = {
    a: 2,
    foo: foo
}
var a = "opps, global"; //全局对象的属性
obj.foo(); // 2
const bar = obj.foo;
bar(); // opps, global
```

**如果在调用位置有上下文对象，说简单点就是这个函数调用时是用一个对象`.`出来的。就像下边这样，它就遵循隐式绑定**

第`9`行代码，就是函数在调用的时候，是用前边的对象加上`.`操作符调用出来的，此时就用到了隐式绑定规则，隐式绑定规则会将函数调用中的`this`绑定到这个上下文对象，此时的`this.a`和`obj.a`是一样的。

##### 隐式丢失

但隐式绑定会出现一个问题，就是**隐式丢失**

上边的第`10`行代码，是新建一个`foo`函数的引用，即`bar`，在最后一行调用的时候，这个函数不具有上下文对象，此时采用默认绑定规则，得到的结果自然是`opps, gloabl`

绑定丢失也会发生在函数作为参数传递的情况下，即传入回调函数时，因为参数传递就是一种隐式赋值（这里讲下[函数参数的小例子](https://juejin.cn/post/6844903854882947080#heading-7)），看如下代码：

```js
function foo () {
    console.log(this.a);
}

function doFoo (fn) {
    fn(); // 在此处调用，参数传递是隐式赋值，丢失this绑定
}

var obj = {
    a: 2,
    foo: foo
};

var a = "opps, global";
doFoo(obj.foo); // opps, global
```

`javascript`环境中内置的函数，在具有接受一个函数作为参数的功能的时候，也会发生像上边这种状况。例如`setTimeout`函数的实现就类似于下边的伪代码：

```js
function setTimeout (fn, delay) {
    // 等待delay毫秒 
    fn();// 在此处调用
}
```

所以回调函数丢失`this`绑定是非常常见的，后边我们再看如何通过固定`this`来修复这个问题。

#### 显式绑定

##### call apply bind

在此之前，相信你已经用过很多次`apply`、`call`和`bind`函数了，使用这3个函数可以直接为你要执行的函数指定`this`，所以这种方式称为**显式绑定**。

```js
function printName (a, b, c) {
    console.log(this.name)
    console.log(a, b, c);
};

const myy = {
    name: 'myy',
    age: 100
}

// call
printName.call(myy, 1, 2, 3)

// apply
printName.apply(myy, [1, 2, 3])

// bind
const printMyyName = printName.bind(myy, 1, 2, 3) // 返回一个硬编码的新函数，将你指定的对象绑定到调用它的函数的this上
printMyyName()
setTimeout(printMyyName, 1000);
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204160038880.png" alt="image-20220416003839838" style="zoom:50%;" />

通过像上边这样调用，我们可以将`foo`的`this`强制绑定到`obj`上。

PS: 如果给`call`传入的是一个基本类型数据，这个基本类型数据将会被转换成对应的基本包装类型。

##### API调用参数指定this

许多第三方库里的函数，以及许多语言内置的函数，都提供了一个可选的参数用来指定函数执行的`this`：

```JS
function foo (el) {
    console.log(el, this.id);
}
const obj = {
    id: "awesome"
};
[1, 2, 3].forEach(foo, obj); // forEach的第二个参数就是用来设置this
```

#### new绑定

> `js`中的所谓的构造函数，其实和一般的普通函数没有什么区别，并不具有特殊性，它们只是被`new`操作符调用的普通函数而已。实际上并不存在什么构造函数，只存在对于函数的**构造调用**

发生构造函数的调用时，会自动执行下边的操作：

1. 创建一个全新的对象。
2. 这个对象会被执行[[Prototype]]连接。
3. 这个新对象会绑定到函数调用的`this`。
4. 执行这个函数里的代码。
5. 如果函数没有返回其他对象，则自动返回这个新对象。 

这个在执行`new`操作的时候对`this`的绑定就叫做`new`绑定。

```js
function fun () {
  this.a = 1;
  this.b = 2;
}
const instance = new fun();
console.log(instance.a);
```

#### 箭头函数的this

`ES6`中的箭头函数是无法使用以上几种规则的，它是根据外层的作用域来决定`this`，即**取决于外层的函数作用域或全局作用域**，而且箭头函数的绑定无法修改，即使是`new`绑定也不可以。

```js
function foo () {
    return (a) => {
        console.log(this.a);
    }
}
const obj1 = {
    a: 2
};
const obj2 = {
    a: 3
};
const bar = foo.apply(obj1);
bar.apply(obj2); // 2
```

### 绑定规则的优先级

前边我们已经说了`this`的几种绑定规则，当函数调用的位置可以使用多条绑定规则的时候，我们就需要确定这几种规则的优先级。

```js
function foo () {
    console.log(this.a);
}
const obj1 = {
    a: 2,
    foo: foo
};
const obj2 = {
    a: 3,
    foo: foo
}
obj1.foo(); // 2
obj2.foo(); // 3
obj1.foo.call(obj2); // 3
obj2.foo.call(obj1); // 2
```

从上边的代码可以看出来，显式绑定的优先级要高于隐式绑定，下边再看看显式绑定和`new`绑定的优先级：

```js
function foo (something) {
    this.a = something;
}
const obj1 = {};
const bar = foo.bind(obj1);
bar(2);
console.log(obj1.a); // 2
const baz = new bar(3);
console.log(obj1.a); // 2
console.log(baz.a); // 3
```

仔细看这段代码，`bar`是`foo`绑定到`obj1`上返回的一个函数，对这个函数进行`new`操作，并传入新的`a`值，发现改变的是新对象`baz`的属性，和`obj1`已经脱离关系。说明`new`绑定的优先级高于硬绑定。 

综上所述，我们在遇到`this`时，如果不是箭头函数，就可以以这种顺序判断它的指向：

1. 如果函数在`new`中调用，绑定到新建的对象
2. 函数通过`call`或`apply`或者硬绑定调用，`this`绑定到指定的对象上
3. 函数在某个上下文对象中调用，绑定到这个上下文对象上
4. 采用默认绑定规则

### 练习题

1. 在浏览器和node环境下运行：

   ```js
   function outerFunc() {
     console.log(this) // ?
   
     function func() {
       console.log(this) // ?
     }
   
     func()
   }
   
   outerFunc.bind({ x: 1 })()
   ```

   **答案:**

   ```
   { x: 1 }
   globalThis
   ```

   PS: [什么是globalThis](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

2. 下面代码执行后，func.count 值为多少？

   ```js
   function func(num) {
     this.count++
   }
   
   func.count = 0
   func(1)
   ```

   **答案:**

   ```
   0
   ```

   在非严格模式下，`this` 指向全局对象。`this` 跟 func 一点关系都没有，所以 `func.count` 保持不变。so easy。

3. 以下箭头函数中 this 指向谁呢？

   ```js
   obj = {
     func() {
       const arrowFunc = () => {
         console.log(this._name)
       }
   
       return arrowFunc
     },
   
     _name: "obj",
   }
   
   obj.func()()
   
   func = obj.func
   func()()
   
   obj.func.bind({ _name: "newObj" })()()
   
   obj.func.bind()()()
   ```
   
   **答案:**

   ```
   obj
   undefined
   newObj
   undefined
   ```

## [ES6的class](https://zh.javascript.info/classes)

前面听完比较绕的东西，接下来我们就来讲讲比较好玩的东西🥰

----

class我个人认为单从语法层面来说的话，是比较简单理解的，具体语法的话可以自己去学习，课上讲就没意思了😶‍🌫️

主要还是在理解面向对象编程（OOP）

### 面向对象和面向过程的区别

面向对象和面向过程是两种不同的编程思想，我们经常会听到两者的比较

其实面向对象和面向过程并不是完全相对的，也并不是完全独立的。

我认为面向对象和面向过程的主要区别是面向过程主要是以动词为主，解决问题的方式是按照顺序一步一步调用不同的函数。而面向对象主要是以名词为主，将问题抽象出具体的对象，而这个对象有自己的属性和方法，在解决问题的时候是将不同的对象组合在一起使用。

- 面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了。
- 面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为。

#### 把大象放冰箱

**具体的实现我们看一下最经典的“把大象放冰箱”这个问题: **

- **面向过程的解决方法**

  在面向过程的编程方式中实现“把大象放冰箱”这个问题答案是耳熟能详的，一共分三步：

  1. 开门（冰箱）
  2. 装进（冰箱，大象）
  3. 关门（冰箱）

- **面向对象的解决方法**
  
  1. 冰箱.开门（）
  2. 冰箱.装进（大象）
  3. 冰箱.关门（）

#### 五子棋例子

如果通过上面的简单例子无法看出面向过程与面向对象的区别，那么可以看下面这个例子：

五子棋，面向过程的设计思路就是首先分析问题的步骤：

1. 开始游戏
2. 黑子先走
3. 绘制画面
4. 判断输赢
5. 轮到白子
6. 绘制画面
7. 判断输赢
8. 返回步骤2

把上面每个步骤用分别的函数来实现，问题就解决了。

而面向对象的设计则是从另外的思路来解决问题。整个五子棋可以分为 

1. 黑白双方，这两方的行为是一模一样的
2. 棋盘系统，负责绘制画面
3. 规则系统，负责对棋局进行判定

**第一类对象（玩家对象）**负责接受用户输入，并告知**第二类对象（棋盘对象）**棋子布局的变化，棋盘对象接收到了棋子的变化就要负责在屏幕上面显示出这种变化，同时利用**第三类对象（规则系统）**来对棋局进行判定。

可以明显地看出，面向对象是以功能来划分问题，而不是步骤。同样是绘制棋局，这样的行为在面向过程的设计中分散在了总多步骤中，很可能出现不同的绘制版本，因为通常设计人员会考虑到实际情况进行各种各样的简化。而面向对象的设计中，绘图只可能在棋盘对象中出现，从而保证了绘图的统一。

功能上的统一保证了面向对象设计的可扩展性。比如我要加入悔棋的功能，如果要改动面向过程的设计，那么从输入到判断到显示这一连串的步骤都要改动，甚至步骤之间的循序都要进行大规模调整。如果是面向对象的话，只用改动棋盘对象就行了，棋盘系统保存了黑白双方的棋谱，简单回溯就可以了，而显示和规则判断则不用顾及，同时整个对对象功能的调用顺序都没有变化，改动只是局部的。

再比如我要把这个五子棋游戏改为围棋游戏，如果你是面向过程设计，那么五子棋的规则就分布在了你的程序的每一个角落，要改动还不如重写。但是如果你当初就是面向对象的设计，那么你只用改动规则对象就可以了，五子棋和围棋的区别不就是规则吗？（当然棋盘大小好像也不一样，但是你会觉得这是一个难题吗？直接在棋盘对象中进行一番小改动就可以了。）而下棋的大致步骤从面向对象的角度来看没有任何变化。

----

了解更多编程范式可以看[聊聊编程范式](https://juejin.cn/post/6935681018649116703)

目前来说，前端更加流行的是[函数式编程](https://juejin.cn/post/7065093131233919006)

## homework

1. 实现一个万年历类（必做）

   ```js
   class Calendar {
     constructor() {
       /** @description 当前年份 */
       this.currentYear = 1970;
       /** @description 当前月份 注意这里是从0开始记起 */
       this.currentMonth = 0;
       /** @description 当前几号 */
       this.currentDay = 1;
       /** @description 本月的第一天是星期几 */
       this.currentFirstWeekDay = 4;
       /** @description 本月共有多少天 */
       this.allDay = 31;
       /** @description dom操作函数 */
       this.renderCallback;
   
       this.init(new Date())
     }
   
     init (date) {
     }
   
     /** @description 获取日历中上个月需显示的天数的具体信息 */
     getPrevMonthDay () {
     }
   
     /** @description 日历中这个月需显示的天数的具体信息 */
     getNowMonthDay () {
     }
   
     /** @description 日历中下个月需显示的天数的具体信息 */
     getNextMonthDay () {
     }
   
     /** @description 上翻一月操作 */
     prevMonth () {
     }
   
     /** @description 下翻一月操作 */
     nextMonth () {
     }
   
     /** @description 上翻一年操作 */
     prevYear () {
     }
   
     /** @description 下翻一年操作 */
     nextYear () {
     }
   
     /**
      * @description 渲染函数，用户新建一个实例后必须使用此函数获取最新的日期数据，接收一个回调函数
      * @param {*} fn dom操作函数
      */
     render (fn) {
     }
   }
   ```

   实现效果（功能有但不限于这些，UI随意）：

   <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204161447144.png" alt="image-20220416144749054" style="zoom:50%;" />

   ​                                                                                                               或

   <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204161416451.png" alt="image-20220416141608293" style="zoom:40%;" />

2. 轮播图插件类(建议做)

   [参考](https://bytedance.feishu.cn/file/boxcn0vymHvT8flR0loNcQfZ4ne)

   三种写法：

   - [插件化](https://code.h5jun.com/weru/3/edit?html,css,js,output)

   - [模板化](https://code.h5jun.com/zuve/3/edit?html,css,js,output)

   - [抽象化](https://code.h5jun.com/vata/4/edit?html,css,js,output)

   里面有实现代码，希望大家去看懂它的实现

   交作业的时候，可以在一些地方写上自己的理解

## ref

[JS中的this详解](https://juejin.cn/post/6844903487721963533)

[JS 中 this 指向问题](https://juejin.cn/post/6946021671656488991)

[聊聊编程范式](https://juejin.cn/post/6935681018649116703)
