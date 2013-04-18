---
layout: post
title: R语言学习笔记--统计模型（二）
categories:
- research
tags:
- R language
- Statistics
---

## 引言
上一篇中介绍了线性回归模型、方差分析和广义的线性模型。这篇主要来介绍其他常用的统计模型。在R软件中也有很多程序库，它们都包含了很多的函数。我们在开启R的时候，就会自动加载base和stats这两个库。比如我们前面用到的lm()、aov()、glm()以及i前面提到的内置函数等等，都是和可以在这两个库里面找到的。可以用library(help=stats)或library(help=base)来进行查看。

这一篇笔记里面介绍到的统计模型不一定能在stats或者base里面找到。建立这些统计模型的时候可能要预先加载特定的库文件。会在程序中说明。

## 线性判别分析
R内置的数据里面有鸢尾花的数据。共有5列：Sepal.Length（花萼长度）、Sepal.Width（花萼宽度）、Petal.Length（花瓣长度）、Petal.Width（花瓣宽度）、Species（品种）。品种一共有三种：setosa、versicolor以及virginica。每个品种各有50行，即一共有150行。我们首先来读取iris：

    > data(iris)
    > attach(iris)
    > names(iris)
    [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width"  "Species"

接下来我们用判别分析，用花萼和花瓣的长和宽，来判别鸢尾花的品种。

    > library(MASS)
    > iris.lda=lda(Species~Sepal.Length+Sepal.Width+Petal.Length+Petal.Width)
    > iris.lda
    Call:
    lda(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width)
    
    Prior probabilities of groups:
        setosa versicolor  virginica
     0.3333333  0.3333333  0.3333333 
    
    Group means:
               Sepal.Length Sepal.Width Petal.Length Petal.Width
    setosa            5.006       3.428        1.462       0.246
    versicolor        5.936       2.770        4.260       1.326
    virginica         6.588       2.974        5.552       2.026
    
    Coefficients of linear discriminants:
                        LD1         LD2
    Sepal.Length  0.8293776  0.02410215
    Sepal.Width   1.5344731  2.16452123
    Petal.Length -2.2012117 -0.93192121
    Petal.Width  -2.8104603  2.83918785
    
    Proportion of trace:
       LD1    LD2
    0.9912 0.0088
    
    > plot(iris.lda,col="blue")

我们首先需要加载MASS库，再调用其中的lda()函数来做线性判别分析，语法与前面类似。最后用输出iris.lda：

输出结果包括了每组的平均向量，线性判别函数的系数等等。有关线性判别的理论，可以参考多元统计分析的有关书籍。

![](/media/files/2012/02/16/linear-classify.png)

需要注意的是，R的内置函数predict()，可以将lda()的输出应用于原本iris的数据进行预测。从而对比真正的品种，看看误差率有多少。

    > iris.id=predict(iris.lda)$class
    > table(iris.id,Species)
                Species
    iris.id      setosa versicolor virginica
      setosa         50          0         0
      versicolor      0         48         1
      virginica       0          2        49

在predict()函数输出中class这一项，储存预测的品种。我们应用了table()去列表预测与真正品种的比较。从数据中得到，在150个数据中，只有3处错误，误差率为2%。而有两朵versicolor的鸢尾花被误认为virginica，一朵virginica的鸢尾花被误认为versicolor。

## 聚类分析（分层）
上面的例子中，我们知道了每一朵鸢尾花的品种并应用了这些数据。假设我们只知道数据内有三种品种而不知道每朵花的真正品种，就只凭花萼及花瓣的长度和宽度去分类成三组，这就是聚类分析。这也是我第一次用R来做聚类，以前都是在SPSS里面做的。由于Linux版本的SPSS属于商业软件，收费也在所难免，自然选择免费的R了。聚类分析比判别分析更困难，误差率也较高。聚类分析通常有两种方法：分层和非分层。R的内置函数hclust()和kmeans()，分别就是这两种聚类方法的函数。当然了，无论用哪种方法，都需要计算每对数据之间的距离或非相似性。R有内置函数dist()去计算每对数据的距离矩阵。下面先说说分层聚类。

    > iris.hc=hclust(dist(iris[,1:4]))
    > plot(iris.hc)
    > plot(iris.hc,col="blue")

hclust()就是R的分层聚类分析函数，它的输入项是距离矩阵。dist()正好计算iris的距离矩阵，它用的是欧式距离。最后我们画出了树枝型分类图。

![](/media/files/2012/02/16/lay-classify.png)

在这张图上面有些东西还是看不出来的的，比如数据量太多，树枝型分类图就没办法很清楚地列出没一点的分类的。不过我们可以使用内置函数cutree()，将数据点编制若干组，每个数据点都附上有组别的标签。

    > iris.id=cutree(iris.hc,3)
    > table(iris.id,Species)
           Species
    iris.id setosa versicolor virginica
          1     50          0         0
          2      0         23        49
          3      0         27         1

以上指令就是根据hclust的输出：iris.hc，我们将iris编成三组，标签1,2和3存储在iris.id。然后我们将iris.id与真正的品种Species作比较。从上面的数据来看，有23+1朵花被误人，误差率为16%。

## 聚类分析（非分层）
k-means是一种常用的非分层的聚类分析方法。kmeans()就是R的k-means聚类分析函数。需要注意的是，kmeans()的输入是数据矩阵而非距离矩阵，而使用者还要输入组别的数目。下面的kmeans()指令将iris数据分成三组，结果储存于iris.km内：

    > iris.km=kmeans(iris[,1:4],3)
    > table(iris.km$cluster,Species)
       Species
        setosa versicolor virginica
      1      0          2        36
      2     50          0         0
      3      0         48        14

从上面的结果来看，误差率为(14+2)/150=10.67%，较hclust()更好一些。

## 分类树
上面提到的聚类分析，虽然可以将数据分门别类，但未能提供数据分类的法则。分类树就是一个既可将数据分类，也可以提供分类法则的一个方法。不过，分类书就像判别分析一样，是需要数据类别的，而并非像聚类分析。R的rpart的rpart()函数可以用来建立分类树。

    > library(rpart)
    > iris.ct=rpart(Species~Sepal.Length+Sepal.Width+Petal.Length+Petal.Width,method="class")

这里需要说明一下method参数，由于Species是属于分类的范畴，我们用class。其他的选项如method=anova，possion，exp等等，可以自行参照help(rpart)。存储结果之后继续画图：

    > plot(iris.ct,asp=2,main="iris分类树")
    > text(iris.ct,use.n=T,cex=0.6,col="blue")

![](/media/files/2012/02/16/iris-classify-tree.png)

需要说明有几点：asp=2指的是比例，cex=0.6表示的文字放大倍数，用来调节树形图的比例，从而让图和文字有比较好的配合。当然了，这些比例都需要多次尝试。use.n=T是显示终点的数据。

图上的数据标识的是简易的分类法则：

如果 Petal.Length < 2.45, Species = setosa (50/0/0);

否则，如果Petal.Width < 1.75, Species = versicolor (0/49/5);

否则 Species = virginica (0/1/45)。

在每个终点的三个数字分别是这三种鸢尾花的分布（第一个数字是setosa的数目，第二个是versicolor，第三个是virginica）。从图上的判断结果可以看到误差率有6/150=4%，还是不错的。

假如我们想用列别分类树的结果与真正品种，我们先要做好每朵花的分类标签。

    > prob=predict(iris.ct)
    > iris.id=1*(prob[,1]>1/3)+2*(prob[,2]>1/3)+3*(prob[,3]>1/3)
    > table(iris.id,Species)
           Species
    iris.id setosa versicolor virginica
          1     50          0         0
          2      0         49         5
          3      0          1        45

首先，predict(iris.ct)回传三列概率向量，分别代表这朵花属于setosa, versicolor以及virginica的机会。最自然的想法就是以概率1/3作为临界点。最后用table()表列iris.id和真正品种。

## 人工神经网络

分类方法当然还有牛逼的玩意，对，AI的部分来了。还是以前面的数据为例，输入数目为4,输出数目为1，。为简单起见，设定只有一层中间层，中间层也只有两个神经元。

    > library(nnet)
    > sp=as.numeric
    > sp=as.numeric(Species)
    > iris.nn=nnet(sp~Sepal.Length+Sepal.Width+Petal.Length+Petal.Width,size=2,linout=T)
    # weights:  13
    initial  value 444.507451
    iter  10 value 7.335368
    iter  20 value 3.185719
    iter  30 value 2.282656
    iter  40 value 2.059564
    iter  50 value 1.947865
    iter  60 value 1.698321
    iter  70 value 1.629689
    iter  80 value 1.443660
    iter  90 value 1.069777
    iter 100 value 1.049964
    final  value 1.049964
    stopped after 100 iterations

当然，随着nnet()开始的自变量的不同，每次执行命令的时候也会得到不同的结果。我们也可以用summary(iris.nn)来显示更详细的结果：

	> summary(iris.nn)
	a 4-2-1 network with 13 weights
	options were - linear output units
	b->h1  i1->h1  i2->h1  i3->h1  i4->h1
	-7.36   -1.71  -15.30   22.19   20.57
	b->h2  i1->h2  i2->h2  i3->h2  i4->h2
	-117.49  -55.37  -52.12   97.08   79.40
	b->o   h1->o   h2->o
	0.99    1.00    0.99

上面的就是这个神经网络训练之后的自变量的值。这些自变量代表了下面的方程式：

$$h1=-7.36-1.71x_1-15.30x_2+22.19x_3+20.57x_4$$

$$h2=-117.49-55.37x_1-52.12x_2+97.08x_3+79.40x_4$$
$$e\^{c}sss$$
$$e\^c sss$$
$$h1=\frac{e^{h1}{1+e^h1}$$

$$h2=\frac{e^{h_2}} {1+e^{h_2}}$$

$$v=0.96-37.96h_1+2.47h_2$$

前面两个是连接输入和h1, h2的模型，第三个是神经元的激励函数。最后一个是连接神经元与输出的模型。人工神经网络的输出存储在iris.nn的fitted.values。它是一个实数的向量，接近1代表这朵花是setosa，2和3类似。我们需要用round()将fitted.values转化成最近的整数，这就是每朵花的标签了。
    
    > iris.id=round(iris.nn$fitted.values)
    > table(iris.id,Species)
           Species
    iris.id setosa versicolor virginica
          1     50          0         0
          2      0         49         0
          3      0          1        50

从结果来看，误差率为4/150=2.67%，应该还是可以接受的。