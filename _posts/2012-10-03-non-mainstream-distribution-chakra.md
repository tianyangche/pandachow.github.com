---
layout: post
categories: technology
tagline: Just another awesome Linux distribution
tags: [Linux, Archlinux]
---
小明：小熊猫，查克拉是什么？

答：
1. [火影忍者](http://baike.baidu.com/view/66454.htm#sub5112035)
2. [宠物小精灵](http://baike.baidu.com/view/66454.htm#sub6789971)
3. [能量中枢](http://baike.baidu.com/view/66454.htm#sub5112034)

小明：>_<

答：囧

---
好吧，正文部分开始，这里的查克拉（Chakra Linux）其实是一款基于 Archlinux 略非主流的 Linux 发行版，从目前的 Rank 来看基本上处于20名左右，不过好像还在下滑中……

##一、从基于 Arch 谈起
既然是基于 Arch，咱们不妨先来看看 Arch 的一些特点好了。

>* 简洁：Arch 设计理念的核心是所谓 KISS 原则（Keep It Simple, Stupid），即保持简洁。Arch 所指的简洁，是指没有任何不必要的附加软件，为用户提供一个精简的最小化系统以及清晰易懂的配置文件。

>* 自由：基于简洁原则，Arch 安装之后仅仅提供了一个精简的最小化系统（没有图形界面），用户可以此为起点构建一个完全符合自己需要的操作系统，而不会受到原有任何束缚。

>* 轻量：Arch 针对桌面系统常用的 i686/x86-64 架构进行了特定优化，并且具有简洁的系统结构，因此在性能上比 Ubuntu、Fedora 等其他流行的发行版更为优越，无论是系统启动，还是运行程序，感觉都较为轻快。

>* 滚动更新：Arch 采取滚动更新的策略，因此没有 Ubuntu、Fedora 等其他流行的发行版中的跨版本升级的概念，用户随时可以将系统更新到最新状态。

>* 更新迅速：Arch 不仅具有滚动更新的特性，而且软件更新推送的策略也更为激进，用户可以在第一时间使用到最新的软件。

>* AUR：AUR 全称是 Arch Linux User-Community Repository，即 Arch Linux 用户社区软件仓库，是供 Arch 用户分享 PKGBUILD 文件之用。所谓 PKGBUILD 文件，用于自动下载软件源码并编译，可以大大方便用户编译安装程序的过程。对于还未进入 Arch 官方软件源的软件，用户可以使用其他 Arch 用户提供的 PKGBUILD 方便地进行编译安装。

没错，Archlinux 就是这样一个简洁和定执行高的发行版，特别是在 experienced linuxer 中有非常好的口碑。我没有将 Arch 作为自己主力系统的原因并不是安装和配置障碍，而是 Arch+KDE 不稳定所致，这一度让我非常郁闷。对于一个普通用户来说，选择一个稳定且易用的桌面甚至比选择一个发行版或者内核版本更有意义。当然不可否认的是，我从 Arch 和 Gentoo 的 wiki 中学到了非常多的东西。

鉴于 Archlinux 的诸多优点和一些不足，也就诞生了一些基于 Arch 的发型版，比如使用 Openbox 的 ArchBang。而 Chakra 就是这样一款继承了 Arch 众多优点，又特别定制优化的独立发行版。

##二、Chakra
说完 Arch，那 Chakra 和它又有什么区别和联系呢？从某种程度上来说，Chakra = Arch + KDE，但这也是非常不准确的，因为它有着独立的官方和社区软件仓库，独立的发布周期和独立的软件更新策略。总的来说，Chakra 相对于 Arch 还是有一些比较大的区别。
>* 纯粹的 QT：基于 Chakra 的主旨，一方面 Chakra 不可能像 Ubuntu 或者 Fedora 那样提供不同桌面环境下载；另一方面 Chakra 旨在成为一个纯净的 QT 的发行版，所以大部分基于 GTK 的软件都不会出现在软件仓库中。而那些我们熟知的诸如 Firefox、Chromium、Eclipse 这样的 GTK 软件都会以 Bundle 的形式来安装。这个 Bundle 可以理解为将软件和依赖打包，而在运行这些软件的时候将 Bundle 挂载为一个独立的虚拟文件系统，以此来维护系统的纯 QT 特性。就个人而言，我还是非常喜欢这一点的。
>* 半滚动升级：滚动升级一直是 Arch 最广为人知的特性之一。而 Chakra 仅仅推送必要的软件更新。所以这一点孰优孰劣，还需要用户自己在稳定性和快速更新之间进行权衡。个人观点，稳定性为上。
>* 社区软件仓库：Arch 用户对 yaourt 的易用性和仓库软件数量无一不表示赞同和肯定，而大部分软件的稳定性也还说的过去。这一点上，Chakra 的 ccr 算是给继承过去了，不过现在 ccr 里面软件的数量和 aur 还是不能相提并论。
>* 不一样的 KDE：由于自己对桌面的种种要求，只能选择 KDE，而 Chakra 的 KDE 已经在原生的 KDE 上做了很多的优化。远比 Arch+KDE 要美观和易用的多。当然如果你选择其他的 DE 或者以折腾美化设置为乐趣可以忽视这一点。

其实关于 Arch 和 Chakra 之间的关系也是一个非常有意思的问题，KDEmod 诞生的时候就是为了给 Archlinux 定制模块以及提供一个精简的 KDE 桌面。而后来 KDEmod 的作者又开始制作 Arch+KDEmod的LiveCD，于是这便成了 Chakra 的早期版本。此时的 Chakra 与现在的 ArchBang 非常类似。而后来 Chakra 的作者 Jan Mette 去世之后（1978-2010，R.I.P.），开发团队决定继续 Chakra，并使 Chakra 成为一个完全独立的发行版，而不再是简单的定制版 Arch。渐渐的，Chakra 有了独立的官方和社区软件仓库，独立的发布周期和独立的软件更新策略。

##三、不再废话
原本还想在这里写上 Chakra 的安装配置过程，毕竟想想觉得没什么必要，毕竟敢于尝试这样一款小众 Linux 发行版的同学都有一定的折腾经验和英文阅读能力，更何况 Chakra 的安装过程比 Ubuntu 还傻瓜。

**不在卖弄，旨在推荐。**