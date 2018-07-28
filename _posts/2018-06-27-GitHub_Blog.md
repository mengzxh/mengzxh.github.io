---
layout: post
title: 'GitHub Blog搭建过程'
subtitle: ''
date: 2018-6-27
categories: 技术
cover: '/cover_img/github.jpg'
tags: jekyll github 博客 域名
---

&emsp;&emsp;建立博客的初衷是为了督促自己学习，博客主要发布自己读书的相关笔记和工作心得。主要内容包括：Python基础知识笔记，Python爬虫框架，Python web， 大数据技术入门，机器学习入门基础。对博客内容感兴趣的可以收藏网站，也可以在每篇文章中进行评论留言。



本文主要介绍如何搭建自己的GitHub Pages个人主页.

基础知识和账号准备：

* Git基础入门（学习参考：[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)）并生成相关密钥

* [GitHub](https://github.com/)账号注册

相关介绍：
  GitHub Pages: GitHub Pages是一个静态站点托管服务，旨在直接从GitHub存储库托管您的个人，组织或项目页面。([官网英文版介绍](https://help.github.com/articles/what-is-github-pages/))

步骤介绍：

* 域名注册： 打开https://www.godaddy.com/，注册之后。搜索需要的域名，购买并付费。打开DNS域名管理，并设置www 为github ping出来的用户名。
* 在本地计算机安装Ruby程序，我的电脑是Windows10可以参考[安装教程](http://www.runoob.com/ruby/ruby-installation-windows.html)。(通过"ruby -v"命令判断是否安装成功和环境变量是否配置正确)
* 通过Ruby的gem工具安装jekyll，以管理员权限打开cmd，运行gem install jekyll
* 下载[Git](https://git-scm.com/downloads)工具并安装。
* 打开GitHub首页登陆，登陆后新建仓库，这里需要注意仓库的名称是自己账号.github.io，比如我的用户名是mengzxh，那么我的仓库名称:mengzxh.github.io。
* clone仓库到本地，复制刚刚新建里的地址，在git命令行工具中输入： git clone git@github.com:mengzxh/mengzxh.github.io.git(**请注意换成自己的仓库地址**)
* 寻找到你喜欢的对应的jekyll主题([参考官方主题](http://jekyllthemes.org/)),我这边选择的是$H_2O主题$，[GitHub链接地址](https://github.com/kaeyleo/jekyll-theme-H2O)，按照之前的操作git clone项目地址到本地，复制到自己的github.io对应文件夹中（需要排除.git隐藏文件夹）**这里也可以fork对应的项目然后改名为自己用户名.github.io**
* 根据主题的readme修改对应的配置后，在命令行窗口切换目录到mengzxh.github.io，运行jekyll serve,如果遇见报错cannot load such file -- bundler (LoadError) ，其中bundler指的是模块名称通过运行gem install 模块名称安装，运行之后就可以在本地打开127.0.0.1:4000的地址访问本地博客，修改完成之后可以提交了到github仓库了(git add . 和 git commit -m "自定义内容")，**不要忘记添加CNAME文件**。



##### 参考链接：
[GithubPages教程 在GithubPages上搭建个人主页](https://blog.csdn.net/yanzhenjie1003/article/details/51703370)

[Ruby 安装 - Windows](http://www.runoob.com/ruby/ruby-installation-windows.html)

[jekyll 完整安装教程](https://blog.csdn.net/joelcat/article/details/78642434)

[GoDaddy最新域名解析教程（中文版）](http://godaddy.idcspy.com/godaddy-jx)

[$H_2O主题$](https://github.com/kaeyleo/jekyll-theme-H2O)