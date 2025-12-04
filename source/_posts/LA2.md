---
title: Determinant
date: 2022-07-28
tag: [linear algebra]
category: [Linear Algebra]
math: true
---

行列式

<!--more-->

设二阶方阵

$$
A=\left[ \begin{array}{ccc}
a & b\\
c & d 
\end{array} \right]
$$

容易证明，若 $A$ 可逆，当且仅当 $ad-bc\neq 0$。

若 $A$ 可逆，我们还能更进一步求出 $A$ 的逆矩阵：

$$
A^{-1}=\frac{1}{ad-bc}\left[ \begin{array}{rrr}
d & -b\\
-c & a 
\end{array} \right]
$$

看起来 $ad-bc$ 这个数似乎与矩阵 $A$ 有着特殊的联系。我们将其称为 $A$ 的行列式，记作 $\det A=ad-bc$。

然后利用“如果一个 $n$ 阶方阵可逆，当且仅当它的行列式非零。”这一定义，将行列式的概念推广到更大的方阵。

$A,B$ 均为三阶矩阵，$|A|=3,|B|=2,|A^{-1}+B|=2$，求 $|A+B^{-1}|$