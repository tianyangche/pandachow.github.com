---
layout: post
categories: research
title: R语言学习笔记--统计模型（一）
tags: [R language, Statistics]
---

昨天中午连续睡了17个小时的结果就是今天早晨7点便醒了，真是感觉没有什么可以比酣睡一场更能让人满足的了。起床后泡了杯咖啡，一边搅着一边看着窗外的星星。几个小时前小屋子里面的景象放佛还历历在目：不知疲倦摇着头的取暖器、充满喜感的鸡蛋图、肉沫茄子和土豆肉丝。

多罗嗦了几句，下面是前段时间的统计模型学习笔记。主要涉及到的是线性回归模型、方差分析、逻辑斯特模型。

# 回归模型
回归模型利用自带的faithful数据来示例，faithful是某位地质学家在黄石公园旅游景点"Old Faithful"间歇泉所记录的喷发数据。这个数据包括两组向量，它们分别是泉水的持续时间按(eruptions)(以分钟计)和喷发间隔时间(waiting)(以分钟计)。下面我们来简单画张它的关系图。
    
    > data(faithful)
    > attach(faithful)
    > names(faithful)
    [1] "eruptions" "waiting"
    > plot(eruptions,waiting,col="blue")

[![](http://panda0411.com/wordpress/wp-content/uploads/2012/02/eruptionwaiting.png)](http://panda0411.com/wordpress/wp-content/uploads/2012/02/eruptionwaiting.png)

从这张图里可以发现，waiting和eruptions之间基本呈现出正相关，即随着这次喷发持续时间的增长，下一次的喷发就是相距越远。我们继续尝试用eruptions来解释waiting。lm函数就是用来建立线性回归模型，命令如下：
   
    > lm(waiting~eruptions)
    
    Call:
    lm(formula = waiting ~ eruptions)
    
    Coefficients:
    (Intercept)    eruptions
          33.47        10.73

函数lm拟合了一个线性回归模型：

[latex]waiting=33.47+10.73eruptions[/latex]

并建立了一个属于线性回归模型的对象，并传回各个变量系数和其他不同的资料。当然，这个变量方便的话还是应该保存起来。下面可以用plot函数对这个回归模型作诊断检验。
    
    > par(mfrow=c(2,2))
    > plot(lm(waiting~eruptions),col="blue")

[![](http://panda0411.com/wordpress/wp-content/uploads/2012/02/faithful.lm_.png)](http://panda0411.com/wordpress/wp-content/uploads/2012/02/faithful.lm_.png)

指令par(mfrow=c(2,2))可以将R的输出窗口设定成为2行2列，下次输入par(mfrow=c(1,1))即可恢复默认设置。

这四张图里面显示一些比较有用的诊断信息：残余图、正态分位图、曲氏距离等等。关于曲氏距离，我自己是第一次涉及，wiki一大概代表的是每一点对回归线的影响力的大小，数值越大表示影响力越大。

## 多元回归模型
R的内置档案stackloss，记录了由氧化氨气而制造硝酸的数据。数据包括4列：Air.Flow(空气流量)、Water.Temp(水温)、Acid.Conc.(硝酸浓度)、stack.loss(氨气损失之百分比)。
  
    > data(stackloss)
    > attach(stackloss)
    > stackloss.lm=lm(stack.loss~Air.Flow+Water.Temp+Acid.Conc.)
    > summary(stackloss.lm)
    
    Call:
    lm(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.)
    
    Residuals:
        Min      1Q  Median      3Q     Max
    -7.2377 -1.7117 -0.4551  2.3614  5.6978 
    
    Coefficients:
                Estimate Std. Error t value Pr(>|t|)
    (Intercept) -39.9197    11.8960  -3.356  0.00375 **
    Air.Flow      0.7156     0.1349   5.307  5.8e-05 ***
    Water.Temp    1.2953     0.3680   3.520  0.00263 **
    Acid.Conc.   -0.1521     0.1563  -0.973  0.34405
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
    
    Residual standard error: 3.243 on 17 degrees of freedom
    Multiple R-squared: 0.9136,	Adjusted R-squared: 0.8983
    F-statistic:  59.9 on 3 and 17 DF,  p-value: 3.016e-09

从以上结果能够得到这个多元线性回归模型为：

[latex]stack.loss=-39.9197+0.7156Air.Flow+1.2953Water.Temp-0.1521Acid.Conc.[/latex]

最后一个[latex]p-[/latex]的值非常小(3.016e-09)，是表示并非所有的自变量都没用，但也不是每一个自变量都有用。其中Acid.Conc.的[latex]p-[/latex]值非常高(0.344)，因此Acid.Conc.应该首先被移除。重新输入新的回归模型：

    > stackloss.lm=lm(stack.loss~Air.Flow+Water.Temp)
    > summary(stackloss.lm)
    
    Call:
    lm(formula = stack.loss ~ Air.Flow + Water.Temp)
    
    Residuals:
        Min      1Q  Median      3Q     Max
    -7.5290 -1.7505  0.1894  2.1156  5.6588 
    
    Coefficients:
                Estimate Std. Error t value Pr(>|t|)
    (Intercept) -50.3588     5.1383  -9.801 1.22e-08 ***
    Air.Flow      0.6712     0.1267   5.298 4.90e-05 ***
    Water.Temp    1.2954     0.3675   3.525  0.00242 **
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
    
    Residual standard error: 3.239 on 18 degrees of freedom
    Multiple R-squared: 0.9088,	Adjusted R-squared: 0.8986
    F-statistic: 89.64 on 2 and 18 DF,  p-value: 4.382e-10

我们可以看到新的拟合的多元回归模型为：

[latex]stack.loss=-50.3588+0.6712Air.Flow+1.2954Water.Temp[/latex]

结果也比较理想，最后我们还是对回归模型作诊断检验：
    
    > par(mfrow=c(2,2))
    > plot(stackloss.lm,col="blue")

[![](http://panda0411.com/wordpress/wp-content/uploads/2012/02/stackloss.lm_.png)](http://panda0411.com/wordpress/wp-content/uploads/2012/02/stackloss.lm_.png)

从上面的图来看，第21点和第1点的曲式距离非常大。这样的情况下，我们优先移除这两点。

    > stackloss.lm=lm(stack.loss~Air.Flow+Water.Temp+Acid.Conc.,subset=c(-4,-21))
    > plot(stackloss.lm,col="blue")

[![](http://panda0411.com/wordpress/wp-content/uploads/2012/02/Rplot.png)](http://panda0411.com/wordpress/wp-content/uploads/2012/02/Rplot.png)

移除了1和21点之后，基本上就没什么问题了。

# 方差分析模型
R内置数据里面PlantGrowth记录了用不同肥料种植植物的重量。

    > data(PlantGrowth)
    > attach(PlantGrowth)
    > names(PlantGrowth)
    [1] "weight" "group" 
    > group=as.factor(group)

这组数据中一共有3个组别，控制组和两种肥料种植组。我们首先要将group转换成因子。然后我们用盒形图来表示，并做简要的方差分析。

    > plot(group,weight,main="植物重量",xlab="肥料")
    > summary(aov(weight~group))
                Df Sum Sq Mean Sq F value Pr(>F)  
    group        2  3.766  1.8832   4.846 0.0159 *
    Residuals   27 10.492  0.3886                 
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

[![](http://panda0411.com/wordpress/wp-content/uploads/2012/02/植物重量.png)](http://panda0411.com/wordpress/wp-content/uploads/2012/02/植物重量.png)

通过方差分析我们发现，由于p-的值非常小(0.01591)，所以这三个组别的植物的重量有着比较显著的差别。最后照例进行诊断检验。

[![](http://panda0411.com/wordpress/wp-content/uploads/2012/02/植物重量，方差.png)](http://panda0411.com/wordpress/wp-content/uploads/2012/02/植物重量，方差.png)