---
layout: post
category : technology
tagline: Decompiling from *.swf to *.fla
tags : [Front End, Flash]
---

上个星期总算是断断续续抛开了服务器后端的工作了，暂时可以忘掉神经网络和SVM。各种能量、共振峰、基频和高次谐波也能告一段落了，总算是要开始转战前端布置了。功能倒是不太复杂：网页实现录音，再传送给服务器即可。原先的计划是用HTML5来完成这一切，后来查了手册，原来HTML5还很有很多的功能都没有实现。比如这个网页的录音和录像仍旧在项目计划之中。

既然原生HTML5不能实现，我只有另寻其道了。Google若干资讯后考虑Flash或者Java Applet，但是在他们之间犹豫了挺久，因为这两块知识我都几乎没有涉及过。继续咨询几位朋友，决定用相对较为成熟的Flash来完成，所以苹果用户只能暂时Say sorry了。对了，这里要特别感谢Yeguikun的帮助和建议，^_^。

Flash代码？ActionScript？哈哈，我当然不会写了，可是万里长城总得迈出第一步啊，没办法就硬着头皮学吧。我一直认为**现学现卖**是一项非常实用，并且需要经常锻炼的能力（不过有时间多花时间打基础还是最好）。从Adobe官网上找到ActionScript 3.0的手册，700多页花了2个多小时大概浏览了一遍，也算是知道了个大概（怎么着也得弄明白动作和函数怎么调用啊）。然后我们就要开始着手代码的事情了，不过，你真的觉得我看了2个小时就能用AS写个功能健全的flash嘛？开什么国际玩笑呢。Google“swf反编译”才是我下一步要做的。接下来就是漫长的信息过滤和忐忑的尝试过程了。毕竟从来没有做过写过flash，朋友也没有做过反编译的工作，所以不免还是要走一些弯路的。在翻阅了很多的笔记、尝试用了N多国内、外的免费、收费（xxoo版）之后终于找到让我比较满意的方案，几乎实现了完美的反编译。之所以说是几乎，是因为还是有一些很小的bug，不过依靠着前面2个小时的工作，还是很轻松地将问题解决了。

下面就简单提一下这个被我尝试出来的方案吧。一共需要两款软件（都是Windows Only，所以Mac和Linux用户请……）

### 硕思闪客精灵
国产收费软件（有免费适用版，但功能上实在是过于欠缺）。作用是打开*.swf，能够导出*.fla文件和AS脚本。按理说这样也就大功告成了。但是编译会各种Error，仔细一看发现都是反编译不成功出现的变量未赋值的错误，而且中文全部乱码。但是对错误进行注释或者一些处理之后，却发现能够编译通过并运行，说明这个fla还是可以用的。于是继续寻找反编译AS脚本的工具，功夫不负有心人，我终于找到了提取AS脚本的神器。

### AS3 Sorcerer
国外收费软件（同样有免费试用版，功能几乎没变化，就是会弹窗广告）。专门用来提取flash里面的ActionScript脚本，用起来很傻瓜，打开swf即可。再将里面的AS脚本拖出来覆盖前面硕思闪客精灵提取出来的脚本。好了，然后尝试编译，正常运行。随后在测试的过程中发现发送的按钮传递函数有个变量出错，不过稍作分析代码之后已经修正。

BTW，这两款软件的收费版本价格也并不高，如果你是专业开发人员或者使用比较频繁的话，还是强烈推荐购买正版。当然了，他们的xxoo版本都可以在国内各大网盘或者博客空间都有下载，不过希望大家还是要带着感恩的心去使用。如果你是通过搜索引擎来到我这篇博客，只是为了寻找反编译工具，那么你已经可以Ctrl+w，然后去下载他们了。

---
下面写一下走过的弯路和想到的东西吧，可能会有一点点的杂乱。

前面有提到硕思闪客精灵这个东西，其实我对它突然就有了点印象，想起来初中玩Flash的时候好像有这玩意。随后Google了一下，发现还有传说中的硕思三剑客：闪客精灵、闪客之锤和魔法菜单。比较奇怪的是现在好像之后硕思闪客精灵了，其余两款产品在硕思的官网上已经找不到了。想来还有点可惜，从软件介绍的角度来说，我感觉闪客之锤更加强悍，可以直接导入swf+脚本改写+编译测试+导出fla。实在是有些可惜了中间在寻找提取swf脚本工具的时候，发现一款软件被提到和推荐的次数极高。Action Script Viewer，功能如名。虽然试了之后发现准确率是的确还不错，但是不知道为什么有的脚本无法显示。

最后想说一点，反编译不靠谱。想真心学，老老实实看AS的Manual+Adobe提供的Example。
