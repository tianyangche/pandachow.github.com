---
layout: post
title: Arch Linux安装笔记
categories:
- technology
tags:
- Arch Linux
- Linux
---

最近刚刚放假也就闲了下来，毕设什么都先放一边好了。看着自己都Ubuntu 10.10越来越慢，就想换个发行版了。当然了，并不是我不喜欢Ubuntu，之后10.10之后都Unity和GNOME3都让我很反感。GNOME3在我的电脑上跑起来非常卡，况且我非常不理解的是图标弄得那么大难道是要让我用手触摸吗？所以在我看来GNOME3更适合平板用户。无奈之举我决定转战KDE阵营。

犹豫了一下，在Kubuntu、OpenSUSE和Arch之间选择了Arch，一个在业内非常被推崇的发行版。

在折腾Ubuntu和OpenSUSE相当长的一段时间之后就对他们的庞大和遮掩很疑惑，诚然类似于Ubuntu和Fedora这样的发行版做的非常人性化和简单，但是对于有一定的Linux基础的人来说，这显然不太适合。他们可以去追求更加个性化和适合自己的操作系统。当然我只是针对有一定Linux基础的人而言，什么叫有一定基础？如果/etc/下面的文件和脚本你都能知道是做什么用的，top出来的进程也都知道作用，我觉得就没啥问题了。所以以前在某邮件列表里面看到给新人推荐Arch或者Gentoo这样的发行版的时候，真不知道这些人都是什么样的心理。

在Arch的wiki里面被反复提到的就是 Arch 哲学也即是著名的 Unix 哲学：K.I.S.S. (keep it simple, stupid)，所谓大道至简，所有的东西不求丰富，够用、简洁、迅速并且高效地为我们服务就是Linux最大的优点。当然这个简单并不是指Arch安装和配置很简单，而是指系统简洁、或者对于一名能够深入了解操作系统内核的人来说，这样的电脑是简单而纯净的。例如在Arch的配置过程中，会反复修改/etc/下的各个脚本文件，这些脚本中包含着几乎所有计算机的重要配置文件，不仅一目了然，弄明白它们都意义之后修改起来也是非常简单。这几乎也是遵循了BSD风格。

