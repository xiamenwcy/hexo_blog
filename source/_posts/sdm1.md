title: Some improvements about SDM for face alignment (一)
date: 2015-08-14 23:46:24
tags: [机器学习]
mathjax: true
toc: false
categories: [机器学习,人脸识别]#
---

本篇我们阐述对[Github][1]上给出的SDM程序,我们做的一些Bug修正。关于SDM for face alignment,请参考：
《  [Supervised Descent Method and its Applications to Face Alignment][2]》
<!--more-->

我们的程序在开始阶段需要载入数据，由于数据层次不齐，所以需要做形状归一化。其中的一个必要操作就是裁剪图片，取出包含人脸的那部分区域。如下图：
![][3]，
实际上我们不要这么大，我们只需要人脸的那部分，于是我们根据shape的包围盒并且向左上和右下拓展，扩大截取区域，得到：![][4]
但是在截取过程中，我们发现对于一些人脸过于靠近边界的图片，我们的截取区域超过了图片的范围，如图：
![][5]
在这种情况下，**一种办法**就是直接把超过图片范围的区域去掉，只取剩下的这部分。这种办法简单，很多时候也可以使用，但是在我们的程序中，这种方法可能会导致在后来的迭代过程中计算出的shape超过图片范围，造成无法继续迭代的后果。所以在这种状况下，我们希望**初始截取的图片人脸最好可以居于中间**。我们想到了**直接去填充超过图片的那部分区域即可**。如下：
![][6]
这部分中可以使用Matlab的填充图像函数：
```matlab
B = padarray(A,padsize,padval,direction)
   A为输入图像，B为填充后的图像，padsize给出了给出了填充的行数和列数，通常用[r c]来表示。padval和direction分别表示填充方法和方向。它们的具体值和描述如下：
padval：
'symmetric'表示图像大小通过围绕边界进行镜像反射来扩展；
'replicate'表示图像大小通过复制外边界中的值来扩展；  
'circular'图像大小通过将图像看成是一个二维周期函数的一个周期来进行扩展。

 direction：
'pre'表示在每一维的第一个元素前填充；
'post'表示在每一维的最后一个元素后填充；
'both'表示在每一维的第一个元素前和最后一个元素后填充，此项为默认值。
    若参量中不包括direction，则默认值为'both'。若参量中不包含padval，则默认用零来填充。若参量中不包括任何参数，则默认填充为零且方向为'both'。在计算结束时，图像会被修剪成原始大小。
举例：
        A = [1 2; 3 4];
        B = padarray(A,[3 2],'replicate','post')
```
这部分程序在common/cropImage/cropImage.m中，用法如下：
```matlab
[ img,shape,box,t ] = cropImage( img,shape )
%Input:原始的image和shape
Output:经过裁剪后的image和变换后的shape，以及包围盒、平移向量
```
最终，我们来看看效果：
<center><img src="https://dn-xiamenwcy.qbox.me/sdm/img2.jpg"></center>
<center><img src="https://dn-xiamenwcy.qbox.me/sdm/img2_crop.jpg"></center>

<center><img src="https://dn-xiamenwcy.qbox.me/sdm/image_0113.png"></center>
<center><img src="https://dn-xiamenwcy.qbox.me/sdm/image_0113.jpg"></center>

  [1]: https://github.com/tntrung/impSDM
  [2]: http://blog.csdn.net/xiamentingtao/article/details/47306887
  [3]: https://dn-xiamenwcy.qbox.me/sdm/img3_rectangle2.jpg
  [4]: https://dn-xiamenwcy.qbox.me/sdm/img3_crop.jpg
  [5]: https://dn-xiamenwcy.qbox.me/sdm/img_rectangle3.jpg
  [6]: https://dn-xiamenwcy.qbox.me/sdm/img1_crop.jpg
  
  