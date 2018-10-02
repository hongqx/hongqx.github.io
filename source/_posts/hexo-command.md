---
title: 关于hexo的一些常用命令汇总
date: 2018-08-31 15:12:33
tags: 
- hexo
categories:
- fe
toc: true
---
最近使用hexo构建博客，将一些常用命令，以及自己使用的主题及主题配置本分说明下，[全面的hexo文档](https://hexo.io/zh-cn/docs/)

<!-- more -->

## 安装
```shell
$ npm install -g hexo
```

## 初始化
```shell
$ cd ./Workspaces/hexo/
$ hexo init
```
```shell
$ hexo g # 生成静态文件 等同于hexo generate 
$ hexo s # 启动服务 等同于hexo server
$ hexo d #完整命令为hexo deploy,将本地编译好的静态文件发布到github上
$ hexo n #完整命令为hexo new,新建一篇文章
$ hexo clean #清除当前项目的静态文件
```
### 端口和配置
hexo启动的时候默认端口号为4000，想要更改启动的端口号如下
```shell
$ hexo s -p 5000
```
如果不想每次都更改这个文件这么麻烦的话，可以在hexo的配置文件_config.yml中修改或增加这段代码
```shell 
# Server
server:
  port: 4000
  compress: true
  header: true

```
## 修改主题
### 拷贝主题
我使用的主题是yilia这个主题, [hexo-theme-yilia主题github地址](https://github.com/litten/hexo-theme-yilia.git),使用方法将改主题clone到themes之下
```shell
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
# 拷贝完成切换到主题目录下
$ cd themes 
# 能直接查看theme中多了一个yilia的目录，就是主题配置目录
$ ls -l 
```
### 启用主题
回到项目的主目录，会看到有_config.yml这样一个文件，打开该文件，将配置中的theme修改为你先使用的主题名称
```shell
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: yilia
```
然后重新编译静态文件 启动服务,就能通过http://localhost:4000, 在浏览器中看到自己的博客了
```shell
$ hexo clean
$ hexo g
$ hexo s
```
### 主题自定义配置
```css
.article-entry .highlight, .article-entry pre {
  background: linear-gradient(to bottom, #192224 0%, #21728b 100%);
  font-family: Menlo,Monaco,Consolas,"Courier New",monospace;
  font-style: italic;
  color:#a9b7c6
}
pre .class .params, pre .function .keyword, pre .keyword {
  color: #e8bf6a;
}
pre .string, pre .value{
  color: #6a8759;
}
pre .comment{
  color: #a9b7c6;
}

function name : #268bd2
number #6897bb

main #d7ecff
bg-pattern {
    background: url(../img/bg-pattern.png) repeat;
    opacity: 0.5;
    width: 100vw;
    height: 100vh;
    position: fixed;
    z-index: -1;
}
```