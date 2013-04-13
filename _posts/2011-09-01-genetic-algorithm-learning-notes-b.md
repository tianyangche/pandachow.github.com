---
layout: post
title: 遗传算法学习笔记(二)
categories:
- 伪技术
tags:
- MATLAB
- 人工智能
- 数学建模
- 算法
- 遗传
---

上一篇简单讲了讲GA的理论部分, 其实就是非常简略的描述了一下实现的步骤, 小白可以用来普及 GA~, 老鸟就跳过把, 哈哈. 这次再来说说 GA 在 MATLAB 中的实现. MATLAB实现有两种方式: GA toolbox 或者自己写函数.

**GA工具箱**
鉴于遗传算法数学理论太过于 boring, 也为了降低程序的开发难度, matlab 已经讲遗传算法命令进行了封装, 作成了专门的工具箱--GA Toolbox, 方便用户的调用, 对于这个工具箱有几点需要说明一下:
1. 现在貌似matlab各个版本下的GA Toolbox版本不一样, 功能和用法也不统一....= =
2. 有一些比较非主流的盗版的matlab中可能GA Toolbox会有所缺失, 或者是部分功能无法使用, 以前google过, 貌似是破解序列号的原因, 而且破解方法不一样会导致工具箱的丢失程度也不同....= =
3. 由于这个遗传算法的工具箱写得并不好, 虽然为了操作方便, 对各个命令都做好了集成. 但是封装好的工具箱的内部命令不能依据我们的需要进行相应的调整和修改, 所以个人还是建议尽量自己去编写GA的函数, 不要过于依赖这个过于傻瓜的Toolbox.

下面讲讲这个工具箱的核心函数

**1. ga**
语法格式: [x, fval, reason]=ga(@fitnessfun, nvars, options)
其中, x是经过遗传进化以后自变量最佳染色体返回值; fval位最佳染色体的适应度; reason为算法停止的原因; @fitnessfun为适应度句柄函数; nvars为目标函数自变量的个数; options为算法的属性设置, 该属性是通过函数gaoptimset设置的.

**2. gaoptimset**
语法格式: options=gaoptimset('PropertyName1',value1,....)
这个函数用来设置一些ga的参数和句柄函数, 更多请自行help gaoptimset

下面举个例子来说明, 求解函数
$$!max f(x)=200e^{-0.05x}sin(x), x\in[-2, 2]$$

    
    %主程序, 求解y=200*exp*(-0.05*x).*sin(x)在[-2 2]区间上的最大值
    function f=lbw(x)
    if(x2)
        f=500;
    else
        f=-200*exp(-0.05*x).*sin(x);%求最大值, 此处适应度函数用$$-f(x)$$
    end


输入命令, 或者另建m脚本执行

    
    clear;
    clc;
    options=gaoptimset('Generations',1000,'StallGenLimit',800,'PlotFcns',@gaplotbestf);
    [x,f]=ga(@lbw,1,options)


