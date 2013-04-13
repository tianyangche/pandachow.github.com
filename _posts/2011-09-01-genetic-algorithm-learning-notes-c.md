---
layout: post
title: 遗传算法学习笔记(三)
categories:
- 伪技术
tags:
- MATLAB
- 人工智能
- 数学建模
- 算法
- 遗传
---

本来不准备写这个的, 突然发现 GA 在很多题目中都有应用, 稍微看了一下, 几乎每年的 CUMCM 都会遇到多约束的非线性规划问题. 对这样的非线性规划的问题的建模不是很容易, 当然对这样的模型进行求解同样不容易. 简单总结一下, 对于这种问题, 目前的解决办法也是很多的, 类似 MATLAB 的优化工具箱, Lingo/Lindo 软件, 在数据量不是很大的情况下甚至可以采用穷举法.
这几种方法里面 MATLAB 优化工具箱求解精度通常比较低, 而穷举法效率又实在是太低了. 因此通常 GA 和 Lindo/Lingo 是首选的方法.

下面举例说明使用 GA 工具箱来求解多约束非线性规划问题.
$$!min f(x)=e^{x_1}(4x_1^2+2x_2^2+4x_1x_2+2x_2+1)$$
$$!s.t. \begin{cases}1.5+x_1x_2-x_1-x_2\le 0 \\ -x_1x_2\le 0\end{cases}$$

主程序ga02.m:

    
    %主程序: 本程序采用遗传算法接力进化
    %将上次进化结束后的得到的最终种群群体作为本次输入的初始种群
    clc;
    clear all;
    close all;
    %进化代数
    optionsOrigin=gaoptimset('Generations',50,);
    [x, fval, reason, output, finnal_pop]=ga(@ch14_2f, 2, optionsOrigin);
    %进行第二次接力进化
    options1=gaoptimset('Generations',50,'InitialPopulation',finnal_pop,@gaplotbestf);
    [x,fval,reason,output,finnal_pop]=ga(@ch14_2f,2,options1);
    Bestx=x;
    BestFval=fval;


子函数, 写适应度函数, 也就是目标函数.

    
    %子函数: 适应度函数同时也是目标函数
    function f=ch14_2f(x)
    g1=1.5+x(1)*x(2)-x(1)-x(2);
    g2=-x(1)*x(2);
    if(g1>0|g2>10)
        f=100;
    else
        f=exp(x(1))*(4*x(1)^2+2*x(2)^2+4*x(1)*x(2)+2*x(2)+1)%目标函数
    end


试验结果:
	>> Bestx, BestFval
	Bestx =
	-8.9975 1.1097
	BestFval =
	0.0358
[![](http://panda0411.com/wordpress/wp-content/uploads/2011/09/GA03.jpg)](http://panda0411.com/wordpress/wp-content/uploads/2011/09/GA03.jpg)

这里需要提醒的一点是, 对于约束条件比较复杂的情况, 可能会导致可行解不太容易被找到, 直接用普通的交叉, 变异操作可能会很容易导致产生的大量解不可行, 或者说产生的新的染色体并不是对应着可行解.
这个时候如果仍然继续使用GA, 只有对交叉变异操作进行一个特殊的设计, 以便新的染色体能尽量保持在可行解的区域内.

GA就这么多, 明天争取撸一遍蚁群, 碎叫拉~ O_O