下面是我在虚拟机中折腾无误之后，在实体机器上安装的全过程。其实大部分在[arch的wiki](https://wiki.archlinux.org/index.php/Beginners%27_Guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))上已经有了十分详细的安装方法，在此我只挑些重点以及上面没有的讲。

## 安装方式
这个在Arch的wiki上花了不少的篇幅，我就不再重复了。我自己起先用的是Unetbootin这款软件在Ubuntu下烧录的，结果发现无法启动，可能是Unetbootin重点是放在了Ubuntu这样的发行版上了吧，最后我用的是dd的方式。这里需要注意的一点是dd的目录是整个U盘：/dev/sdb，而不是某个目录/dev/sdb1

	dd if=archlinux-2010.05-core-dual.iso of=/dev/sdb

## 硬盘分区
这个照着wiki直接来就行，如果没有什么特殊的要求直接用第一种分区方式就可以了。我自己的/boot/给分的500兆，/home/200G，/usr/local给分的100G，剩下的全部给根目录了。文件系统格式什么的与Ubuntu无异，略过。

## USER

习惯了sudo给我的方便，在Arch里面装sudo直接用pacman就可以了。
用adduser来增加我平时的使用账户，然后在 /etc/sudoers 中增加一条记录： xxx ALL=(ALL) ALL 把我的账户列入sudoer中方便使用，之后便不再用root了，全部sudo搞定。

## AUR
Arch 的精髓之一便是 AUR (Arch User Repository)， 正如其名称所示， AUR 是一个软件仓库，里面的软件包均为 Arch 用户上传，有着非常庞大的软件储备。总的来说AUR是属于民间的软件仓库，和ppa的道理比较类似，但是又不像ppa添加起来那样麻烦。而 yaourt (Yet AnOther User Repository Tool) 则是一款神器级别的软件包管理工具。

AUR使用方法：/etc/pacman.conf 中加入：

	[archlinuxfr] Server = http://repo.archlinux.fr/i686"

然后安装yaourt

	sudo pacman -S yaourt

## 声音
声音问题还真是弄了好久，虽然试照着wiki一步步来，但仍然没有办法出声音。不过最后还是依靠google解决的了。

	sudo yaourt -S alsa-utils alsa-oss sudo
	gpasswd -a pandachow audio
	sudo alsaconf # 这个貌似wiki上没讲，我自己的电脑上必须要进行配置才能生效


## X
X的安装是我在虚拟机里面一直没有搞定的地方，没想到在试题机器上一步就完成了。当然这回我学乖了，在GNOME还未稳定下来之前，我投靠了KDE。装上之后发现与我3年前第一次用OpenSUSE的变化真的很大，已经完善很多了。漂亮、迅速、简洁、方便、好用是KDE给我的全部感受。一如GNOME2.3当年带给我的感觉一样。
安装基础包:

	sudo pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils

安装 [mesa](http://en.wikipedia.org/wiki/Mesa_3D_(OpenGL))以获得 3D 支持:

	sudo pacman -S mesa

3D 工具 glxgears 和 [glxinfo](http://dri.freedesktop.org/wiki/glxinfo)都包含在 **mesa-demos** 包里。如果需要这些工具，请安装这个包:

	sudo pacman -S mesa-demos

对于显卡驱动，集成显卡表示非常省事，一个命令即可全部解决，N卡和A卡都同学请自行对照wiki折腾。。。

	sudo pacman -S xf86-video-intel

xorg都搞定之后，KDE就可以进行安装了，由于我不太喜欢太多乱七八糟都软件，使用的是KDE都最小安装：

	sudo pacman -S kdebase kde-l10n-yourlanguagehere phonon-vlc

当然不要忘记把xorg、kdm、dbus、bluetooth什么的都加到DAEMONS里面去。如果是使用virtualbox作为虚拟机，还需要把vboxdrv模块加到这个脚本里面去，免得每次都要手动加载。

## 无线网络
Arch下无线网络的配置并不是意见很容易的事情，至少我是这样认为的。这里推荐使用wicd来管理无线网络（当然，wicd也是可以同时管理有限网络的）。由于我是KDE桌面，有集成好的软件包：

	sudo pacman -S wicd-kde

使用多种网络管理工具容易产生各种问题，因此，请只用一种网络管理工具来管理网络连接。所以，在使用wicd前，必须先关闭其他网络管理工具。
首先，使用以下命令手动关闭network、dhcdbd和networkmanager这些守护程序。

	sudo rc.d stop network
	sudo rc.d stop dhcdcd
	sudo rc.d stop networkmanager

然后，以root身份编辑/etc/rc.conf。禁用各种网络管理守护进程，包括**network**, **dhcdbd**, 和 **networkmanager**：
添加守护进程**dbus** 和**wicd** 到DAEMONS中：

	sudo vim /etc/rc.conf # DAEMONS=(syslog-ng dbus !network !dhcdbd !networkmanager wicd)

把你帐号加入到network组中，把$USERNAME替换成你自己帐号名称。

	sudo gpasswd -a pandachow network

最后，启动守护进程 **dbus** 和**wicd** :

	sudo rc.d start dbus
	sudo rc.d start wicd

**注意: **如果守护进程**dbus**已经启动，需要重新启动它:

	sudo rc.d restart dbus


## In a word
最后装上常用的软件就差不多了，值得一提都是Arch的AUR实在是非常的好用。当我准备安装thunderbird的时候yaourt -Ss thunder出来的结果大大出乎我的意料。不仅把thunderbird给搜出来，让我震惊的是搜出来了wine-thunder。哈哈，社区力量实在是异常强大。当然Arch的配置远不止这么简单，比如我自己现在蓝牙仍然没有解决。所以我还是推荐有条件的能装双系统，保证一个系统能用（比如Ubuntu或者温都死），然后没事的时候来折腾Arch，直到把Arch下所有的问题都搞定了再去单系统，免得来任务都时候没系统用。