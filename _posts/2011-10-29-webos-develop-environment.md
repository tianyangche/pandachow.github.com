---
layout: post
title: WebOS开发——开发环境的搭建
categories:
- 伪技术
tags:
- webOS
- 移动开发
---

这篇文章算是半转载半原创吧，不过也是参考了很多资料，感谢煮机网和机锋网的先人们的经验总结。

这里全部都是在Ubuntu Linux下的搭建过程，版本是Ubuntu 10.10（Maverick），由于个人习惯原因不太喜欢Unity和Gnome 3 &Gnome Shell，就索性换到10.10了。

惠普官方的开发者页面上其实已经给出了相当详细的配置过程，详情请[点击这里](https://developer.palm.com/content/resources/develop/sdk_pdk_download.html)，但是整个步骤还是有一些不太清楚的地方。

## 1. 安装Java环境
下载并安装JRE，这个没什么好说的，整个WebOS的所有软件包都是用跨平台效果很好的java写的，毫无疑问需要java的支持，这里我们需要去官网下载，注意对应自己的操作系统版本，[下载页面](http://www.java.com/zh_CN/download/manual.jsp?locale=zh_CN)，貌似Ubuntu是装机就装好的？

注意：官方开发文档注明，经常在Ubuntu升级之后，我们默认的Java版本可能已经高于sun-java6。而需要保证sun-java6-jre作为默认。

设置默认的方法：
    
    sudo update-alternatives --config java

并选择/usr/lib/jvm/java-6-sun/jre/bin/java来启动。
如果palm-emulator以后出现了停止工作或者出错的情况，可能在这里需要对JRE进行重新配置。

## 2. 安装VirtualBox
官网的说法是VB需要先装，这个直接去Oracle的官网下就好了，下载地址[点此](https://www.virtualbox.org/wiki/Downloads)，注意下载对应的操作系统。注意，官网上说法是palm-emulator已经可以支持VirtualBox的4.0和4.1了，现在搜到很多以前用来配置开发环境的帖子都是支持3.2.

## 3. 安装ia32-libs（仅64位系统需要）
如果你的电脑安装的是64位系统，需要安装这个玩意儿，如果你已经在此之前安装过palm-novacom（驱动），则还需要对palm-novacom进行重启。
    
    sudo apt-get install ia32-libs
    sudo stop palm-novacomd
    sudo start palm-novacomd

## 4. 安装SDK

1. 安装novacom，这货其实就是webos设备的驱动而已。分别下载安装即可，[32位点此](https://cdn.downloads.palm.com/sdkdownloads/3.0.4.669/sdkBinaries/palm-novacom_1.0.80_i386.deb)，[64位点此](https://cdn.downloads.palm.com/sdkdownloads/3.0.4.669/sdkBinaries/palm-novacom_1.0.80_amd64.deb)。其实在刷机的时候，WQI连接手机前就会安装的。
2. SDK，注意这个SDK不算小，可以先下载一边完成上面的步骤，[点此下载](https://cdn.downloads.palm.com/sdkdownloads/3.0.4.669/sdkBinaries/palm-sdk_3.0.4-svn519870-pho669_i386.deb)。

由于这两个已经是打好的deb包，所以在Ubuntu下直接安装即可，需要注意的是这里下的SDK是基于WebOS 3.0.4，也就是完全是为了开发TouchPad用的，手机版本SDK请见文章后部分。

SDK安装完成之后就可以在电脑上试试虚拟环境了，在终端输入：
    
    palm-emulator

如果可以看到下图就说明虚拟环境安装完成了，当然了，你可能只能看到最后一个SDK。

[![](http://panda0411.com/wordpress/wp-content/uploads/2011/10/HP-webOS-Emulator-_008.png)](http://panda0411.com/wordpress/wp-content/uploads/2011/10/HP-webOS-Emulator-_008.png)

前面提到了，这个3.0.4是专门为TouchPad准备的，那Pre Plus/Pre2/Pre3呢？当然有了，请到HP官网的自行[下载](https://developer.palm.com/content/resources/develop/sdk_pdk_download.html)，注意在页面底部。需要注意的是，我们下载下来的应该是一个zip的压缩包，不需要解压，用palm-emulator的安装命令来进行安装。
    
    palm-emulator --install "~/Downloads/SDK 2.1.0.519.vmdk.zip"

当然了这里的目录和文件名都是示例而已，安装完毕之后启动palm-emulator就会出现多个虚拟环境引导了。

## 5. Eclipse的安装

其实本来不准备用这货写代码的，考虑Palm为方便众多的开发者，已经做好了很多Eclipse的插件，首先去Eclipse的官网[下载即可](http://www.eclipse.org/downloads/)，注意最好下载Java EE版本的。下载完成之后点击运行，下面简单说明插件的配置。

1. Help-->Install New Software
2. 在Work with里面填写webos – [http://cdn.downloads.palm.com/sdkdownloads/eclipse-update-site/site.xml](http://cdn.downloads.palm.com/sdkdownloads/eclipse-update-site/site.xml)，并点击add按钮，勾选搜索出来的webos插件，如图。[![](http://panda0411.com/wordpress/wp-content/uploads/2011/10/Install-_007.png)](http://panda0411.com/wordpress/wp-content/uploads/2011/10/Install-_007.png)
3. 重启Eclipse，应该就能在工具栏上看到一个手机的标志了，cong～