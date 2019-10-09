---
title: yarn介绍
date: 2017-08-31 15:47:28
categories:
- javascript
tags:
- js
- node
- npm
- yarn
---
>[yarn](https://github.com/yarnpkg/yarn) - https://github.com/yarnpkg/yarn

## 一、为什么需要yarn
   作为一个前端，说到包管理首先想到的可能是node里的npm。至今为止，它可以访问在npm注册的 300,000 多个安装包。超出500万的工程师使用npm注册，每个月的下载量高达 50 亿<!-- more -->但是随着npm的普遍过程中，有一些问题也随之暴露出来：

1. 当跨机器或用户安装依赖时，拉取依赖包消耗的时间较长（依赖网络）。
2. 在版本管理没用锁定的情况下一个小升级就有可能导致项目出错。
3. 在协作开发时，npm安装依赖到  node_modules 目录时顺序不是固定的，不能保证不同终端中的开发环境一致。
4. node_modules里的依赖有很多重复的，即占用了硬盘资源，又拉长了拉取的时间和网络资源。
所以yarn诞生了。

## 二、yarn简介
facebook于 2016-10-12 开源的javascript包管理工具 Yarn，开源三天star数就超过了npm。Yarn 作为一个新的包管理器，用于替代现有的 npm 客户端或者其他兼容 npm 仓库的包管理工具。Yarn 保留了现有工作流的特性，优点是更快、更安全、更可靠。

>新的特性：
>
1. 离线模式（Offline Mode）：如果你之前安装过某个包，那么你可以在没有互联网连接的情况下，对这个包进行重新安装。
2. 确定性（Deterministic）：不管安装顺序如何，相同的依赖在每台机器上会以完全相同的方式进行安装。
3. 网络性能：Yarn会对请求进行高效地排队，避免出现请求瀑布（waterfall），便于将网络的使用效率达到最大化。
4. 网络弹性（Network Resilience）：单个请求的失败不会导致整个安装的失败，请求会基于故障进行重试。
5. 扁平模式（Flat Mode）：将不匹配的依赖版本都会解析为同一个版本，避免重复创建。

总结一下就是：兼容npm、缓存和离线下载、yarn.lock锁定版本与安装顺序

## 三、yarn的使用
安装
**macOS**
官方推荐的是homebrew：
```
brew install yarn
```
或者用npm也可以：
```
npm install -g yarn
```
**windows**
下载.msi文件安装 ( [下载传送门](https://yarnpkg.com/latest.msi) )，不过需要先装个node ( [下载传送门](https://nodejs.org/) ). 
**Linux**
apt-get:
```
sudo apt-get update && sudo apt-get install yarn
```
yum:
```
sudo yum install yarn
```
and so on...

简单指令

npm | yarn | 注释
--- | --- | ---
npm init | yarn init |
npm install | yarn / yarn install |
npm install xxx —save | yarn add xxx |  yarn add [package]@[version]
npm uninstall xxx —save | yarn remove xxx |
npm update |  yarn upgrade | yarn upgrade [package]@[version]
npm install xxx -g | yarn global add xxx |

## 结尾
如果你只是想本地改一下依赖就用npm好了，yarn每次更改好像都会更改.lock文件。如果并不想锁定版本用npm也是一种可接受方案。



