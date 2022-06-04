---
title: Web前端第四次课
date: 2022-1-17 16:53:50
categories: 
  - redrock
tags: 
  - JavaScript
  - redrock
description: 2021Web前端第四次课课件
top_img: https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171959549.JPG
cover: https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202204031611191.png
---

# Web前端第四次课

## 前言

### 几个概念的区分

- **什么是JS？**
  **JavaScript (** **JS** ) 是一种具有**函数优先**的**轻量级**，**解释型**的**编程语言**

- **什么是ES？**
  JavaScript 最开始由Netscape公司开发。1996年11月，Netscape公司将JavaScript 的标准制定工作交给 ECMA（欧洲计算机制造商协会），希望这种语言能够成为国际标准。次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript，这个版本就是1.0版。所以 **ECMAScript** （ES）就是 **JS** 的**标准**，类似于C语言的C99，教科书的教学大纲。

  - **为什么叫ECMAScript**

    有两个原因。一是商标，Java是Sun公司的商标，根据授权协议，只有Netscape公司可以合法地使用JavaScript这个名字，且JavaScript本身也已经被Netscape公司注册为商标。二是想体现这门语言的制定者是ECMA，不是Netscape，这样有利于保证这门语言的开放性和中立性。

    因此，**ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现**（Jscript和ActionScript也是ECMAScript的两种实现）。日常场合，这两个词是可以互换的。

  - **ES6**

    ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。
  
  更多具体内容可以点击查看这个[链接](https://es6.ruanyifeng.com/#docs/intro)和红宝书的开头。

- **什么是 xxx.js？**
  **xxx.js**几乎都是JS的框架（除了 node.js 之类的**运行环境**）

- **JS能做什么？**
  事实上JS最初发明出来是为了快速相应用户的操作，通俗的来说就是搞一些弹窗，弹一些广告之类的。但是随着JS，特别是node.js的出现之后，可以说是前端领域的**大爆炸（big bong）**，做前端的才被真正的称为前端工程师，而不被别人叫做做页面，和视觉一起的的**切图仔**。从上面JS能在哪里运行就可以窥探JS能做的远不止弹广告，所以可以说学好JS，对前端来说非常重要。



## [类型转换](https://zh.javascript.info/type-conversions)

### 字符串转换

- 显式的类型转换  **String(value)**

  ```js
  let value = true;
  console.log(typeof value); // boolean
  
  value = String(value); // 现在，值是一个字符串形式的 "true"
  console.log(value, typeof value); // string
  ```

- 隐式的类型转换  **value+''**

  ```js
  let value = true;
  console.log(typeof value); // boolean  
  value = value + ''; // 现在，值是一个字符串形式的 "true" 
  console.log(value, typeof value); // string
  ```

### 数字型转换

- 显示转换  **Number(value) 和 parseInt(string[, redix])** 

  ```javascript
  let str = "123";
  alert(typeof str); // string
  
  let num1 = Number(str); // 变成 number 类型 123
  alert(typeof num1); // number
  
  let num2 = parseInt(str); // 变成 number 类型 123
  alert(typeof num2); // number
  ```

- 隐式转换

  ```js
  console.log( "6" / "2" ); // 3, string 类型的值被自动转换成 number 类型后进行计算
  console.log('6' - '2'); // 4, string 类型的值被自动转换成 number 类型后进行计算
  ```

  number 类型转换规则：

  | 值              | 变成……                                                       |
  | --------------- | ------------------------------------------------------------ |
  | `undefined`     | `NaN`                                                        |
  | `null`          | `0`                                                          |
  | `true 和 false` | `1` and `0`                                                  |
  | `string`        | 去掉首尾空格后的纯数字字符串中含有的数字。如果剩余字符串为空，则转换结果为 `0`。否则，将会从剩余字符串中“读取”数字。当类型转换出现 error 时返回 `NaN` |

- 加号 ‘+’ 连接字符串

  **几乎所有的算术运算符都将值转换为数字进行运算**，加号 `+` 是个**例外**。如果其中一个运算元是字符串，则另一个也会被转换为字符串。 
    **这仅仅发生在至少其中一方为字符串的情况下**。否则值会被转换为数字。

  ```js
    console.log(1 + '2'); // '12' (字符串在加号右边)
    console.log('1' + 2); // '12' (字符串在加号左边)
    console.log(true + 2); // 3
  ```

### 布尔型转换

转换规则如下：

- 直观上为“空”的值（如 `0`、空字符串、`null`、`undefined` 和 `NaN`）将变为 `false`。
- 其他值变成 `true`。

举个🌰：

```javascript
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("hello")); // true
console.log(Boolean("")); // false
console.log(Boolean(" ")); // true
console.log(Boolean("0")); // true
```



## [字符串](https://zh.javascript.info/string)

上节课只是简单介绍了字符串，字符串也有一些API：

```js
// 改变大小写
console.log( 'Interface'.toUpperCase() ); // INTERFACE
console.log( 'InTerFace'.toLowerCase() ); // interface
console.log( 'Interface'[0].toLowerCase() ); // 'i'
```

```js
// 查找字符串
let str = 'Widget with id';

console.log(str.indexOf('Widget')); // 0，因为 'Widget' 一开始就被找到
console.log(str.indexOf("id")); // 1，"id" 在位置 1 处（Widget中有'id'）
console.log(str.indexOf('id', 2)); // 12
console.log(str.lastIndexOf('id')); // 12
console.log(str.includes("Widget")); // true
console.log(str.includes("id", 3)); // true
console.log(str.startsWith("Wid")); // true，"Widget" 以 "Wid" 开始
console.log(str.endsWith("id")); // true，"Widget" 以 "id" 结束
```

```js
// 获取字符串
let str = 'Widget with id';
console.log( str.slice(0, 5) ); // 'Widge'，从 0 到 5 的子字符串（不包括 5）
```



## 函数

### [函数表达式 vs 函数声明](https://zh.javascript.info/function-expressions#han-shu-biao-da-shi-vs-han-shu-sheng-ming)

这些是上次课讲到的创建函数的其中两种方法：

```JS
// 函数声明
function test(){
}

// 函数表达式
const test = function(){
};
```

现在有个🌰：

```javascript
sayHi("John"); // Hello, John
function sayHi(name) {
  console.log(`Hello, ${name}`);
}

sayHi("John"); // error!
let sayHi = function(name) {  // (*) no magic any more
  console.log(`Hello, ${name}`);
};
```

可能会有同学疑惑，为什么用函数声明的可以执行，而用函数表达式的却不可以执行？

那么我们应该先思考这个问题：**函数声明**和**函数表达式**有什么区别？

**函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用。**

一旦代码执行到赋值表达式 `let sayHi = function(name) ...` 的右侧，此时就会开始创建该函数，并且从现在开始才可以使用（分配，调用等）。

**而函数声明被定义之前，它就可以被调用。**

例如，一个全局函数声明对整个脚本来说都是可见的，无论它被写在这个脚本的哪个位置。

当JavaScript**准备**运行脚本时，首先会在脚本中寻找全局函数声明，并创建这些函数。在处理完所有函数声明后，才会开始执行代码。

所以，**在函数声明被定义之前，它就可以被调用。**

如果能够理解二者区别的话，那么也就不难理解上面的运行结果了。

### [箭头函数][https://zh.javascript.info/arrow-functions-basics]

创建函数还有另外一种非常简单的语法，并且这种方法通常比函数表达式更好，就是**箭头函数**。

举个🌰：

```js
// 我们现在有这样的一个函数表达式
const sum = function(a, b) {
    return a + b;
}
console.log(sum(1, 2)) // 3


// 事实上我们拥有一个简单并且减少很多麻烦的函数定义方式--箭头函数
const sum = (a, b) => {
    return a + b;
}
console.log(sum(1, 2))
```

你以为就这？还可以更加精简！！！

- 如果我们**只有一个参数，还可以省略掉参数外的圆括号**，使代码更短。

  ```js
  const add = a =>{
      return ++a;
  }
  console.log(add(1)); // 2
  ```

  但是需要注意，如果**没有参数的话是不能省略圆括号的**（达咩）

  ```js
  const func =  =>{ 
      return 1;
  }
  console.log(func()); // error
  
  
  const func = () =>{
      return 1;
  }
  console.log(func()); // 1
  ```

- 在函数只有一行的时候我们可以将 `{}` 和 `return` 去掉

  ```js
  let sum = (a, b) => a + b; // 计算 a+b , 并返回 a+b 的结果
  console.log(1,2) // 3
  ```

  ```js
  const func = ()=>console.log('钰钰姐姐');
  func()
  // console.log(func()) // 需注意这里会有返回值
  ```

是不是非常简洁呀！

尤其在调用一些方法当中体现的更加明显，这里再扔几个🌰：

```js
// 正常函数写法
let arr = [1,2,3].map(function (x) {
  return x * x;
});
console.log(arr);

// 箭头函数写法
let arr = [1,2,3].map(x => x * x);
console.log(arr);
```

```js
const numbers = (...nums) => nums;

numbers(1, 2, 3, 4, 5)
// [1,2,3,4,5]
```

最后再整一波骚操作

```js
let name = prompt('Who is the god of FrontEnd?')
showChoice = name === 'lomirus' ? () => alert('You are right!!!') : () => alert('Wrong!!!')
showChoice()
```

当然箭头函数没有 `this`，没有 `arguments`，不能使用 `new` 进行调用，也没有 `super`（但目前我们还没有学到它们）。

### 默认值

```js
const showMessage = (from, text = "没有给信息") => {
  console.log( from + ": " + text );
}
showMessage("Annie"); // Annie: 没有给信息
```

### Rest 参数

在 JavaScript 中，无论函数是如何定义的，你都可以使用任意数量的参数调用函数。

例如：

```javascript
function sum(a, b) {
  return a + b;
}

alert(sum(1, 2, 3, 4, 5));
```

虽然这里不会因为传入“过多”的参数而报错。但是当然，在结果中只有前两个参数被计算进去了。

Rest 参数可以通过使用三个点 `...` 并在后面跟着包含剩余参数的数组名称，来将它们包含在函数定义中。这些点的字面意思是“将剩余参数收集到一个数组中”。

例如，我们需要把所有的参数都放到数组 `args` 中：

```js
let add =  (...args) => args.reduce((previous,num)=>num+previous)
console.log(add(1,2,3,4,5)); // 15

// 复杂的写法
let add = (...args) =>{
    let sum = args.reduce((previous, num)=>{
        return previous + num;
    }, 0) 
    return sum;
}
// previous num
//    0      1
//    1      2
//    3      3
//    6      4
//    10     5
//    15
```

当然也可以只要后面几个参数被收集到数组中：

```js
const showName = (firstName, lastName, ...titles) => { // 剩余的参数会被放入 titles 数组中
    console.log(`${firstName}-${lastName}`); // 露-姐姐
    // console.log( firstName + '-' + lastName ); // 露-姐姐
    console.log( titles[1] ); // 还是神
    console.log( titles.length ); // 3
  }

showName('露', '姐姐', '神', '还是神', '永远滴神');
```

**注意：Rest 参数必须放到参数列表的末尾**

### [arguments](https://zh.javascript.info/rest-parameters-spread#arguments-bian-liang)

 `Rest参数`是ES6新增的语法。在ES6以前，使用 `arguments` 是获取函数所有参数的唯一方法。现在它仍然有效，我们可以在一些老代码里找到它。

 `arguments` 是一个特殊的**类数组对象**，该对象按参数索引包含所有参数。

例如： 

```javascript
function showName() {
  console.log( arguments.length );
  console.log( arguments[0] );
  console.log( arguments[1] );

  // 它是可遍历的
  // for(let arg of arguments) alert(arg);
}

// 依次显示：2，Julius，Caesar
showName("Julius", "Caesar");

// 依次显示：1，Ilya，undefined（没有第二个参数）
showName("Ilya");
```

但缺点是，尽管 `arguments` 是一个类数组，也是可迭代对象，但它终究不是数组。它不支持数组方法，因此我们不能调用 `arguments.map(...)` 等方法。不过，我们可以利用Array.from()方法将arguments转为数组。除此之外，它始终包含所有参数，我们不能像使用 `Rest 参数`那样只截取入参的一部分。

因此，当我们需要这些功能时，最好使用 `Rest 参数`。

### [递归](https://zh.javascript.info/recursion)

如果你不是刚接触编程，那么你可能已经很熟悉它了，那么你可以跳过。

递归是一种编程模式。

有时候，一个任务可以自然地拆分成**多个相同类型但更简单**的任务。又或者有的时候，一个任务可以简化为**一个简单的行为加上该任务的一个更简单的变体**。那么这个时候，我们可以使用递归。

当一个函数解决一个任务时，在解决的过程中它可以调用很多其它函数。在部分情况下，函数会调用 **自身**。这就是所谓的 **递归**。

举个🌰：

简单起见，让我们写一个函数 `pow(x, n)`，它可以计算 `x` 的 `n` 次方。换句话说就是，`x` 乘以自身 `n` 次。

```javascript
pow(2, 2) = 4
pow(2, 3) = 8
pow(2, 4) = 16
```

- 使用for循环：

  ```js
  function pow(x, n) {
    let result = 1;
  
    // 在循环中，用 x 乘以 result n 次
    for (let i = 0; i < n; i++) {
      result *= x;
    }
  
    return result;
  }
  
  console.log(pow(2, 3)); // 8
  ```

- 使用递归：

  ```js
  function pow(x, n) {
    if (n == 1)
      return x;
    else
      return x * pow(x, n - 1);
  }
  
  console.log(pow(2, 3)); // 8
  ```

  我们可以理解成pow(2, 3) =2 * 2 * 2  = pow(2, 2) * 2 , 而pow(2, 2) = pow(2, 1) * 2, 而pow(2, 1) = 2 

```js
// 上节课lv2的作业的一种实现方法
// 利用 reduce, ...拓展运算符, isArray, 递归

const arr = [1, [2, [3, [4, 5]]], 6];
let myFlat = (arr) => {
    return arr.reduce((pre,cur) =>{
       if(Array.isArray(cur)=== true){
           return [...pre,...myFlat(cur)]
       }else{
           return [...pre, cur]
       }
    },[])
}

// 简化版
let myFlat = (arr) => arr.reduce((pre,cur) => Array.isArray(cur)=== true? [...pre,...myFlat(cur)]: [...pre, cur],[])
console.log(myFlat(arr));
```

## [Spread语法](https://zh.javascript.info/rest-parameters-spread#spread-syntax)

我们刚刚看到了如何从参数列表中获取数组（Rest参数）。

不过有时候我们也需要做与之相反的事儿。

例如，内建函数 Math.max会返回参数中最大的值：

```javascript
console.log( Math.max(3, 5, 1) ); // 5
```

假如我们有一个数组 `[3, 5, 1]`，我们该如何用它调用 `Math.max` 呢？

毫无疑问，我们不可能这样写：

```js
let arr =  [2,5,1]
Math.max(arr[0], arr[1],arr[2])
```

这样的写法太蠢了。再者如果有100个数字呢？我们不可能一个个去写。

这时候**Spread 语法** 来帮助你了！它看起来和 rest 参数很像，也使用 `...`，但是二者的用途**完全相反**。

当在函数调用中使用 `...arr` 时，它会把可迭代对象 `arr` “展开”到参数列表中。

以 `Math.max` 为例：

```javascript
let arr = [3, 5, 1];
console.log(Math.max(...arr)); // 5（spread 语法把数组转换为参数列表）
// 相当于
// console.log(Math.max(3, 5, 1));
```

可以看到，确实方便了许多！

上面只是简单地引出了Spread语法，下面还有更多应用🌰：

- **复制数组:**

  ```js
  let arr1 = [1,2,3,4];
  let arr2 = [...arr1];
  
  console.log(arr1); // [ 1, 2, 3, 4 ]
  console.log(arr2); // [ 1, 2, 3, 4 ]
  ```

- **合并数组：**

  ```js
  let more = [3,4,5];
  
  // ES5
  [1, 2].concat(more);
  
  // ES6
  [1, 2, ...more];
  ```

  ```js
  let arr1 = ['a', 'b'];
  let arr2 = ['c'];
  let arr3 = ['d', 'e'];
  
  // ES5的合并数组
  arr1.concat(arr2, arr3); // [ 'a', 'b', 'c', 'd', 'e' ]
  
  // ES6的合并数组
  [...arr1, ...arr2, ...arr3]; // [ 'a', 'b', 'c', 'd', 'e' ]
  [...arr1, ...arr2, ...arr3, 'f']; // [ 'a', 'b', 'c', 'd', 'e', 'f']
  ```

- **与解构赋值结合:**

  扩展运算符可以与解构赋值结合起来，用于生成数组。

  ```js
  let list = [1, 2, 3, 4, 5]
  
  // ES5
  let a = list[0], rest = list.slice(1)
  
  // ES6
  let [a, ...rest] = list
  console.log(a,rest); // 1 [ 2, 3, 4, 5 ]
  ```

  （解构赋值还没讲到，一会讲到）

- **分割字符串**

  ```js
  console.log([...'露露姐姐俺的超人']); //[ "露", "露", "姐", "姐", "俺", "的", "超", "人" ]
  ```

- **......**



## 对象

复习下对象的一些基本操作：

```js
let person = {
  name:'myy',
  age:100,
  say(){
      console.log('hello')
  }
}
console.log(person); // { name: 'myy', age: 100 }
console.log(person.name); // myy

person.gender = 'unknown';
console.log(person.gender); // unknown

let a = 'age';
console.log(person[a]); // 100

console.log('----------------');

for(key in person) {
  // key 是 person 中的键
  console.log(person[key]); // 输出键所对应的值
}
```

### 方法

通常我们会创建对象来表示真实世界中的实体，如用户和订单等：

```javascript
let user = {
  name: "John",
  age: 30
};
```

在现实世界中，用户是可以进行**操作**：从购物车中挑选某物、登录和注销等。所以对象本身应该是可以拥有行为的。

在 JavaScript 中，行为（action）由属性中的函数来表示。

作为对象属性的函数被称为**方法**。

```js
const user = {
  name: "John",
  age: 30,
  sayHi: function() { // 可以发现我们这里使用类似于“函数表达式”的方式来创建一个函数
    console.log("Hello");
	}
};
user.sayHi(); // Hello!

// 方法简写看起来更好，对吧？
const user = {
  name: "John",
  age: 30,
  sayHi() { // 与 "sayHi: function()" 一样
    console.log("Hello");
  }
};
user.sayHi(); // Hello!
```

> 注意：上面这两种创建函数的方法并不是在所有情况中表现是一样的

也可以这样添加方法：

```javascript
const user = {
  name: "John",
  age: 30
};

user.sayHi = function() { // 请注意我们这里使用的是 “函数表达式” 来创建一个函数
  console.log("Hello!");
};

user.sayHi(); // Hello!
```

### this

这时可能你会有疑问了❓ 我们怎么在对象方法中去访问到对象本身❓

相信你肯定想到了，比如上面的 user 定义之后应该在全局作用域中，我们无论在哪里都应该能访问到，在对象方法中也不例外。

```js
const user = {
  name: "John",
  age: 30,
  sayHi() {
    console.log(`我是：${user.name}`);
  }
};
user.sayHi(); // "John"
```

但是，事实上这样的代码是不可靠的。如下：

```js
const user1 = {
    name: "John",
    age: 30,
    sayHi() {
      console.log(`我是：${user1.name}`); // 导致错误
    }
};
const user2 = {
    name: "myy",
    age: 0,
}
user2.sayHi = user1.sayHi;
user2.sayHi(); // 我是：John, 叫错了
```

那我们怎么才能，方便的在对象的方法中访问对象本身？对，就是我们的标题 this

```javascript
const user = {
  name: "John",
  age: 30,
  sayHi() {
    console.log(`我是：${this.name}`);
  }
};
user.sayHi(); // 我是：John
```

```js
const user1 = {
    name: "John",
    age: 30,
    sayHi() {
      console.log(`我是：${this.name}`); // 导致错误
    }
};
const user2 = {
    name: "myy",
    age: 0,
}
user2.sayHi = user1.sayHi;
user2.sayHi(); // 我是：myy, 总算叫对了
```

请注意：它的值在**普通情况**(不是箭头函数)下**并不取决于方法声明的位置**，而是**（取决）于在『点之前』**的是什么对象。

```js
const user = { name: "John" };
const admin = { name: "Admin" };

const sayHi = function() {
  console.log(`我是：${this.name}`);
}

// 在两个对象中使用的是相同的函数
user.f = sayHi;
admin.f = sayHi;

// 它们调用时有不同的 this 值。
// 函数内部的 "this" 是 ***点之前*** 的这个对象。
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin（使用点或方括号语法来访问这个方法，都没有关系。）
```

this的指向问题一直是js里面一个比较难的点，由于时间限制，这里我讲的比较粗浅，大家可以多去看看相关的文章

这里给大家推荐下这篇文章：https://juejin.im/entry/6844903487721963533

### 引用

上节课我们讲到：

原始类型（字符串，数字，布尔值等）始终是以“整体值”的形式被复制的。

```javascript
//这里我们将 message 复制到 phrase
let message = "Hello!";
let phrase = message;
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171819330.png" alt="image-20220117181936060" style="zoom:50%;" />

每个都独立存储着字符串 `"Hello!"`。

**而对象不同。对象的变量存储的不是对象本身，而是该对象“在内存中的地址”，换句话说就是对该对象的“引用”。**

让我们看一个这样的变量的例子：

```javascript
let user = {
  name: "John"
};
```

这是它实际存储在内存中的方式：

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171819341.png" alt="image-20220117181952141" style="zoom:50%;" />

我们可以将对象变量（例如 `user`）想象成一张带有地址的纸。

当我们对对象执行操作时，例如获取一个属性 `user.name`，JavaScript 引擎将对纸上的地址进行搜索，并在实际对象上执行操作。

**当一个对象变量被复制：引用则被复制，而该对象并没有被复制。**

```javascript
let user = { name: "John" };
let admin = user; // 复制引用
```

现在我们有了两个变量，它们保存的都是对同一个对象的引用：

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171820055.png" alt="image-20220117182007967" style="zoom:50%;" />

```js
let user = { name: "John" };
let admin = user; // 复制引用

console.log(user === admin) // true

user.age = 100;
console.log(admin.age) // true
```

### 浅拷贝

我们会发现，如果我们仅仅是拷贝复制了一个对象，那么对这个对象的修改会影响到原对象的值，这是非常危险的一件事，如何能拷贝一个对象后，新的对象跟原对象无关联呢？

- 使用Object.assign

  ```js
  let user = { name: "John" };
  let user2 = {};
  Object.assign(user2, user);
  alert(user); // { name: "John" }
  ```

- 遍历对象

既然有浅拷贝，肯定有深拷贝呀！关于**深拷贝**，感兴趣可以点这个[链接](https://juejin.cn/post/6844903929705136141#heading-2)

### 基本类型的方法

你可能有这样的疑问，`String` 又不是对象为什么他能像对象方法那样去使用❓

```javascript
const str = 'Hello';
console.log(str.toUpperCase()); // HELLO
```

以下是 `str.toUpperCase()` 实际发生的情况：

1. 字符串 `str` 是一个**基本类型**。所以在访问**它的属性时**，会即刻**创建一个包含字符串字面值的特殊对象**，并且具有很多有用的内置方法，例如 `toUpperCase()`。

1. 该方法运行并返回一个新的字符串（由 `console.log` 显示）。

1. 特殊对象被销毁，只留下基本类型 `str`。

所以基本类型可以提供方法，但它们依然是轻量级的。



## 垃圾回收

对于开发者来说，JavaScript 的内存管理是自动的、无形的。我们创建的原始值、对象、函数……这一切都会占用内存。

当我们不再需要某个东西时会发生什么？JavaScript 引擎如何发现它并清理它？

### 可达性

JavaScript 中主要的内存管理概念是**可达性**。

简而言之，“可达”值是那些以某种方式可访问或可用的值。它们一定是存储在内存中的。

1. 这里列出固有的可达值的基本集合，这些值明显不能被释放。

   比方说：

   - **当前函数的局部变量和参数**
   - 嵌套调用时，当前**调用链上所有函数的变量与参数**
   - **全局变量**
   - （还有一些内部的）

   这些值被称作 **根（roots）**。

2. 如果一个值可以通过引用或引用链从根访问任何其他值，则认为该值是可达的。

   比方说，如果全局变量中有一个对象，并且该对象有一个属性引用了另一个对象，则 **该** 对象被认为是可达的。而且它引用的内容也是可达的。

这里是一个最简单的例子：

```javascript
// user 具有对这个对象的引用
let user = {
  name: "John"
};
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171820469.png" alt="image-20220117182026301" style="zoom:50%;" />

这里的箭头描述了一个对象引用。全局变量 `"user"` 引用了对象 `{name："John"}`。

如果 `user` 的值被重写了，这个引用就没了：

```javascript
user = null;
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171820492.png" alt="image-20220117182039333" style="zoom:50%;" />

现在 John 变成不可达的了。因为没有引用了，就不能访问到它了。垃圾回收器会认为它是垃圾数据并进行回收，然后释放内存。

现在，我们把 `user` 的引用复制给 `admin`：

```javascript
// user 具有对这个对象的引用
let user = {
  name: "John"
};

let admin = user;
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171820908.png" alt="image-20220117182053748" style="zoom:50%;" />

现在如果执行刚刚的那个操作：

```javascript
user = null;
```

 <img src="https://myyoss.oss-cn-shenzhen.aliyuncs.com/img/md/202201171821388.png" alt="image-20220117182107220" style="zoom:50%;" />

然后对象仍然可以被通过 `admin` 这个全局变量访问到，所以对象还在内存中。

------

在 JavaScript 引擎中有一个被称作 [垃圾回收器](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) 的东西在后台执行。它监控着所有对象的状态，并删除掉那些已经不可达的。



## [解构赋值](https://es6.ruanyifeng.com/#docs/destructuring)

当我们想把`Object` 和 `Array`传递给函数时，此时函数可能不需要一个整体的对象/数组，而是需要其中的一部分。这时候，解构赋值就派上用场了。

> ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
>

### 数组解构

```js
// 上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。
let [a, b, c] = [1, 2, 3];

//多层嵌套数组
let [foo, [[bar], baz]] = [1, [[2], 3]];

// 当我们不想要中间的元素时
let [x, , y] = [1, 2, 3];

// 当我们只想要前面的元素时
let [x, y] = [1, 2, 3];

// 如果取不存在的值呢
let [a, b] = [1]

// 如果没有的话，咱们给他一个默认值吧
let [a, b=2] = [1]

// 与扩展运算符结合使用
let [head, ...tail] = [1, 2, 3, 4];

// 交换变量的值（swap）
let x = 1;
let y = 2;
[x, y] = [y, x];
```

### 对象解构

解构不仅可以用于数组，还可以用于对象。只要变量与属性同名，就能取到正确的值。

```js
const person = {
    name: 'myy',
    address:{
        city: 'ChongQing',
        college: 'CQUPT'
    },
    title:['student', 'Chinese', 'five']
}

// 解构
const {age} = person;
console.log(age); // 20

// 可以很方便地将现有对象的方法，赋值到某个变量
const {log} = console;
log('hello') // hello
// 当然一般我们不会用在console这个对象上，后面的项目开发会用到

// 如果想要给变量改个名字时
const {name: userName} = person;
console.log(userName); // myy

// 这实际上说明，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。
let {age} = person;
// 是下面的简写
let {age: age} = person;

// 嵌套结构的对象
const {address: {city}} = person;
console.log(city); // ChongQing

const {title:[titleOne]} = person;
console.log(titleOne); //student
```

### 函数参数解构

```js
function add([x, y]){
  return x + y;
}
add([1, 2]); // 3

[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```



## Homework

- `lv0` (必做) : 通过解构赋值的方式获取对象中所有属性

  ```js
  const person = {
    name: '露露姐姐',
    age: 1000000,
    address: {
      city: 'ChongQing',
      area: 'NanShan'
    },
    title: ['student',{year:2021, title:'GoodStudent'}]
  }
  
  
  ...// 你的代码
  
  
  console.log(name) // 露露姐姐
  console.log(year) // 1000000 这里要把age改变量名为year
  console.log(city) // ChongQing
  console.log(mountain) // NanShan 这里要把area改变量名为mountain
  console.log(title1) // student
  console.log(title2) // GoodStudent
  ```

  其实上面课件已经写得很清楚，但是还是希望大家不要抄课件，要理解并自己写

- `lv1`  (必做) : 编写一个函数 `factorial(n)` 计算 n! 
  举个例子：
  
  ```
  factorial(1) = 1
  factorial(2) = 2 * 1 = 2
  factorial(3) = 3 * 2 * 1 = 6
  factorial(4) = 4 * 3 * 2 * 1 = 24
  ...
  factorial(10) = 10 * 9 * ... * 2 * 1 = 3628800
  ...
  ```
  
  注意用两种方式实现：
  
  1. 循环
  2. 递归
  
- `lv2` : 自行实现一个浅拷贝

- `lv3` : 实现一个深拷贝

  可以参考上面课件深拷贝的推荐文章，但要求写下自己的思考和实现过程（想偷懒写注释也可

11月12日晚23:59:59前将作业的GitHub链接发到mayingyi@redrock.team这个邮箱

标题格式: 第四次课-lv1(这里写自己做到的最高等级就行)-姓名-学号



## 一些推荐

系统学习ES6: https://es6.ruanyifeng.com/

js入门教程: https://zh.javascript.info/

