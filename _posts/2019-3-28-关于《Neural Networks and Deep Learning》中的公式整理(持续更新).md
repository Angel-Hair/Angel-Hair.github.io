---
title: 关于《Neural Networks and Deep Learning》中的公式整理(持续更新)
categories:
 - 读书笔记
tags:
 - 读书笔记
 - 神经网络
 - 深度学习
 - 《Neural Networks and Deep Learning》
 - 公式
---

# 前言

最近在看Michael Nielsen的《Neural Networks and Deep Learning》，想整理下里面的一些公式帮助记忆，顺便还可以随取随用。

注意:正文章节按照原文章节整理分布，并且我有意没有删掉公式序号，方便查找。

另外，根据网上推荐，我也在coursera上追吴恩达博士的机器学习课，感觉很好，推荐给大家。

# 公式整理

## 感知机

感知机工作原理:

$$\begin{eqnarray} \mbox{output} & = & \left\{ \begin{array}{ll} 0 & \mbox{if } \sum_j w_j x_j \leq \mbox{ threshold} \\ 1 & \mbox{if } \sum_j w_j x_j > \mbox{ threshold} \end{array} \right. \tag{1}\end{eqnarray}$$

$$\begin{eqnarray} \mbox{output} = \left\{ \begin{array}{ll} 0 & \mbox{if } w\cdot x + b \leq 0 \\ 1 & \mbox{if } w\cdot x + b > 0 \end{array} \right. \tag{2}\end{eqnarray}$$

## Sigmoid神经元

sigmoid函数(sigmoid function):

$$\begin{eqnarray} \sigma(z) \equiv \frac{1}{1+e^{-z}} \equiv \frac{1}{1+\exp(-\sum_j w_j x_j-b)}. \tag{3}\end{eqnarray}$$

关于σ函数的平滑属性的神经元输出改变量:

$$\begin{eqnarray} \Delta \mbox{output} \approx \sum_j \frac{\partial \, \mbox{output}}{\partial w_j} \Delta w_j + \frac{\partial \, \mbox{output}}{\partial b} \Delta b, \tag{5}\end{eqnarray}$$

## 通过梯度下降法学习参数

平方代价函数/均方误差/MSE:

$$\begin{eqnarray} C(w,b) \equiv \frac{1}{2n} \sum_x \| y(x) - a\|^2. \tag{6}\end{eqnarray}$$

其他形式有:

$$\begin{eqnarray} C = \frac{1}{2n} \sum_x \|y(x)-a^L(x)\|^2, \tag{26}\end{eqnarray}$$

速度变化量:

$$\Delta v \equiv (\Delta v_1, \Delta v_2)^T$$

梯度向量:

$$\begin{eqnarray} \nabla C \equiv \left( \frac{\partial C}{\partial v_1}, \frac{\partial C}{\partial v_2} \right)^T. \tag{8}\end{eqnarray}$$

代入学习率的速度变化量:

$$\begin{eqnarray} \Delta v = -\eta \nabla C, \tag{10}\end{eqnarray}$$

代价函数变化量:

$$\begin{eqnarray} \Delta C \approx \frac{\partial C}{\partial v_1} \Delta v_1 + \frac{\partial C}{\partial v_2} \Delta v_2 \approx \nabla C \cdot \Delta v = -\eta \|\nabla C\|^2. \tag{7、9}\end{eqnarray}$$

$v$移动公式:

$$\begin{eqnarray} v \rightarrow v' = v -\eta \nabla C. \tag{11}\end{eqnarray}$$

神经网络中的梯度下降:

$$\begin{eqnarray} w_k & \rightarrow & w_k' = w_k-\eta \frac{\partial C}{\partial w_k} \tag{16}\\ b_l & \rightarrow & b_l' = b_l-\eta \frac{\partial C}{\partial b_l}. \tag{17}\end{eqnarray}$$

随机梯度下降（SGD）中的平均 $\nabla C_x$ :

$$\begin{eqnarray} \frac{\sum_{j=1}^m \nabla C_{X_{j}}}{m} \approx \frac{\sum_x \nabla C_x}{n} = \nabla C \tag{18}\end{eqnarray}$$

SGD中的梯度下降:

$$\begin{eqnarray} w_k & \rightarrow & w_k' = w_k-\frac{\eta}{m} \sum_j \frac{\partial C_{X_j}}{\partial w_k} \tag{20}\\ b_l & \rightarrow & b_l' = b_l-\frac{\eta}{m} \sum_j \frac{\partial C_{X_j}}{\partial b_l}  \tag{21}\end{eqnarray}$$

## 一个基于矩阵的快速计算神经网络输出的方法

从第$l−1$层的第$k$个神经元到第$l$层的第$j$个神经元的连接的权重: $w_{jk}^l$

第$l$层第$j$个神经元的偏置: $b^l_j$

第$l$层的第$j$个神经元的激活值: $a^l_j$

激活值 $a^l_j$ 公式:

$$\begin{eqnarray} a^{l}_j = \sigma\left( \sum_k w^{l}_{jk} a^{l-1}_k + b^l_j \right) \tag{23}\end{eqnarray}$$

其矩阵形式为:

$$\begin{eqnarray} a^{l} = \sigma(w^l a^{l-1}+b^l).\tag{25}\end{eqnarray}$$

第$l$层神经元的加权输入(weighted input):

$$z^l_j= \sum_k w^l_{jk} a^{l-1}_k+b^l_j$$

其矩阵形式:

$$z^l \equiv w^l a^{l-1}+b^l$$

## 关于代价函数的两个假设

代价函数的第一假设形式:

$$C=\frac{1}{n} \sum_x C_x$$

第二假设中单一训练样本$x$的二次代价:

$$\begin{eqnarray} C_x = \frac{1}{2} \|y-a^L\|^2 = \frac{1}{2} \sum_j (y_j-a^L_j)^2 \tag{27}\end{eqnarray}$$

## Hadamard积

$$(s \odot t)_j = s_j t_j$$

## 反向传播背后的四个基本等式

$l$层的$j$神经元的错误量(error):$\delta^l_j$

错误量定义:

$$\begin{eqnarray} \delta^l_j \equiv \frac{\partial C}{\partial z^l_j} \tag{29}\end{eqnarray}$$

输出层中关于错误量$\delta^L$的等式(BP1):

$$\begin{eqnarray} \delta^L_j = \frac{\partial C}{\partial a^L_j} \sigma'(z^L_j) \tag{BP1}\end{eqnarray}$$

BP1的矩阵形式:

$$\begin{eqnarray} \delta^L = (a^L-y) \odot \sigma'(z^L). \tag{30}\end{eqnarray}$$

依据下一层错误量$\delta^{l+1}$获取错误量$\delta^l$的矩阵形式(BP2):

$$\begin{eqnarray} \delta^l = ((w^{l+1})^T \delta^{l+1}) \odot \sigma'(z^l) \tag{BP2}\end{eqnarray}$$

网络的代价函数相对于偏置的改变速率的等式(BP3):

$$\begin{eqnarray} \frac{\partial C}{\partial b^l_j} = \delta^l_j \tag{BP3}\end{eqnarray}$$

BP3的简略式:

$$\begin{eqnarray} \frac{\partial C}{\partial b} = \delta \tag{31}\end{eqnarray}$$

网络的代价函数相对于权重的改变速率的等式(BP4):

$$\begin{eqnarray} \frac{\partial C}{\partial w^l_{jk}} = a^{l-1}_k \delta^l_j \tag{BP4}\end{eqnarray}$$

BP4的简略式:

$$\begin{eqnarray} \frac{\partial C}{\partial w} = a_{\rm in} \delta_{\rm out} \tag{32}\end{eqnarray}$$

# 后记

说来也奇怪，我在追吴恩达博士的课的时候，学到吴恩达介绍Yann LeCun的一段video的那段视频，刚好是公布Yann LeCun荣获2018年图灵奖开始的时候。