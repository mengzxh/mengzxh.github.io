---
layout: post
title: '逆向android app相关工具'
subtitle: ''
date: 2018-09-08
categories: android逆向
cover: ''
tags: apktool jd-gui dumpdex dex2jar
---

### 逆向android app相关工具

* [Apktool](https://ibotpeaches.github.io/Apktool/install/)
* [dumpDex-Android脱壳工具](https://github.com/WrBug/dumpDex)
* [jd-gui](https://github.com/java-decompiler/jd-gui) A standalone Java Decompiler GUI
* [jadx](https://github.com/skylot/jadx) Dex to Java decompiler
* [luyten](https://github.com/deathmarine/Luyten) An Open Source Java Decompiler Gui for Procyon

* [dex2jar](https://github.com/pxb1988/dex2jar) Tools to work with android .dex and java .class files

### 反编译android app步骤

#### 加壳判断

1.判断app是否加壳，通过7zip工具解压android app，查看解压后的文件夹的根目录下是否含有classes.dex或者classes2.dex,如果含有一般可以通过上面的jadx工具打开是可以看到对应代码，这个是最简单的情况。

2.如果没有对应的classes.dex，一般可以确定是加壳了。通过查看资料发现大多数加壳后都会生成相应的特征so文件。这样就可以根据so来查壳，本文以360的某一款浏览器为例解压，并在其中可以找到libjiagu.so文件，可以确定该浏览器使用了360加固。

| 特征So文件          | 所属加固公司 | 特征So文件                      | 所属加固公司 |
| ------------------- | ------------ | ------------------------------- | ------------ |
| libchaosvmp.so      | 娜迦         | libsgsecuritybody.so            | 阿里聚安全   |
| libddog.so          | 娜迦         | libmobisec.so                   | 阿里聚安全   |
| libfdog.so          | 娜迦         | libtup.so                       | 腾讯         |
| libedog.so          | 娜迦企业版   | libexec.so                      | 腾讯         |
| libexec.so          | 爱加密       | libshell.so                     | 腾讯         |
| libexecmain.so      | 爱加密       | mix.dex                         | 腾讯         |
| ijiami.dat          | 爱加密       | lib/armeabi/mix.dex             | 腾讯         |
| ijiami.ajm          | 爱加密企业版 | lib/armeabi/mixz.dex            | 腾讯         |
| libsecexe.so        | 梆梆免费版   | libtosprotection.armeabi.so     | 腾讯御安全   |
| libsecmain.so       | 梆梆免费版   | libtosprotection.armeabi-v7a.so | 腾讯御安全   |
| libSecShell.so      | 梆梆免费版   | libtosprotection.x86.so         | 腾讯御安全   |
| libDexHelper.so     | 梆梆企业版   | libnesec.so                     | 网易易盾     |
| libDexHelper-x86.so | 梆梆企业版   | libAPKProtect.so                | APKProtect   |
| libprotectClass.so  | 360          | libkwscmm.so                    | 几维安全     |
| libjiagu.so         | 360          | libkwscr.so                     | 几维安全     |
| libjiagu_art.so     | 360          | libkwslinker.so                 | 几维安全     |
| libjiagu_x86.so     | 360          | libx3g.so                       | 顶像科技     |
| libegis.so          | 通付盾       | libapssec.so                    | 盛大         |
| libNSaferOnly.so    | 通付盾       | librsprotect.so                 | 瑞星         |
| libnqshield.so      | 网秦         |                                 |              |
| libbaiduprotect.so  | 百度         |                                 |              |
| aliprotect.dat      | 阿里聚安全   |                                 |              |
| libsgmain.so        | 阿里聚安全   |                                 |

参考来源：[APK脱壳简介](http://www.bigfog.info/2018/04/15/Apk%E8%84%B1%E5%A3%B3%E7%AE%80%E4%BB%8B/)

#### 脱壳准备工作

既然发现软件加壳，要想看到源代码只能进行脱壳操作。本文以dumpDex-Android脱壳工具进行脱壳。这款工具需要配合安卓5.0以上的手机 + xposed框架操作，如果没有可以使用虚拟机进行操作，或者使用virtualxposed。

国内大部分的安卓模拟器基本不支持5.0以上的android系统，本文使用支持android 5.0 genymotion做演示。

虚拟机安装过程说明：

* [打开官网](https://www.genymotion.com/)注册（需要验证邮箱）
* 打开[下载网址](https://www.genymotion.com/download/)下载with VirtualBox的Genymotion。
* 下载之后打开Genymotion并安装软件，基本都是点击下一步，同时该软件会安装virtualBox确认并安装。
* 安装完成以后打开Genymotion软件，添加android 虚拟机并安装完成。建议使用**google Nexus5X-6.0**操作系统。

* 下载[Genymotion ARM Translation](https://pan.baidu.com/s/1NBCIas1TbqI3t7CpbV1Stw) 密码: g7xw，使android 虚拟机支持arm
* 打开android 虚拟机，**在非中文目录**将Genymotion ARM Translation.zip用鼠标拖动到android虚拟机界面上，安装文件并重启虚拟机。
* 添加android虚拟与windows共享目录方便脱壳之后的源文件在电脑上分析。

#### 通过dumpDex脱壳

虚拟机安装完成以后，请安装软件到虚拟机上，安装app可以通过拖动app到android虚拟机界面上即可安装。

准备工作做完之后：

* 安装xposed框架，并激活框架
* 安装dumpDex模块并激活
* 安装需要脱壳的软件

安装完成后，没有激活框架或模块的，请务必重启手机（虚拟机），以使xposed框架生效。

以上完成以后，打开需要脱壳的软件，等待片刻，即可在**/data/data/包名/dump**中看到dex后缀文件即为脱壳后的源文件。



### dex文件处理

dex文件可以通过[jadx](https://github.com/skylot/jadx)工具直接查看，也可以通过[dex2jar](https://github.com/pxb1988/dex2jar)转为jar文件后查看，个人还是倾向使用dex2jar转化后通过[jd-gui](https://github.com/java-decompiler/jd-gui)查看。

下载[dex2jar](https://bitbucket.org/pxb1988/dex2jar/downloads/)并解压，将对应的dex文件放入解压后的dex2jar文件夹，运行下面命令即可。

dex2jar转化命令：

```bash
d2j-dex2jar.bat classes.dex
```

下载[jd-gui](https://github.com/java-decompiler/jd-gui/releases/download/v1.4.0/jd-gui-windows-1.4.0.zip)通过jd-gui打开jar文件。



### 重要提示：

* 部分通过jd-gui打开的jar文件中，可能有部分函数报错，目前通过网站查询资料发现原因基本都是函数太长了，jd-gui解析不了可以使用[Luyten](https://github.com/deathmarine/Luyten/releases/tag/v0.5.3)，可以避免直接研究smali文件，smali文件一般来说晦涩多了。 ------- 这块被坑了好久。
* 通过抓包wireshare/手机抓包工具获取部分的请求信息（URL、Headers、参数等）在jd-gui里面搜索可以更快获取想找到的函数。
* 如果对java/android不熟悉，建议先几分钟入门[java](http://www.runoob.com/java/java-tutorial.html)/[android](http://www.runoob.com/w3cnote/android-tutorial-intro.html)，写出一些基本的代码，以保证能基本代码理解逻辑。



### 参考链接：

[如何使用apktool反编译APK](https://blog.csdn.net/sonnyjack/article/details/79273023)

[Android模拟器Genymotion安装使用教程详解](https://www.cnblogs.com/whycxb/p/6850454.html)

[APK脱壳简介](http://www.bigfog.info/2018/04/15/Apk%E8%84%B1%E5%A3%B3%E7%AE%80%E4%BB%8B/)

[常见app加固厂商脱壳方法研究](https://paper.seebug.org/44/)

[dumpDex-Android脱壳神器](https://juejin.im/entry/5ab201f651882555635e3327)

[Android逆向工程](https://www.jianshu.com/p/ca2847c7bd52)

[Android脱壳圣战之---脱掉360加固壳](https://blog.csdn.net/jiangwei0910410003/article/details/78548069)

[Android模拟器Genymotion安装使用教程详解](https://www.cnblogs.com/whycxb/p/6850454.html)

[Genymotion与电脑文件共享](https://blog.csdn.net/u013183608/article/details/55188869)

[win10安装 Genymotion ARM Translation教程](https://blog.csdn.net/w605283073/article/details/70597368)













