---
layout: post
title: 神经网络学习笔记(一)
categories:
- research
tags:
- MATLAB
- Artificial Intelligence
- MATLAB Toolbox
- Mathematical Modeling
- Neuronic Network
- Algorithm
---

话说其实做 RoboCup 很久了, ANN 也甚至早在深度接触到 MCM 之前就有所了解了, RoboCup 也是充斥着各种 ANN 的 paper, 自然读过不少, 但就现在来说 ANN 还多是用在 RoboCup 的仿真层面上, 实体部分貌似现在还没怎么实现把, 我猜是因为硬件条件所限制把, 当然我没有碰过实体(有点遗憾..), 也是没什么发言权的.

关于 ANN 的理论部分这里就不再叙述了, 也就是一大堆的生物背景, 复杂的数学模型也不再推导了, 各种教科书上都是有很详细的. 下面着重讲讲 BP 神经网络的 MATLAB 工具箱.

## BP神经网络 MATLAB 工具箱
MathWorks 公司为了让用户使用方便, 特意开发了神经网络工具箱. 工具箱的使用的确非常便捷, 节省了用户编程的时间, 我就简单介绍一下这个工具箱的相关命令和函数. 当然, 随着MATLAB编程能力的提高和对ANN 的理解的进一步加深, 也是可以像前面学习遗传算法一样, 自行来编写MATLAB程序的.
	
  1. 前向网络创建函数: newcf, newff, newfftd
  2. 激励函数: logsig, dlogsig, tansig, dtansig, purelin, dpurelin
  3. 学习函数: learngd, learngdm
  4. 性能函数: mse, msereg

### BP网络创建函数
在介绍这类函数之前, 先给出在BP网络创建函数中可能用得到的变量及其含义:

  * PR: 由每组输入元素的最大值和最小值组成的Rx2的矩阵;
  * $$S_i$$: 第i层的长度, 共计N层;
  * $$TF_i$$: 第$$i$$层的激励函数, 默认为"tansig";
  * BTF: 网络的训练函数, 默认为"trainlm";
  * BLF: 权值和阈值的学习算法, 默认为"learngdm";
  * PF: 网络的性能函数, 默认为"mse"

#### 函数newcf
这个函数用于创建级联前向BP网络, 调用格式为:
    
    net=newcf
    net=newcf(PR, [S1, S2...SN], {TF1 TF2 ... TFN}, BTF, BLF, PF)

其中, net=newcf用于在对话框中创建一个BP网络..

#### 函数newff
这个函数用于创建一个BP网络, 其调用格式为:
    
    net=newff
    net=newff(PR, [S1, S2...SN], {TF1 TF2 ... TFN}, BTF, BLF, PF)

其中net=newff用于在对话框中创建一个BP网络.

#### 函数newfftd
这个函数用于创建一个存在输入延迟的前向网络, 其调用格式为:
    
    net=newfftd
    net=newfftd(PR, [S1, S2...SN], {TF1 TF2 ... TFN}, BTF, BLF, PF)

其中, net=newfftd用于在对话框中创建一个BP网络.

### 神经元激励函数
激励函数是BP神经网络的重要组成部分, 必须是连续可微的. BP网络经常采用S型的对数或者正切函数和线性函数.

#### 函数logsig
激励函数logsig为S型的对数函数, 语法格式:
    
    A=logsig(N)
    info=logsig(code)

其中, N为Q个S维的输入列向量; A为函数返回值, 位于区间(0,1)中; info依据code值的不同而返回不同的信息, 具体请help.

#### 函数dlogsig
函数dlogsig为logsig的导函数, 使用格式为:
    
    dA_dN=dlogsig(N,A)

其中, N为SxQ维网络输入; A为SxQ维网络输出; dA_dN为函数的返回值, 输出对输入的导数. 该函数使用的算法为$$d=a(1-a)$$.
示例: 假设一个BP网络某层有3个神经元, 其激励函数为S型的对数函数, 利用函数dlogsig进行计算, 其MATLAB代码为:
    
    N=[1, 6, 2]'
    A=logsig(N)
    da_dn=dlogsig(N,A)

#### 函数tansig
函数tansig为双曲正切S型激励函数, 其使用格式为:
A=tansig(N)
info=tansig(code)
其中, N为Q个S维的输入列向量; A为函数返回值, 位于区间(-1, 1)之间; info依据code不同返回不同的值, 具体请参考help文件. 该函数使用的算法为:
$$!n=\frac{2}{[1+e^{-2n}]-1}$$

#### 函数dtansig
函数dtansig是tansig的导函数, 调用格式为:
    
    dA_dN=dtansig(N,A)

其中, 参数的含义与dlogsig相同, 该函数使用的算法为$$d=1-a^2$$

#### 函数purelin
函数purelin为线性激励函数, 其使用格式为:
A=purelin(N)
info=purelin(code)
其中, N为Q个S维的输入列向量; A为函数返回值, A=N; info仍然是依据code值来返回不同的信息. 该函数使用的算法是$$purelin(n)=n$$

#### 函数dpurelin
函数dpurelin是purelin的导函数, 其调用格式为:
dA_dN=dpurelin(N,A)
其中, 参数的含义与函数dlogsig的相同

### BP网络学习函数
**函数learngd:** 这是梯度梯度下降权值/阈值学习函数, 它通过神经元的输入和误差, 以及权值和阈值的学习速率计算权值或阈值的变化率.
**函数learngdm:** 这是梯度下降动量学习函数, 它利用神经元的输入和误差, 权值或者阈值的学习速率和动量常数计算权值或者阈值的变化率. 具体函数的使用请参考help文件.

### BP网络训练函数
**函数trainbfg:** 这个函数为BFGS准牛顿BP算法函数. 除了BP网络之外, 该函数也可以训练任意形式的神经网络, 要求它的激励函数对于权值和输入存在导数.
**函数traingd:** 函数traingd是梯度下降BP算法训练函数.
**函数traingdm:** 为梯度下降动量BP算法训练函数.
类似其它的训练函数还有很多, 就不一一例举了, 请大家自行help之.

### 性能函数
函数mse和msereg, mse是均方误差性能函数, msereg是在函数mse基础上增加了一项网络权值和阈值的均方值,目的是使网络获得较小的权值和阈值, 从而迫使网络的响应变得更平滑.
具体细节请自行help之, 这几个函数官方的help文件都给的非常详细和浅显, 而且都给出了小例子, 赞一下MathWorks~
