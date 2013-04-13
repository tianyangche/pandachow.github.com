---
layout: post
title: TouchPad折腾手记_Part 2
categories:
- 伪技术
tags:
- touchpad
- 数码
---

这篇记录算是记录下webOS的折腾过程，激活部分就略过了。。。当然了，如果当时手头没有就参照这个Treo8的帖子进行免激活开机。

[http://www.treo8.com/bbs/thread-169971-1-1.html](http://www.treo8.com/bbs/thread-169971-1-1.html)

以下所有操作都需要wifi的接入，当然了速度是越快越好.

## 系统更新
第一步当然是开机，要连上wifi，首先做的是系统更新，这里我默认需要升级到WebOS 3.0.2，当然了HP后期的所有的貌似已经是出场预装3.0.2了，暂时不推荐3.0.4，原因后面会说明。需要说明的是如果是3.0.0到3.0.2可能下载会比较慢，如果卡住了可以尝试关机后再续传。

## 应用安装

### TP用USB线连接到电脑后，右上角会出现提示：USB Drive和Close，不去管它或者选Close都可以（选USB Drive会以移动存储的形式）。

### 在TP的主界面（如果不在主界面，按一下底部的Home键）顶部的Just type内输入webos20090606，出现开发者模式Developer Mode，进去开启即可，第一次会提示设定密码，可以为空直接submit。开启开发者模式主要即为了能在PC上对TP进行软件的安装和刷机等操作。

###（WQI）WebOS Quick Install是个装在PC端的基于Java的跨平台安装软件。WQI的官方发布页在[这里](http://forums.precentral.net/canuck-coding/194832-webos-quick-install.html)，下载回来直接运行即可，当然实现是需要JRE的支持（如果你的电脑上没有，到[这里](http://www.java.com/zh_CN/download/manual.jsp?locale=zh_CN)下载安装）。这时我们已经完成了一种比较快速的第三方软件的安装方式了，下面简单介绍一下第二种。

### 从WQI中安装Preware，点击一下WebOSQuickInstall右侧的地球图标，在弹出的窗口的第一个标签Applications里找到Preware然后安装即可，找不到可以进行搜索。安装完成之后，TouchPad里面应该会出现preware的企鹅图标，哈哈，熟悉的Tux！点击Preware图标启动程序，经过源的加载界面之后，进入协议页面，下拉点击表示同意将IPK文件与preware关联，选择yes即可。Preware已经内置了适合TouchPad的第三方软件和补丁，你还可以设置添加更多的第三方安装源，和linux换源道理类似。

## 中文语言包
由于大多数人对英语还是有些犯晕发怵的，中文语言包[点此下载](http://bbs.zoopda.com/forum.php?mod=attachment&aid=OTgzOTR8ZTBjNzY4YzB8MTMxOTY4MzAzNnwxODM2Mzd8NzgzODc%3D)

有了中文显示，当然中文输入也是必备的，对于中文输入方案这里提供两种，也是最主要的两种。
	
1. QQ的云输入，一查原来作者是WebOS上飞信的作者freeworkzz，大牛一枚。大牛博客[在此](http://www.freeworkzz.com)，输入法[点此下载](http://bbs.zoopda.com/forum.php?mod=attachment&aid=OTc5NjJ8OWM0MTgzNzJ8MTMxOTY4MzI3NHwxODM2Mzd8Nzc5NDU%3D)。
2. 百度输入法，速度尚可，安装的时候可能会出现失败的情况，如果安装完毕后机器重启就不必担心了，另外装外之后是需要在区域和语言设置项里面选择中文的，[点此下载](http://bbs.zoopda.com/forum.php?mod=attachment&aid=MTA0NTg2fDQ5OGM0ZjdhfDEzMTk2ODM1OTR8MTgzNjM3fDgzNzE2)。

## CPU超频
在Preware里面搜索Govnah后安装，完毕后重启，我们就是用它来操作超频核心。这里对超频核心做个简单的介绍吧，超频核心的作用如名，就是为了使CPU频率增加，和我们电脑上的也是类似。其实超频核心的作用很多，比如，通过不同的管理器调节核心电压，管理缓存，调整内存频率，根据不同状态调节频率等等。

UBER是惠普官方发布的正式的给Touchpad使用的超频核心，相对比较稳定保守，除此之外还会有不同的测试核心，能达到更高的频率。关于超频核心可以参考煮机的这个帖子

[http://www.zoopda.com/forum.php?mod=viewthread&tid=84005](http://www.zoopda.com/forum.php?mod=viewthread&tid=84005)

在这里我自己使用的是使用最多的F4核心，安装好之后的超频默认是1.7G吧，如果想改频率，到DOWNLOADS里运行Govnah-->PROFILE-->Advance Settings，在里面调整MAX FREQ就可以了，试了一下1.836G，玩大型游戏好多了，貌似也没发现有什么问题。其实对于超频也不用太过于担心，超频失败的后果无非就是机器重启而已，更不用担心超频会无谓费电什么的，因为CPU的频率会根据即时工作要求来进行调整的。

## 5. 补丁 & 优化
在PREWARE里点击List of Everything，依次搜出下面的应用进行安装，个人感觉确实效率提高不少。

1. Muffle System Logging (WebOS的缺省设计是记录多有的运行记录，这个补丁可以关闭对其它的记录只维持错误的记录，从加快系统的速度。
2. Remove Dropped Packet Logging (另外一个帮助提高运行速度的补丁)
3. Unthrottle Download Manager (绝对加快下载数率，尤其在wifi环境下，类似Pre的下载限制就是64KB/秒)
4. Faster Card Animation HYPER Version (提升各类卡片和Launcher的速度）
5. Increase Touch Sensitivity And Smoothness 10 (增加系统在点触、滑动时的准确性和流畅性)
6. Advanced Reset Option (长按电源键之后，右上角的下拉菜单会弹出“飞行模式”、“Luna重启”、“系统重启”、“关机”和“取消”按钮)

## 6. 关于应用
附几个几个游戏吧[http://115.com/file/bhdfyq22](http://115.com/file/bhdfyq22)；

或者直接去煮机坛子找一些相关的帖子，官方的应用商店里面的游戏实在是有些捉襟见肘的。

[http://bbs.zoopda.com/forum.php?mod=forumdisplay&fid=246&filter=typeid&typeid=325](http://bbs.zoopda.com/forum.php?mod=forumdisplay&fid=246&filter=typeid&typeid=325)

就这么多吧，自己几天的刷机经历和玩机心得，当然参考了煮机坛子和Treo8的很多前辈们的帖子，在此一并谢过了。