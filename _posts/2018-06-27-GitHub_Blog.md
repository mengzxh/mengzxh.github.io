---
layout: post
title: 'GitHub Blog搭建过程'
subtitle: ''
date: 2018-6-27
categories: 技术
cover: '/cover_img/material-5.png'
tags: jekyll github 博客 域名
---

&emsp;&emsp;建立博客的初衷是为了督促自己学习，博客主要发布自己读书的相关笔记和工作心得。主要内容包括：Python基础知识笔记，Python爬虫框架，Python web， 大数据技术入门，机器学习入门基础。对博客内容感兴趣的可以收藏网站，也可以在每篇文章中进行评论留言。



本文主要介绍如何搭建自己的GitHub Pages个人主页.

#### 基础知识和账号准备：

* Git基础入门（学习参考：[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)）并生成相关密钥

* [GitHub](https://github.com/)账号注册

#### 相关介绍：
  GitHub Pages: GitHub Pages是一个静态站点托管服务，旨在直接从GitHub存储库托管您的个人，组织或项目页面。([官网英文版介绍](https://help.github.com/articles/what-is-github-pages/))

#### 步骤介绍：

* 域名注册： 打开https://www.godaddy.com/，注册之后。搜索需要的域名，购买并付费。打开DNS域名管理，并设置www 为github ping出来的用户名。
* 在本地计算机安装Ruby程序，我的电脑是Windows10可以参考[安装教程](http://www.runoob.com/ruby/ruby-installation-windows.html)。(通过"ruby -v"命令判断是否安装成功和环境变量是否配置正确)
* 通过Ruby的gem工具安装jekyll，以管理员权限打开cmd，运行gem install jekyll
* 下载[Git](https://git-scm.com/downloads)工具并安装。
* 打开GitHub首页登陆，登陆后新建仓库，这里需要注意仓库的名称是自己账号.github.io，比如我的用户名是mengzxh，那么我的仓库名称:mengzxh.github.io。
* clone仓库到本地，复制刚刚新建里的地址，在git命令行工具中输入： git clone git@github.com:mengzxh/mengzxh.github.io.git(**请注意换成自己的仓库地址**)
* 寻找到你喜欢的对应的jekyll主题([参考官方主题](http://jekyllthemes.org/)),我这边选择的是$H_2O主题$，[GitHub链接地址](https://github.com/kaeyleo/jekyll-theme-H2O)，按照之前的操作git clone项目地址到本地，复制到自己的github.io对应文件夹中（需要排除.git隐藏文件夹）**这里也可以fork对应的项目然后改名为自己用户名.github.io**
* 根据主题的readme修改对应的配置后，在命令行窗口切换目录到mengzxh.github.io，运行jekyll serve,如果遇见报错cannot load such file -- bundler (LoadError) ，其中bundler指的是模块名称通过运行gem install 模块名称安装，运行之后就可以在本地打开127.0.0.1:4000的地址访问本地博客，修改完成之后可以提交了到github仓库了(git add . 和 git commit -m "自定义内容")，**不要忘记添加CNAME文件**。

#### 添加GA插件流程
* Google Analystic统计插件，用于分析网站流量
* 在_includes.footer.html文件中添加以下代码("UA-121330618-1"是user_id，需要根据自己的代码修改)

```html
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-121330618-1');
</script>
```

#### 添加阅读量的标签

* 申请并配置[LeanCloud](https://leancloud.cn/)
* 在_config.yml文件末尾添加以下代码(配置需要根据LeanCloud内容修改app_id和app_key)

```tex
leancloud: 
    enable: true 
    app_id: edcgBgescCWpVAgimGW1nbfL-123456 
    app_key: 2c30PCULFMUSgBVNOw12345 
```

* 在_includes文件夹中添加文件leancloud-analytics.html并写入下面的代码。需要调整leancloud-analytics.html文件var Counter = AV.Object.extend("blog_view_numbers")的"blog_view_numbers"为自己建立的表名，例如var Counter = AV.Object.extend("SheetName")

```html
{% if site.leancloud.enable %}
  <script src="https://code.jquery.com/jquery-3.2.0.min.js"></script>
  <script src="https://cdn1.lncld.net/static/js/av-core-mini-0.6.1.js"></script>
  <script>AV.initialize("{{site.leancloud.app_id}}", "{{site.leancloud.app_key}}");</script>
  <script>
    function showHitCount(Counter) {
      /* 这是给一个页面中有多篇文章时所调用的，例如博客主界面或者存档界面。
      */
      var query = new AV.Query(Counter);
      var entries = [];
      var $visitors = $(".leancloud_visitors");

      // 获取页面中所有文章的id（page.url）
      $visitors.each(function () {
        entries.push( $(this).attr("id").trim() );
      });
      // 批量查询
      query.containedIn('url', entries);
      query.find()
        .done(function (results) {
          var COUNT_CONTAINER_REF = '.leancloud-visitors-count';

          if (results.length === 0) {
            $visitors.find(COUNT_CONTAINER_REF).text(0);
            return;
          }

          for (var i = 0; i < results.length; i++) {
            var item = results[i];
            var url = item.get('url');
            var hits = item.get('hits');// 获取点击次数
            var element = document.getElementById(url);
            
            // 显示点击次数
            $(element).find(COUNT_CONTAINER_REF).text(hits);
          }
          for(var i = 0; i < entries.length; i++) {
            var url = entries[i];
            var element = document.getElementById(url);
            var countSpan = $(element).find(COUNT_CONTAINER_REF);
            if( countSpan.text() == '') {
              countSpan.text(0);
            }
          }
        })
        .fail(function (object, error) {
          console.log("Error: " + error.code + " " + error.message);
        });
    }

    function addCount(Counter) {
      // 页面（博客文章）中的信息：leancloud_visitors
      // id为page.url， data-flag-title为page.title
      var $visitors = $(".leancloud_visitors");
      var url = $visitors.attr('id').trim();
      var title = $visitors.attr('data-flag-title').trim();
      var query = new AV.Query(Counter);

      // 只根据文章的url查询LeanCloud服务器中的数据
      query.equalTo("url", url);
      query.find({
        success: function(results) {
          if (results.length > 0) {//说明LeanCloud中已经记录了这篇文章
            var counter = results[0];
            counter.fetchWhenSave(true);
            counter.increment("hits");// 将点击次数加1
            counter.save(null, {
              success: function(counter) {
                var $element = $(document.getElementById(url));
                $element.find('.leancloud-visitors-count').text(counter.get('hits'));
              },
              error: function(counter, error) {
                console.log('Failed to save Visitor num, with error message: ' + error.message);
              }
            });
          } else {
            // 执行这里，说明LeanCloud中还没有记录此文章
            var newcounter = new Counter();
            /* Set ACL */
            var acl = new AV.ACL();
            acl.setPublicReadAccess(true);
            acl.setPublicWriteAccess(true);
            newcounter.setACL(acl);
            /* End Set ACL */
            newcounter.set("title", title);// 把文章标题
            newcounter.set("url", url); // 文章url
            newcounter.set("hits", 1); // 初始点击次数：1次
            newcounter.save(null, { // 上传到LeanCloud服务器中
              success: function(newcounter) {
                var $element = $(document.getElementById(url));
                $element.find('.leancloud-visitors-count').text(newcounter.get('hits'));
              },
              error: function(newcounter, error) {
                console.log('Failed to create');
              }
            });
          }
        },
        error: function(error) {
          console.log('Error:' + error.code + " " + error.message);
        }
      });
    }

    $(function() {
      var Counter = AV.Object.extend("blog_view_numbers");
      if ($('.leancloud_visitors').length == 1) {
        // in post.html, so add 1 to hit counts
        addCount(Counter);
      } else if ($('.post-link').length > 1){
        // in index.html, there are many 'leancloud_visitors' and 'post-link', so just show hit counts.
        showHitCount(Counter);
      }
    });
  </script>
{% endif %}
```

* 在\_layouts.default.html和\_includes.post_head.html文件中添加如下代码

```html
<!-- 此处模板基层是用于统计文章浏览量 -->
{% include leancloud-analytics.html %}
```

* 在_layouts.post.html文件中添加如下代码

```html
<!-- 此处放置显示阅读量的模块 -->
{% if site.leancloud.enable %}
<span id="{{ page.url }}" class="leancloud_visitors" data-flag-title="{{ page.title }}">
    <i class="iconfont icon-search"></i>
    <!--<span class="post-meta-item-text"> views:  </span>-->
    <span class="leancloud-visitors-count" color="#fff"></span>
</span>
{% endif %}
```

* 在index.html文件中添加如下代码(需要根据html的格式进行调整)

```html
{% if site.leancloud.enable %}
<span id="{{ post.url }}" class="leancloud_visitors" data-flag-title="{{ post.title }}" style="float: right;color: #a9b0bc;display: inline-block;font-size: 14px;letter-spacing: .6px;line-height: 22px;">
    <span class="post-meta-divider">&ensp;|&ensp;</span>
    <span class="post-meta-item-text">阅读量:  </span>
    <span class="leancloud-visitors-count"></span>
</span>
{% endif %}
```

**主要是记录下配置浏览量的过程，没有具体解释配置的原因。如果没有理解具体配置原因，可以参看参考链接**




#### 参考链接：
[GithubPages教程 在GithubPages上搭建个人主页](https://blog.csdn.net/yanzhenjie1003/article/details/51703370)

[Ruby 安装 - Windows](http://www.runoob.com/ruby/ruby-installation-windows.html)

[jekyll 完整安装教程](https://blog.csdn.net/joelcat/article/details/78642434)

[GoDaddy最新域名解析教程（中文版）](http://godaddy.idcspy.com/godaddy-jx)

[在个人博客中添加文章点击次数](https://blog.csdn.net/u013553529/article/details/63357382)

[$H_2O主题$](https://github.com/kaeyleo/jekyll-theme-H2O)

