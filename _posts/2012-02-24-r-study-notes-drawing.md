---
layout: post
title: R语言学习笔记--绘制图形
categories:
- 伪技术
tags:
- R
- 统计
---

回到学校也有些日子了，最近毕设弄得我十分头疼。不说也罢，想了想还是决定将寒假在家的R笔记全部写完，最后一部分是关于R的画图。说到画图，这里还要重点推荐一下gnuplot这个东西。自己着实被它的名字骗了很久，其他它跟GNU没有一点儿关系。关于这个gnuplot，后面或许还会有学习的笔记。

## 简单示例
这里还是拿前面用到的喷泉数据来做示范：
  
    > data(faithful)
    > attach(faithful)
    > names(faithful)
    [1] "eruptions" "waiting"
    > plot(waiting~eruptions,col="blue",main="Old faithful喷泉")
    > abline(lsfit(eruptions,waiting),col="red")

代码中lsfit()函数是回传waiting~eruptions最小平方线之截距及斜率。再配合abline()函数在散点图内绘制最小平方线。

![](/media/files/2012/02/24/pic_Old.Faithful.png)

比较明显能够看出来图中的数据点基本上分成两块，以eruptions=3.2或者waiting=70来划分应该都可以。

## 多重框图
下面我们计划用直方图来看出这两堆数据。不过我们需要先用par命令来划分一下。
    
    > par(mfrow=c(2,2))
    > hist(waiting)
    > hist(eruptions)
    > qqnorm(waiting,col="red")
    > qqline(waiting,col="blue")
    > qqnorm(eruptions,col="red")
    > qqline(eruptions,col="blue")

![](/media/files/2012/02/24/multi-grid.png)

par命令前面已经用到过，mfrow=c(2,2)表示的就是设定一个2x2的多重框图，mfrow指的是逐行输入。也就是说我们还可以用nfcol来逐列输入图像。第二行的qqnorm是用来诊断数据分布是否近似正态分布；qqline是在正态分位图中加上参考线，图中的点越接近这条线就表示越接近正态分布。很明显，这些数据并非近似一个单一的正态分布，而是有两个正太分布组成的混合分布。

# 图像的修饰
R中提供了许多修饰图像的办法。我们用前面用过的鸢尾花数据为例。下面就是Sepal.Length的直方图，在图中加上了正态分布曲线。
    
    > hist(Sepal.Length,freq=F,ylim=c(0,0.5))
    > par(mfrow=c(1,1))
    > hist(Sepal.Length,freq=F,ylim=c(0,0.5))

这里需要一些说明，一般的直方图y轴的尺度都是频率而并非是密度。但是我们要加上正态密度线，尺度也就必须要是密度。hist()中的子变量freq=F就是指的y轴尺度用频率而不用密度。此外，ylim=c(0,0.5)就是要设定y轴的上下限为0和0.5。否则，R就会选择最合适的上下限：0, 0.4。这样的话，我们就不能完全看到完整的正态分布曲线了。下面的命令就是来绘制正态分布曲线的：

    > par(x,dnorm(x,m,s),lty=2)
    > m=mean(Sepal.Length)
    > s=sd(Sepal.Length)
    > x=seq(4,8,0.1)
    > lines(x,dnorm(x,m,s),lty=2,col="red")

![](/media/files/2012/02/24/pic-modify.png)

在R中，画图可以用很多种不同的颜色，这一点我前面已经用的很多了。除此之外，还有一些更好也更有意义的应用，比如我们可以在前面的鸢尾花数据中，使用不同的颜色来代表不同的品种。
  
    > plot(Petal.Length,Petal.Width,bg=c("red","green","blue")[Species],pch=21)

在iris中，Species是存储鸢尾花品种的数据：分别是setosa、versicolor和virginica。最后的pch=21就是指用圆形绘制数据点。当然了，R还提供了很多形状可供选择，具体可以用help(points)来查看。

![](/media/files/2012/02/24/apply-color.png)

当然了，在一些黑白的书籍中，就得需要用不同的形状来表述了。
  
    > plot(Petal.Length,Petal.Width,pch=c("S","C","V")[Species])

![](/media/files/2012/02/24/apply-word.png)

我们可以在图上面添加一些直线来对点进行划分：

![](/media/files/2012/02/24/line-classify.png)

当然了，abline不仅可以加水平/竖直线，还可以添加一些稍微复杂一些的斜线：[latex]y=k\times x+b[/latex]。我们继续以前面的喷泉数据为例来说明，根据eruptions=3.2来画三条线：

  1. 包括所有数据点
  2. 只包含eruptions<3.2
  3. 只包含eruptions>=3.2

    > plot(eruptions,waiting,main="Old faithful喷泉",col="blue")
    > abline(lsfit(eruptions,waiting))
    > d1=faithful[eruptions<3.2,]
    > d2=faithful[eruptions>=3.2,]
    > abline(lsfit(d1$eruptions,d1$waiting),lty=2)
    > abline(lsfit(d2$eruptions,d2$waiting),lty=2)
    > legend(1.7,95,c("所有","x<3.2","x>=3.2"),lty=c(1:3))

![](/media/files/2012/02/24/add-three-line.png)

首先绘制waiting对eruptions的散点图及第一条最小平方线。然后我们选取了eruptions<3.2的数据点并储存在d1，其余部分存储在d2。lsfit()来回传最小平方的截距a及斜率b。lty=2是表示用虚线，lty=3表示用实线来画。

## 多重图格
前面我们经常会用到par(mfrow=)或者par(mfcol=)来定义多重框图，这里还有一些比较特殊的办法来画多重框图。继续用EuStockMarkets数据的DAX指数为例：

![](/media/files/2012/02/24/multi-pic.png)

明显这不属于常规图，下面简述画图步骤。我们先选定数据，定义u及设定一个3x2的多重框图。在第一行绘制正态分位图和直方图：

    > data(EuStockMarkets)
    > d=data.frame(EuStockMarkets)
    > View(d)
    > t=as.ts(d$DAX)
    > u=(lag(t)-t)/t
    > par(mfrow=c(3,2))
    > hist(u)
    > qqnorm(u)

由于第二行只有一幅图像，我们用par(mfrow=c(3,1))重新设定多重框图为3x1，然后再用指令par(mfg=c(2,1,3,1))去设定绘图的位置。注意：在c(2,1,3,1)的前两个数字是代表想要绘制图的位置，最后两个数字是目前画多重框图的定义。然后画出u的时间序列。

    > par(mfrow=c(3,1))
    > par(mfg=c(2,1,3,1))
    > plot(u)

最后，我们变换回来。并画出u的自相关函数和偏自相关函数：

    > par(mfrow=c(3,2))
    > par(mfg=c(3,1,3,2))
    > acf(u)
    > pacf(u)