---
layout: post
title: Monte Carlo模拟搞定多约束非线性规划
categories:
- research
tags:
- MATLAB
- Algorithm
- Monte Carlo
---

多约束非线性规划在CUMCM中不论是在组建模型和求解模型方程中都是重头戏，今天在比赛前来重点复习复习这种求解此类问题的“杀手锏”，而且从理论上来讲，只要是应用 Monte Carlo 方法都能在一定程度上顺利求解。下面来以一个例子来说明。

$$!max\, f(x)=x_1x_2$$
$$!s.t.\begin{cases}-x_1+2x_2+2x_3\geq0\\x_1+2x_2+2x_3\leq 72\\10\leq x_2\leq20\\x_1-x_2=20\end{cases}$$

易得：$$20\leq x_1\leq$$，$$10\leq x_2\leq 20$$，$$-10\leq x_3 \leq 16$$，编写下面这个程序MonteCarlo_01.m

## 最初的程序

    %程序运行前在“file”菜单下“preferences”将输出格式调成“long”格式
    clc;clear all;
    N=1000;
    x10=[];x20=[];x30=[];vmax=-inf;
    x1=unifrnd(20,30,N,1);
    x2=unifrnd(10,20,N,1);
    x3=unifrnd(-10,16,N,1);
    for i=1:N
        for j=1:N
            for k=1:N
                if -x1(i)+2*x2(j)+2*x3(k)>=0&...
                   x1(i)+2*x2(j)+2*x3(k)vmax,
                   vmax=v;x10=x1(i);x20=x2(j);x30=x3(k);
               end
                end
            end
        end
    end
    x=[x10,x20,x30],vmax

输出：
    
    x =
    []
    vmax =
    -Inf

计算机输出结果 vmax=-Inf 是因为$$x_1-x_2=10$$使可行域在三维立方体中导致体积搭建失败。因为本来约束条件构成的是三维空间，但是等式约束因为随机值难以满足导致约束条件搭建的三维立体其实是一个平面，从而导致体积为0，关键原因是产生的随机数刚好使$$x_1-x_2=10$$是不可能的事件，简单理解一下就是那么多的数字中间，恰好形成$$x_1-x_2=10$$的几率实在是太小了。。

## 第一步修正后的程序

将$$x_1-x_2=10$$，改为abs$$(x_1-x_2-10)<1e-3$$
  
    clc;clear all;
    % T1=clock;
    N=1000;
    x10=[];x20=[];x30=[];vmax=-inf;
    x1=unifrnd(20,30,N,1);
    x2=unifrnd(10,20,N,1);
    x3=unifrnd(-10,16,N,1);
    for i=1:N
        for j=1:N
            for k=1:N
                if -x1(i)+2*x2(j)+2*x3(k)>=0&...
                   x1(i)+2*x2(j)+2*x3(k)vmax,
                   vmax=v;x10=x1(i);x20=x2(j);x30=x3(k);
               end
                end
            end
        end
    end
    x=[x10,x20,x30],vmax

输出：

    x =
    22.4780 12.4772 12.2799
    vmax =
    3.4440e+03

## 问题化简

仔细审题，将原题化为三维问题得到：$$max f(x)=(x_2+10)x_2x_3$$，约束条件成为
$$!x_2+2x_3\geq 12, 3x_2+2x_3\leq 62, 10\leq x_2\leq 20, -5\leq x_3\leq 16$$
编写程序MonteCarlo_03.m：
    
    clc;clear all;
    % T1=clock;
    N=31623;
    x20=[];x30=[];vmax=-inf;
    x2=unifrnd(10,20,N,1);
    x3=unifrnd(-5,16,N,1);
        for j=1:N
            for k=1:N
                if x2(j)+2*x3(k)>=10&...
                   3*x2(j)+2*x3(k)vmax,
                   vmax=v;x20=x2(j);x30=x3(k);
               end
                end
            end
        end
    x=[x20+10,x20,x30],vmax

输出：
    
    x =
    22.5833 12.5833 12.1250
    vmax =
    3.4456e+03

## 修改循环
在上一个程序的基础上，将原来的两重循环改成一重循环，最终程序MonteCarlo_4.m如下：
    
    clc;clear all;
    % T1=clock;
    N=1000000;
    M=1000;
    vmax=-inf;
    for i=1:M
    x2=unifrnd(10,20,N,1);
    x3=unifrnd(-5,16,N,1);
    cc=find(x2+2*x3>=10&3*x2+x*x3vmax,
                  vmax=v;
                  x=[x2(cc(Index_max))+10,x2(cc(Index_max)),x3(cc(Index_max))];
               end
    end
    vmax
    x

输出：
    
    vmax =
    3.4456e+03
    x =
    22.5853 12.5853 12.122

## 尾声
这个小例子一共运用了4种不同的方法求解，虽然思路都是MonteCarlo模拟，并且也都是先产生足够数量的随机数，然后用约束条件检验这些随机数是否合乎要求，最后带入目标函数取最大值（每种方法产生的随机数总量都是相同的），但是可以看到的是输出结果的精度和开销的时间都是不相同的（MATLAB里面前面三个时间差不太多，但是最后一个时间非常长，CPU: SP9400，R2010b，几乎是2分钟左右），可见如何正确选择求解方法在MonteCarlo模拟中显得尤为重要。
MonteCarlo的缺点其实我们也都看到了，就是花费的时间太长了，浪费资源而且精度不是很高，但是不得不说有时候这也是求解的唯一办法。其实一般的数学模型误差都是非常大的，MC算法导致的误差相对来说已经算是比较小的了。