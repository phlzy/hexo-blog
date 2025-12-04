---
title: CO1
date: 2023-07-18
tag: [Convex Optimization]
category: [Convex Optimization]
---

# 0 绪论

## 一些定义

### 优化问题

对于数学优化问题：
$$
\begin{array}{lll}
\min\; f_0(x)\\
s.t. \; f_{i}(x)\le b_{i}\; (i = 1,2,\cdots, m)
\end{array}
$$

其中向量 $x$ 称为**优化变量**；
函数 $f_0:\mathbf{R^n}\to \mathbf{R}$ 称为**目标函数**；
函数 $f_i:\mathbf{R^n}\to \mathbf{R}\;(i = 1,2,\cdots, m)$ 称为**约束函数**；
常数 $b_i$ 称为**约束边界**。

如果在所有满足约束的向量中向量 $x^*$ 对应的目标函数值最小，那么称 $x^*$ 为该问题的**最优解**。

### 线性规划

如果上面的问题中 $f_i$ 是线性函数，即：

$$
\forall x,y\in \mathbf{R^n} \\
\forall \alpha,\beta \in \mathbf{R}\\
s.t.\; f_{i}(\alpha x+\beta y)=\alpha f_i(x)+\beta f_i(y)
$$

则此优化问题称为**线性规划**。

### 凸优化

凸优化问题中的目标函数和约束函数都是凸函数。如果函数 $f_i$ 是凸函数，则需要满足以下条件：

$$
\forall x,y\in \mathbf{R^n} \\
\forall \alpha,\beta \in \mathbf{R}\\
\alpha + \beta = 1,\; \alpha \ge 0,\; \beta \ge 0\\
s.t.\; f_{i}(\alpha x+\beta y)\le\alpha f_i(x)+\beta f_i(y)
$$

直观感受一下可以发现，凸函数的图像是从中间凹陷下去的，即函数图像上取两个点连一条线段，线段中的点都位于函数图像的上方。

显然，线性规划问题是一种凸优化问题。

## 最小二乘问题

最小二乘问题是一种常见的凸优化问题，具体形式如下：

$$
\min\; f_0(x)=\|Ax-b\|_{2}^{2}
=\sum_{i=1}^{k}(a_{i}^{T}x-b)^2
$$

该问题没有约束条件，约束目标是若干项的平方和。

