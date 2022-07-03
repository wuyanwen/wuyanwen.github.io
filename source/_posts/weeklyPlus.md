---
title: 从零开始使用vue搭建带有SSR服务端渲染的前后台项目weeklyPlus
date: 2017-08-30 09:20:29
tags: [nuxt,vue-cli,mongdb]
---

## 项目描述
使用 vue-cli 可以快速创建 vue 项目，使用 nuxt 可以快速创建 带有SSR服务端渲染的vue 项目，vue-cli很好用，nuxt也很好用，但是在最初搭建环境安装vue-cli及nuxt相关内容的时候，对一些人来说是很头疼的一件事情，特此写了一篇搭建环境的教程，每一步尽量详细解析。需要的朋友可以过来参考下，同时本项目也是vue中文社区周刊升级版实战项目，喜欢的可以到github点波star，希望可以帮到大家。
weeklyPlus vue中文社区周刊服务端渲染升级版weeklyPlus
[github源码下载地址：https://github.com/wuyanwen/weeklyPlus](https://github.com/wuyanwen/weeklyPlus) - 欢迎star
<!--more-->
## 引言

* 为什么要做一个这样的weeklyPlus系统?
* 可以做为知识的沉淀
* 可以熟悉最前沿且不仅限于前端的最新知识
* 可以传播到更多的需要这些知识的同学们
* 圈内已经有很多weekly系统了，weeklyPlus有什么不同吗?
* 我们保证是有态度（不求多，只求精）的周刊
* 我们保证发的每一篇文章都是有意义的
* 我们是open source ，欢迎每一位同学添砖加瓦

## 技术栈
* [nuxt](https://github.com/nuxt/nuxt.js) - vue 服务端渲染框架
* [vue-cli](https://github.com/vuejs/vue-cli) - Vue.js 提供的官方命令行工具，可用于快速搭建大型单页应用
* [mongodb](https://www.mongodb.com/) - 数据库
* [koa](http://koajs.com/) -  公开 API 接口
* [element-ui](http://element.eleme.io/#/zh-CN) - 基础组件库
* [axios](https://github.com/mzabriskie/axios) - http 请求
* [less](http://lesscss.org) -  一种动态样式语言
 web 应用
## 功能

* 周刊系统列表
* 周刊后台权限控制
* 周刊后台登录功能
* 周刊后台分类增删改查功能
* 周刊后台内容增删改查功能


## 开发部署
### 1.安装node.js

在node.js中文官网正常下载安装node.js就可以，没有什么特别需要注意的点（傻瓜式安装）。

在官网下载安装node.js后，就已经自带npm（包管理工具），不需要另外再进行安装npm了。

注意下载node.js版本最好要在node7.6以上，避免版本过低影响使用。这里强烈推荐大家安装node版本管理工具nvm。

打开命令行工具（随便哪个文件夹），输入命令行 node -v，npm -v，如果出现相应的版本号，则说明安装成功。

### 2.运行weekplus项目
本项目用的数据库是mongdb,开启mongdb,端口设为10086，强烈建议大家安装mongdb的客户端数据MongoChef可视化管理数据。
``` bash
# 1. 切换到server文件夹,依次执行以下命令
$ npm install 
$ npm run start

# 2. 切换到admin文件夹,依次执行以下命令
$ npm install 
$ npm run start

# 3. 切回到项目根目录,
$ npm install 
$ npm run dev

```
##  注意
*  mongdb 数据库启动的服务端号是10086
*  server 文件夹启动的服务端号是1111
*  admin  文件夹启动的服务端号是2222,后台入口默认地址 http://localhost:2222/#/admin
*   前台默认启动的服务端口号是3333,前台入口默认地址 http://localhost:3333/

## 最后

* 欢迎 issue 和 pr
* 欢迎加入前端实战交流QQ群: 541024234


