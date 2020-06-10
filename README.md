# Java_learn

#### 介绍
**使用docsify制作自己学习Java的记录**
本项目是基于docsify的文档生成器，记录自己学习Java的心路历程，最后形成文档，便于自己温故知新，巩固知识

#### 软件架构
使用docsify.js框架生成文档结构，通过配置导航与navbar，做好页面切换与语言国际化支持


#### 安装教程

1.  全局安装docsify-cli
```javascript
  npm i docsify-cli -g
```
按照一般写法，将文档写在./docs目录下
```javascript
  docsify init ./docs
```


2. 编写内容，init完成后，文档结构目录在./docs下，具体如下：
* index.html 文件入口
* README.md 首页
* .nojekyll 禁止Github Pages 忽略以下划线（_）开始命名的文件

#### 使用说明

启动项目，使用 docsify serve 命令，即可启动 http://localhost:3000,本地查看项目

#### 在线地址

可直接点击地址 [https://gruguy.github.io/Java_learn/#/](https://gruguy.github.io/Java_learn/#/ "点我来查看吧")


