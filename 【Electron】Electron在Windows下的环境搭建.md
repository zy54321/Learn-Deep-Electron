# 【Electron】Electron在Windows下的环境搭建

[TOC]

[【Electron】Electron在Windows下的环境搭建](https://www.cnblogs.com/phpCHAIN/p/6347044.html)

Electron作为一种用javascript写桌面程序的开发方式，现在已经被大众接受。下面就介绍如何在windows（>win7）下快速搭建Electron开发环境。

## **1. nodejs 的安装**

从nodejs 下载最新版本的windows安装程序进行安装，我下载的是v6.9.1，安装时一路默认即可，这个安装会把nodejs和npm配置到系统PATH中，这样在命令行的任何位置都可以直接用node执行nodejs，用npm执行npm命令。

检查nodejs是否安装成功可以这样查看：

![img](https://images2015.cnblogs.com/blog/516449/201701/516449-20170124140132628-1957311958.png)

另外，因为可能的GFW问题（不然会下载很慢很慢，也可能下载失败），所以建议把npm的仓库切换到国内taobao仓库，执行下面的命令：

```
npm config set registry "https://registry.npm.taobao.org/"
```

还有一种方式，注册cnpm命令，如下（我采用的是第二种）：

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

运行效果如下：

![img](https://images2015.cnblogs.com/blog/516449/201701/516449-20170124135845816-995282322.png)

## 2. Electron的安装

因为前面已经启用了node/npm，所以可以采用npm的方法安装Electron。
为了测试的方便，我是在命令行窗口中采用下面的命令:

```
npm install -g electron
```

效果如下：

![img](https://images2015.cnblogs.com/blog/516449/201701/516449-20170124141644222-364750319.png)

其中，查看electron是否安装成功可通过命令 electron -v 查看。

网上有的说命令是 npm install -g electron-prebuilt，其实这是早期的版本命令，现在已经更新为electron了，如下：

![img](https://images2015.cnblogs.com/blog/516449/201701/516449-20170124141540222-625199945.png)

## 3. 编程环境安装

强烈推荐微软退出的VS Code，直接下载安装即可，它支持nodejs等的代码提示，很是方便。

## 4. 打包输出工具

为了方便最终成果输出，建议安装electron-packager工具，安装也很简单，建议以下面的命令全局安装：

```
npm install -g electron-packager
```

效果如下：

![img](https://images2015.cnblogs.com/blog/516449/201701/516449-20170124142434066-978989726.png)

## 5. 如果需要采用git进行版本控制，建议安装git工具

直接在git 下载最新版本的安装即可。

 

至此实际上开发环境已经搭建好了。