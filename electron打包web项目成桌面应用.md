# electron打包web项目成桌面应用

[TOC]

[electron打包web应用成桌面项目](https://segmentfault.com/a/1190000010371811)

## 初步理解

### 简介

Electron 可以让你使用纯 JavaScript 调用丰富的原生 APIs 来创造桌面应用。你可以把它看作是专注于桌面应用而不是 web 服务器的，io.js 的一个变体。
这不意味着 Electron 是绑定了 GUI 库的 JavaScript。相反，Electron 使用 web 页面作为它的 GUI，所以你能把它看作成一个被 JavaScript 控制的，精简版的 Chromium 浏览器。

### 主进程

在 Electron 里，运行 package.json 里 main 脚本的进程被称为主进程。在主进程运行的脚本可以以创建 web 页面的形式展示 GUI。

### 渲染进程

由于 Electron 使用 Chromium 来展示页面，所以 Chromium 的多进程结构也被充分利用。每个 Electron 的页面都在运行着自己的进程，这样的进程我们称之为渲染进程。
在一般浏览器中，网页通常会在沙盒环境下运行，并且不允许访问原生资源。然而，Electron 用户拥有在网页中调用 io.js 的 APIs 的能力，可以与底层操作系统直接交互。

### 主进程与渲染进程的区别

主进程使用 BrowserWindow 实例创建网页。每个 BrowserWindow 实例都在自己的渲染进程里运行着一个网页。当一个 BrowserWindow 实例被销毁后，相应的渲染进程也会被终止。
主进程管理所有页面和与之对应的渲染进程。每个渲染进程都是相互独立的，并且只关心他们自己的网页。
由于在网页里管理原生 GUI 资源是非常危险而且容易造成资源泄露，所以在网页面调用 GUI 相关的 APIs 是不被允许的。如果你想在网页里使用 GUI 操作，其对应的渲染进程必须与主进程进行通讯，请求主进程进行相关的 GUI 操作。
在 Electron，我们提供用于在主进程与渲染进程之间通讯的 [ipc](https://github.com/electron/electron/blob/master/docs-translations/zh-CN/api/ipc-main-process.md) 模块。并且也有一个远程进程调用风格的通讯模块 [remote](https://www.w3cschool.cn/electronmanual/electronmanual-remote.html)。

## 创建 Electron 应用

### 目录结构

```
你的项目目录/
├── package.json
├── main.js
└── index.html

```

### 1.1 创建app目录，在目录中新建index.html及main.js文件（暂时不考虑renderer.js）

#### index.html文件示例：

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    <!-- All of the Node.js APIs are available in this renderer process. -->
    We are using Node.js <script>document.write(process.versions.node)</script>,
    Chromium <script>document.write(process.versions.chrome)</script>,
    and Electron <script>document.write(process.versions.electron)</script>.
  </body>

  <script>
     //require('./renderer.js')
  </script>
</html>

```

#### main.js文件示例：

```javascript
// var app = require('app');  // 控制应用生命周期的模块。
// var BrowserWindow = require('browser-window');  // 创建原生浏览器窗口的模块

const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

// 保持一个对于 window 对象的全局引用，不然，当 JavaScript 被 GC，
// window 会被自动地关闭
var mainWindow = null;

// 当所有窗口被关闭了，退出。
app.on('window-all-closed', function() {
  // 在 OS X 上，通常用户在明确地按下 Cmd + Q 之前
  // 应用会保持活动状态
  if (process.platform != 'darwin') {
    app.quit();
  }
});

// 当 Electron 完成了初始化并且准备创建浏览器窗口的时候
// 这个方法就被调用
app.on('ready', function() {
  // 创建浏览器窗口。
  mainWindow = new BrowserWindow({width: 800, height: 600});

  // 加载应用的 index.html
  mainWindow.loadURL('file://' + __dirname + '/index.html');

  // 打开开发工具
  mainWindow.openDevTools();

  // 当 window 被关闭，这个事件会被发出
  mainWindow.on('closed', function() {
    // 取消引用 window 对象，如果你的应用支持多窗口的话，
    // 通常会把多个 window 对象存放在一个数组里面，
    // 但这次不是。
    mainWindow = null;
  });
});
```

### 1.2 创建配置文件package.json ( npm init -y )

package.json的格式和 Node 的完全一致，并且那个被 main 字段声明的脚本文件是你的应用的启动脚本，它运行在主进程上。你应用里的 package.json 看起来应该像：

```javascript
{
  "name"    : "app",
  "version" : "1.0.1",
  "main"    : "main.js"
}
```

## 运行 Electron 应用

#### 安装electron-prebuilt运行项目

```javascript
npm install electron-prebuilt -save
```

## Electron 应用打包

### 安装electron-packager

```javascript
npm install electron-packager --save-dev
```

### 打包

```
electron-packager ./ HelloWorld --all --out ./outApp --electronVersion 3.0.10 --overwrite --overwrite --ignore=node_modules
```

electron-package .  (生成exe文件的名字)  --win  --out  (打包完文件夹的名字)  -arch=×64  --electronVersion      (electron的版本号)    --overwrite  --ignore=node_modules即可完成打包

### 注意事项

​    1.输入打包命令时，千万要注意所安装的electron的版本。

​    \2. .后面加生成exe文件的名字要加空格

​    3.--electronVersion很多经验文章都写成了--version导致解压报错