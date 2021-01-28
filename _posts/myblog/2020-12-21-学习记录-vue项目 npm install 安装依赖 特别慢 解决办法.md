---
layout: post
title: Vue 安装慢-解决
date: 2020-12-21 11:33:13
categories: Vue
tag: Vue 安装项目
---





使用NPM（[Node.js](http://lib.csdn.net/base/nodejs)包管理工具）安装依赖时速度特别慢，为了安装Express，执行命令后两个多小时都没安装成功，最后只能取消安装，笔者20M带宽，应该不是我网络的原因，后来在网上找了好久才找到一种最佳解决办法，

在安装时可以手动指定从哪个镜像服务器获取资源，我们可以使用阿里巴巴在国内的镜像服务器，命令如下：

```
npm install -gd express --registry=http://registry.npm.taobao.org

npm install --registry=https://registry.npm.taobao.org  --disturl=https://npm.taobao.org/mirrors/node
```

只需要使用–registry参数指定镜像服务器地址，为了避免每次安装都需要`--registry`参数，可以使用如下命令进行永久设置：

```
npm config set registry http://registry.npm.taobao.org
```

换了国内镜像，安装速度就很快了。

---

网上解决办法都是用淘宝镜像，但我先切换了镜像，安装还是慢，最后发现了一个比较快的方案。
打开cmd
先装一个cnpm，指向淘宝npm仓库

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
1
```

再安装vue cli

```
npm install -g vue-cli
1
```

补充：安装vue cli前要先安装node.js
安装成功后创建vue项目，使用”vue create 项目名“可能遇到版本不够问题，需要卸载重装cli

卸载：

```
npm uninstall -g vue-cli
```





---

---


## 个人信息：
### 姓名：邹建
### 19年加入 四川省装备制造机器人应用技术工程实验室，Dream Studio 软件工作室，负责软件开发相关工作
### 擅长：Unity游戏开发，Unity机器人仿真，C#编程语言，工业软件开发
### QQ：451991189
### B站ID：UnitySir
### 个人博客：
### [UnitySir - github.io](https://unitysir.github.io/)
### [UnitySir - 博客园 (cnblogs.com)](https://www.cnblogs.com/unitysir/)
### [- UnitySir (gitee.io)](https://unitysir.gitee.io/)
### [UnitySir (bilibili)](https://space.bilibili.com/308511666)
### B站ID：UnitySir

## 如果内容对你有所帮助：
<img src="https://pic4.zhimg.com/v2-87fbc8ee6ab3fd92f423d414d039b627_b.jpeg" width="300px"/>
<img src="https://pic2.zhimg.com/v2-b8ab4acf7899b2ced11287cdbd8279b5_b.jpeg" width="300px"/>

---