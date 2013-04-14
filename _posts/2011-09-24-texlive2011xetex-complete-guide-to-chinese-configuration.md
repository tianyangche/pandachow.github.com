---
layout: post
title: TeXlive2011+XeTeX 中文配置完全指南
categories:
- technology
tags:
- TeX
- XeTeX
---

其实一直就像换个发行版用用，话说从09年开始用Linux到现在正经用过的也就只有连个版本：OpenSUSE和Ubuntu，当然这也都是因为RoboCup的原因。前几天在折腾LinuxMint，研究了一下貌似Mint有两个版本，分别是基于Debian和基于Ubuntu。

一开始装的是LinuxMint Debian Edition，选择这个版本有两个原因，一来是因为LMDE是基于Debian Testing的滚动更新，换句话说也就是像Gentoo和Archlinux一样没有跨版本更新的概念了，一直都是保持的最新版本。另一个原因是自己基本上也是习惯了也喜欢上了apt-get，未曾想到的是换了LMDE之后发现Debian还是不适合我的，刚装上就发现貌似是不能用ppa，虽然后来已经通过脚本实现ppa，心底想想这样其实已经有些违背了Debian的本质，反倒是十分吻合Ubuntu，哈哈。于是乎，我就装上了基于Ubuntu的LinuxMint 11，代号是Katya，粗略查了一下音译：卡迪亚，貌似没有实质含义了，百度图片里面是个不认识的美女，哈哈名字不错。于是就依然决然的吧这个美女装到电脑上了。

装完之后首要就是搞定一些大软件的安装和配置，MATLAB倒是简单得有点恶心，类似XP的一路Next。倒是TeX不是那么顺畅，毕竟上一次装套件已经是1年多以前了。今天花了2个小时总算是搞定了所有的配置，索性从头到尾写个全攻略。


## **一、安装TeXlive**
这个没什么好说的，强烈建议不要从apt-get的源里面安装，最好[下载镜像文件](ftp://ftp.tug.org/texlive/Images/)来执行安装，一来留存镜像方便以后安装，而来这个镜像文件比较大用迅雷或者一些下载软件断点续传什么的都比较好。我们的安装步骤也很简单：

1. 在安装之前我们先通过装一个perl-tk的软件包，很小，好处是安装的时候可以用图形界面（CLI党请无视）：

    apt-get install perl-tk

2. 把下载好的iso镜像文件挂载到电脑上，执行安装，我这里是挂载到/media/tex/目录：

    sudo mount -o loop texlive2011-20110705.iso /media/tex/
    ./install-tl --gui#前面的perl-tk的软件包就是能在这里使用gui参数

3. 几点说明：
	
  * 第四项有一个Language Collections，可以用来选择性的安装不需要的语言包，节省一些空间；	
  * 最后有一个Create symlinks in system directories，可以让安装程序自己来给我们创建链接；	
  * 当然了，如果你的硬盘不缺这一点空间还是建议选择全部安装的好；	
  * 如果以后需要升级的话直接用tlmgr --gui就好了。

## **二、XeTeX中文配置**
中文的排版对于TeX系统来说本来就是有着一定的先天的不足（谁让这个玩意儿最初设计的时候没有考虑过中文呢）。
对于TeX系统中文支持。这里一并介绍下：
中文支持的常见的几个方案：

### **1. CJK中文解决方案**
CJK包是德国人设计的，他不懂中国人的习惯，但是他设计的编码方式完全可以为我们提供任何可能处理中文的“理论基础”，我们只需要制定自己的“有中国特色的样式”。中国人的样式必需由中国人自己来制定。

### **2. CCT方式**
现在看来基本是过时了。这里不再阐述。

### **3. 基于 XeTeX 的方案**
这是我使用也是推荐的中文方案，但是windows下 运行速度缓慢，具体原因不详，如果你无法忍受请使用Linux。
安装完成TeXlive之后其实就已经可以进行中文的排版了，因为XeTeX是可以直接调用系统字体的。
举个例子：

    
    \documentclass{article}
    \usepackage{fontspec}
    \setmainfont{SimSun} %前提你系统里面要有宋体，没有就换你系统了里面有的字体，查看系统字体见后
    \begin{document}
    我是一只小熊猫啊，喵啊喵啊喵～
    Hi~ I'm an ailurus, meow~meow~
    \end{document}


这个小例子有几点需要说明一下：
1. 对于系统字体可以使用这连个命令来查看：

        fc-list :lang=zh-cn%中文
        fc-list :lang=en-us%英文

2. 对于原生的XeTeX系统，虽然已经可以排版中文，但还是有一些明显的缺陷，比如速度比较慢等等。如果已经把上面的中英文都排出来就会发现比较难看，难看的原因是用宋体来进行英文渲染实在是不妥，这也是就我们为什么还要在XeTeX的基础上就行完善的原因，完善之后的XeTeX能够更好地处理中文的断句、断字、更好地实现中英文的混合排版。

---
下面我们针对第3套方案进行配置的叙述，分为高层与低层两种，分别适用于不同需求的用户。

首先，安装所需的宏包：

    sudo tlmgr install xecjk ctex

### **1. 高层**
高层的方案是使用 ctex 宏包自带的文档类，例如原来用 article 文档类的就改用 ctexart 文档类，原来用 book文档类的就改用 ctexbook 文档类，这样绝大部分你会遇到的中文问题，比如字体设置、hyperref的调用、章节标题的设置等等，都自动为你解决了。下面是一个小例子：

    \documentclass{ctexart}
    \begin{document}
    你好，TeX Live 2009！
    \end{document}

默认情况 (winfonts) 下，你需要宋体 (SimSun)、仿宋 (simfang)、黑体 (simhei)、楷体 (simkai)、隶书(LiSu)、幼圆 (YouYuan) 这六套 Windows 字体，如果你的字体不全，可以编辑ctex-xecjk-winfonts.def 文件 (用 kpsewhich 来找) 来修改设置，也可以选择 Adobe Reader等软件所带的 Adobe Song Std, Adobe Heiti Std, Adobe Kaiti Std, Adobe FangsongStd 四款字体，这时需要给 ctexart 加上 [adobefonts] 选项。又或者，你可以不用这些预置的字体，使用 [nofonts]选项，然后参考 ctex-xecjk-winfonts.def 文件，自己定义对应各个 CJK 字体族的字体。

### **2. 低层**

总体来说，高层是相对来说是比较好写的，配置起来相对来谁比较简单易用，但是很多场合下，我们对中英有着不同的字体要求，于是我们就需要采用这种低层的方法，这种方案还是用我们原来的宏包，但是通过xeCJK宏包来配置字体。下面是一个小例子：
    
    \documentclass{article}
    \usepackage{xeCJK}
    \setCJKmainfont{SimSun}
    \begin{document}
    你好，TeX Live 2009！
    \end{document}

当然，你得有 SimSun 这个字体。不管选用高层的还是低层的，都可以直接用 xelatex 命令直接编译你的文档。

## **三、参考文章**

1. 一个Ubuntu forum很赞的老帖子：[http://forum.ubuntu.org.cn/viewtopic.php?f=35&t=168940](http://forum.ubuntu.org.cn/viewtopic.php?f=35&t=168940)
2. ChinaTeX的新浪帖子：[http://blog.sina.com.cn/s/blog_5e16f1770100lhbm.html](http://blog.sina.com.cn/s/blog_5e16f1770100lhbm.html)