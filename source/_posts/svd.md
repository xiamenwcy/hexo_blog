title: 奇异值分解（SVD）介绍
date: 2015-07-26 19:59:58
tags: [机器学习，数学算法]
mathjax: true
categories: 机器学习 #
---
本文是笔者在阅读众多资料，包括网上资料、教科书的基础上，编写而成。
其基本写作框架是：
1.从数学的角度，对奇异值分解做更加准确的描述，包括定义和性质；
2.matlab的奇异值分解函数简介；
<!--more-->

## 数学上的SVD

我们阐述关于SVD的定义。
【**定义**】令$A\in R^{m\times n}$,则存在正交矩阵 $U\in R^{m\times m}$, $V\in R^{n\times n}$使得：
$$ A=U\Sigma V$$，其中$$\Sigma =
diag(\Sigma_1,O)
\in R^{m\times n}$$且 $\Sigma_1=diag(\sigma_1,\sigma_2,...,\sigma_r)$
其对角元素按照顺序$$\sigma_1\geqslant\sigma_2\geqslant...\geqslant \sigma_r>0,r=rank(A)  $$排列。
【**性质**】
  

 一、 奇异值和特征值的关系：
   我们将从下面几个定理阐述：
 1. 假设$A\in R^{n\times n}$对称，具有特征值$\lambda_i$和标准正交特征向量$u_i$。换言之
 $A=U\Lambda U^T$是A的特征分解，其中
$$\Lambda =diag(\lambda_1,\lambda_2,...,\lambda_n),U=[u_1,u_2,...,u_n]$$和
$$ UU^T=I $$.则A的的一个SVD是$A=U\Sigma V^T$,其中$\sigma_i=|\lambda_i|,v_i=sign(\lambda_i)u_i,sign(0)=1$


2.对称矩阵$A^TA$特征值是$\sigma_i^2$，右奇异向量$v_i$是对应的标准正交向量。


3.对矩阵$AA^T$的特征值是$\sigma_i^2$和m-n个零,左奇异向量$u_i$是特征值$\sigma_i^2$对应的标准正交向量, 对特征值0可取任意m-n个正交向量作为特征向量。

从以上定理来看，<font color="#FF0000">对称矩阵的特征值和奇异值相差一个符号</font> ，而$A^TA$和$AA^T$的非零特征值就是A的奇异值的平方，换言之，A的奇异值是$A^TA$和$AA^T$的非零特征值的平方根。
二、奇异值分解的改写。
1.由于V为正交矩阵，故$A^{-1}=A^T$,因此：
 ![][4]

四、奇异值分解的变形
1. $m\times n$矩阵A的共轭转置$A^H$的奇异值分解为：
 $$ A^H=V\Sigma^TU^H$$
2. $A^HA,AA^H$的奇异值分解分别为:
$$A^HA=V\Sigma^T  \Sigma  V^H$$
$$AA^H=U\Sigma\Sigma^T U^H$$
其中：
$$ \Sigma^T\Sigma=diag(\sigma_1^2,\sigma_2^2,...,\sigma_r^2,\overbrace{0,0,...,0}^{n-r个})$$和

$$\Sigma\Sigma^T=diag(\sigma_1^2,\sigma_2^2,...,\sigma_r^2,\overbrace{0,0,...,0}^{m-r个})$$
## matlab的svd函数
matlab中有两个关于svd的函数，分别是：
```matlab
  [U,S,V] = svd(X)
   S = svds(A)
```
svd用法如下：

    [U,S,V] = svd(X) 将产生与X维数相同的对角矩阵并且对角元非负递减,U,V为正交矩阵,使得
    X = U*S*V'.

    S = svd(X) 将返回包含奇异值的向量。
 
    [U,S,V] = svd(X,0) 将产生"economy size"的分解。
    如果 X是 m*n，并且m > n, 将产生U的前n列，S是n*n.
    对于m<=n,,svd(X,0)等价于 svd(X).
   
    [U,S,V] = svd(X,'econ') a也产生 "economy size"
    分解. 如果 X是 m*n，并且m > n, 等价于svd(X,0). 
    对于m < n,  将产生V的前m列，S是m*m. 
svds用法如下：

    S = svds(A) 返回 A的6个最大奇异值.
    [U,S,V] = svds(X)返回A的奇异值和奇异向量。
    S = svds(A,K) 返回 A的K个最大奇异值.
    S = svds(A,K,SIGMA)返回K个与SIGMA最接近的奇异值. 比如 S = svds(A,K,0) 将计算K个最小的奇异值。
    [u,d,v]=svds(A,10，2)将得到与2最接近的10个特征值及其对应的特征行向量和特征列向量。

上面两个函数中svd一般就可以满足需求，对于特别大规模的矩阵分解或者需要求一部分奇异值，可以使用svds。

参考文献：

 1. [矩阵特征值分解与奇异值分解含义解析及应用][1]
 2. [奇异值分解][2]
 3. [求matlab 奇异值分解函数 svd和svds的区别？][3]
  [1]: http://blog.csdn.net/xiahouzuoxin/article/details/41118351
  [2]: http://blog.csdn.net/ningyaliuhebei/article/details/7104951
  [3]: http://wenda.chinabaike.com/b/37396/2013/1208/714610.html
  [4]:https://dn-xiamenwcy.qbox.me/svd/svd1.jpg