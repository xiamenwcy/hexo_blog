title: Some improvements about SDM for face alignment (三)
date: 2015-08-15 00:17:06
tags: [机器学习]
mathjax: true
categories: [机器学习,人脸识别]#
---


论文[《Extended Supervised Descent Method for Robust Face Alignment》][1]对SDM方法做了扩展，使程序更鲁棒。
<!--more-->

论文主要在三方面做了Improments,分别是：
## Adaptive Feature Block
在初始的SDM方法中，我们使用fixed-size blocks去提取SIFT特征进而预测shape。但实际上，从直观上来看，the feature extraction block size与the value of shape increment有很大关系。当shape increment比较大时，我们应该提取较大块的SIFT特征，这样就可以获取更多有用的信息，便于处理大的shape变差并且保持鲁棒性。而在shape increment比较小时，我们只需提取较小块的SIFT特征去减少噪声的比例，确保了准确性。如下图：
![][2]
如下图，在我们初始迭代的时候，init shape与true shape的差值比较大，因此我们需要提取较大块的SIFT特征，而随着迭代的进行，差值越来越小，因此我们需要提取较小块的SIFT特征。

##Adaptive Regression
训练阶段，所有点的特征向量组成了$\Phi$,同样的，所有的特征点的位移组成了$\Delta X$,$R$和$b$可以通过线性回归获得。$\Phi$和$\Delta X$是所有特征点的全体，我们称这种回归为**global regression**.global regression保证了特征点之间的shape constraint，确保了鲁棒性，避免了迭代过程中的形状分离，这是非常重要的，尤其当prediction远离target时。

尽管如此，但是global regression仍然牺牲了部分准确性去确保鲁棒性。另一种回归称为**part regression**,分别在人脸的各部分回归，**local regression**，分别在各个特征点上单独回归。

初始阶段，由于estimated shape不可信，如果让各个区域分别回归，对于某些特征不明显的区域，如鼻子，它将回归到一些不好的地方，所以可以使用shape constraints强迫这部门区域回归到比较正确的地方，因此处于鲁棒性的原因，我们使用global regression.经过几次global regression,各区域基本回归到比较准确的位置，除了某些部分。这时我们使用part regression,可以将这部分不太好位置回归到比较准确的位置。最后使用local regression,作进一步的细化，提高准确性。
![][3]
如图。(a)图使用global regression,各部分已经回归得比较靠近准确的位置，唯独嘴巴偏得比较大。这时我们使用part regression，可以看到(b)，嘴巴已经回归得比较好了，其他各部基本不变。最后使用local regression，使整个特征点回归到更准确的位置。

## Ragid Regularization
这部分主要是对回归方程的一个常用的改进，使用Regularization处理。
这里 $\lambda $控制着正规化的长度，其值可以调整。比如一个单独点的HOG特征维数是128，对于由68个特征点组成的face shape，$\lambda $可达10K+。没有正规化，我们会发现线性模型倾向于**过拟合**。更一步的讲，正规化会引起更多的迭代次数，降低收敛的速度，如此的话我们前两步修正变得有可能。Regularization strength应该被不断调整以符合我们的由粗到精的准则，确切地说**，在初始阶段，应该用较大的长度以确保得到较鲁棒的模型，后期使用较小的长度更好地拟合true shape.**

总而言之，我们获得了更灵活鲁棒的模型：
![][4]


  [1]: https://dn-xiamenwcy.qbox.me/sdm/Extended%20Supervised%20Descent%20Method%20for%20Robust%20Face%20Alignment.pdf
  [2]: https://dn-xiamenwcy.qbox.me/sdm/robust_sdm1.jpg
  [3]: https://dn-xiamenwcy.qbox.me/sdm/robust_sdm2.jpg
  [4]: https://dn-xiamenwcy.qbox.me/sdm/robust_sdm3.jpg