结果如图
x =
1.5212
f =
-185.1242
适应度变化趋势如下图
[![](http://panda0411.com/wordpress/wp-content/uploads/2011/09/GA01.jpg)](http://panda0411.com/wordpress/wp-content/uploads/2011/09/GA01.jpg)

**MATLAB的遗传算法程序设计**
遗传算法虽然有工具箱, 但是这个工具箱真的不是万能的, 很多情况下需要具体问题具体对待. 另外说句实话, 仅仅是使用工具箱对于从本质上理解遗传算法是没有什么好处的. 下面就尝试对前面同样的问题进行matlab程序编写和求解.

主程序GA.m

    
    %主程序：用遗传算法求解y=200*exp(-0.05*x).*sin(x)在[-2 2]区间上的最大值
    clc;
    clear all;
    close all;
    global BitLength
    global boundsbegin
    global boundsend
    bounds=[-2 2];%一维自变量的取值范围
    precision=0.0001; %运算精度
    boundsbegin=bounds(:,1);
    boundsend=bounds(:,2);
    %计算如果满足求解精度至少需要多长的染色体
    BitLength=ceil(log2((boundsend-boundsbegin)' ./ precision));
    popsize=50; %初始种群大小
    Generationnmax=12;  %最大代数
    pcrossover=0.90; %交配概率
    pmutation=0.09; %变异概率
    %产生初始种群
    population=round(rand(popsize,BitLength));
    %计算适应度,返回适应度Fitvalue和累积概率cumsump
    [Fitvalue,cumsump]=fitnessfun(population);
    Generation=1;
    while Generation


子程序：函数名称存储为crossover.m

    
    %新种群交叉操作
    function scro=crossover(population,seln,pc);
    BitLength=size(population,2);
    pcc=IfCroIfMut(pc);  %根据交叉概率决定是否进行交叉操作，1则是，0则否
    if pcc==1
       chb=round(rand*(BitLength-2))+1;  %在[1,BitLength-1]范围内随机产生一个交叉位
       scro(1,:)=[population(seln(1),1:chb) population(seln(2),chb+1:BitLength)];
       scro(2,:)=[population(seln(2),1:chb) population(seln(1),chb+1:BitLength)];
    else
       scro(1,:)=population(seln(1),:);
       scro(2,:)=population(seln(2),:);
    end


子程序：函数名称存储为fitnessfun

    
    %计算适应度函数
    function [Fitvalue,cumsump]=fitnessfun(population);
    global BitLength
    global boundsbegin
    global boundsend
    popsize=size(population,1);   %有popsize个个体
    for i=1:popsize
       x=transform2to10(population(i,:));  %将二进制转换为十进制
        %转化为[-2,2]区间的实数
    xx=boundsbegin+x*(boundsend-boundsbegin)/(power((boundsend),BitLength)-1);
       Fitvalue(i)=targetfun(xx);  %计算函数值，即适应度
    end
    %给适应度函数加上一个大小合理的数以便保证种群适应值为正数
    Fitvalue=Fitvalue'+230;
    %计算选择概率
    fsum=sum(Fitvalue);
    Pperpopulation=Fitvalue/fsum;
    %计算累积概率
    cumsump(1)=Pperpopulation(1);
    for i=2:popsize
       cumsump(i)=cumsump(i-1)+Pperpopulation(i);
    end
    cumsump=cumsump';


子程序：新种群变异操作，函数名称存储为mutation.m

    
    %新种群变异操作
    function snnew=mutation(snew,pmutation);
    BitLength=size(snew,2);
    snnew=snew;
    pmm=IfCroIfMut(pmutation);  %根据变异概率决定是否进行变异操作，1则是，0则否
    if pmm==1
       chb=round(rand*(BitLength-1))+1;  %在[1,BitLength]范围内随机产生一个变异位
       snnew(chb)=abs(snew(chb)-1);
    end


子程序：判断遗传运算是否需要进行交叉或变异, 函数名称存储为IfCroIfMut.m

    
    %判断遗传运算是否需要进行交叉或变异
    function pcc=IfCroIfMut(mutORcro);
    test(1:100)=0;
    l=round(100*mutORcro);
    test(1:l)=1;
    n=round(rand*99)+1;
    pcc=test(n);


子程序：函数名称存储为selection.m

    
    %新种群选择操作
    function seln=selection(population,cumsump);
    %从种群中选择两个个体
    for i=1:2
       r=rand;  %产生一个随机数
       prand=cumsump-r;
       j=1;
       while prand(j)


子程序: 函数名称存储为transform2to10.m

    
    %将2进制数转换为10进制数
    function x=transform2to10(Population);
    BitLength=size(Population,2);
    x=Population(BitLength);
    for i=1:BitLength-1
       x=x+Population(BitLength-i)*power(2,i);
    end


子程序：函数名称存储为targetfun.m

    
    %对于优化最大值或极大值函数问题，目标函数可以作为适应度函数
    function y=targetfun(x); %目标函数
    y=200*exp(-0.05*x).*sin(x);


本程序输出结果:
	Bestpopulation =
	1.4858
	Besttargetfunvalue =
	185.0100
平均适应度和种群最大适应度的变化趋势如图:
[![](http://panda0411.com/wordpress/wp-content/uploads/2011/09/GA02.jpg)](http://panda0411.com/wordpress/wp-content/uploads/2011/09/GA02.jpg)
