---
layout: post
title: 传递函数的表示
categories:
- research
tags:
- MATLAB
- Transfer Function
---

简单对几种传递函数的表示方法进行一些罗列解释, 权当是复习和笔记.

**1. 显示多项式传递函数**
$$!\phi(s)=\frac{13s^3+4s^2+6}{5s^4+3s^3+16s^2+s+7}$$
分子和分母各项系数按照降次排列, 分别计入num=[], den=[], 缺项系数补零
建立传递函数模型:

    
    sys=tf(num,den)


运行代码如下:

    
    num[13 4 0 6];
    den=[5 3 16 1 7];
    sys=tf(num,den)


**2. 因子形式传递函数转化成位多项式传递函数**
$$!\phi(s)=\frac{4(s+3)(s^2+7s+6)^2}{s(s+1)^3(s^3+3s^2+5)}$$
这里要介绍conv函数, 主要用来实现两个多项式降次系数乘积运算, 注意体会它的右结合性

    
    num=4*conv([1 3], conv([1 7 6], [1 7 6]));
    den=conv([1 0], conv([1 1], conv([1 1], conv([1 1], [1 3 0 5]))));
    sys=tf(num, den)


**3. 显示零极点式传递函数**
$$!G(s)=\frac{7(s+3)}{(s+2)(s+4)(s+5)}$$
这里需要介绍zpk函数的用法:

    
    z=[ ]; %输入零点
    p=[ ]; %输入极点
    k= %输入增益


函数用法如下:

    
    z=[-3];
    p=[-2 -4 -5];
    k=7;
    sys=zpk(z,p,k)


**4. 传递函数的多项式形式与零极点形式转换**

也就是tf形式和zpk形式的转换

    
    [num, den]=zp2tf(z, p, k); %zpk转换至tf
    [z, p, k]=tf2zp(num, den); %tf转换至zpk


$$!G(s)=\frac{4(s+7)(s+2)}{(s+3)(s+5)(s+9)}$$

    
    %零极点形式转换为多项式形式
    z=[-7 -2]' ; %必须是列向量
    p=[-3 -5 -9]' %必须是列向量
    k=4;
    spk=zpk[z, p, k]; %零极点形式传递函数
    [num, den]=zp2tf(z, p, k);
    stf=tf(num, den); %多项式形式传递函数



    
    %多项式形式转换为零极点形式
    num=[4 36 56]; %必须是行向量
    den=[1 17 87 135]; %必须是行向量
    stf=tf(num, den); %多项式形式传递函数
    [z, p, k]=tf2zp(num, den);
    spk=zpk(z, p, k) %零极点形式传递函数


**5. 目标函数的串联**
$$!G_1(s)=\frac{2}{(s+2)(2s+1)}$$
$$!G_2(s)=\frac{7s+3}{5s^2+2s+1}$$
这里需要介绍函数series的用法

    
    %使用方法主要有这三种, 详细的用法请查询帮助文档
    G=G1*G2;
    G=series(G1, G2);
    [num, den]=series(num1, den1, num2, den2)


下面是一例求解两个多项式串联传递函数

    
    %求解传递函数串联
    %求解g1传递函数
    z1=[ ]' ;
    p1=[-3, -1/2]' ;
    k1=1;
    g1zp=zpk(z1, p1, k1);
    [n1, d1]=zp2tf(z1, p1, k1);
    
    % 求解g2传递函数
    n2=[7 3];
    d2=[5 2 1];
    g2tf=tf(n2, d2);
    
    %求串联传递函数
    [gn, gd]=series(n1, d1, n2, d2)
    gnd=tf(gn, gd)


**6. 传递函数的并联**
$$!G_1(s)=\frac{2}{(s+3)(2s+1)}$$
$$!G_2(s)=\frac{7s+3}{5s^2+2s+1}$$
这里需要介绍parallel函数的应用.

    
    %使用方法主要有这三种, 详细用法清查询帮助文档, 例子同上, 这里不再详述..
    G=G1+G2;
    G=parallel(G1, G2)
    [num, den]=parallel(num1, den1, num2, den2)