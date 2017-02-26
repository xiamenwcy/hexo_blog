title: 图像极坐标变换及在OCR中的应用
date: 2017-02-16 15:43:56
tags: [图像处理]
mathjax: true
categories: [图像处理]#
---



# 极坐标变换定义

我们知道在二维坐标系中，有直角坐标系，也有极坐标系，二者的转换关系是：
<center>
![][1]
</center>

如下图：
<center>
![][2]
</center>

如图，直角坐标系的圆心与极坐标系的圆心一一对应，且圆弧BA可以通过极坐标变换到极坐标系$\rho=r$的一条直线上，实现由圆形到直线的转换。这往往在一些图像处理中很有用。
<!--more-->

实际上，我们在图像处理中，往往还不是处理这样的圆弧，而更多的是处理圆环区域。如下，
<center>
![][3]
</center>

同理，我们可以把（a）图中的圆环区域1234,转换成矩形区域（b）.矩形区域与圆环存在一定的对应关系，区域转换满足：转换前后两区域顶点1234一一对应，转换后的矩形区域宽为圆环内外弧长之差$(\phi_2-\phi_1)\cdot R_2$，高为圆环内外半径的差$R_2-R_1$.

具体的数学转换关系是：获取圆环区域的圆心坐标{% math (x_0,y_0)%}、外半径{% math R_2%}、内半径{% math R_1%}、圆环起始角度{% math \phi_1%}和终止角度{% math \phi_2%}.
取矩形区域一点{% math A(x,y)%},它在圆弧内对应的点为{% math A'%}.在图（b）矩形区域中，每一个单位长度对应的角度为{% math (\phi_2-\phi_1)/[(\phi_2-\phi_1)\cdot R_2]%},记{% math (\phi_2-\phi_1)%}为{% math \Delta_{\phi}%},则A对应于A'在圆环区域内极坐标下的角度{% math \phi_A %}表示为：
{% math_block %}
\phi_A=\phi_1+\frac{\Delta_{\phi}}{\Delta_{\phi}\cdot R_2}x~~~~~~~（1）
{% endmath_block %}
点A对应于A'在圆环区域内极坐标下的半径{% math R_A %}为：
{% math_block %}
R_A=R_1+y.~~~~~~~(2）
{% endmath_block %}
得到{% math \phi_A,R_A %}后，可以通过极坐标变换得到直角坐标系下的坐标。
设A'在圆环区域内{% math (x',y') %},则
{% math_block %}
\left\{\begin{matrix}
x' & =x_0+R_A\cos\phi_A\\ 
y' & =y_0+R_A\sin\phi_A
\end{matrix}\right. ~~~~~~~~~~~~~~~~~(3)
{% endmath_block %}
显然A点的灰度值应该与A'的灰度值相同，但是A'的坐标值通常不是整数，因此无法计算A'的像素值，可以通过[双线性插值][4]获得其近似像素值。





# 极坐标变换在OCR中的应用

在工业视觉领域，经常要进行字符识别，但是有些字符是印在像硬币、CD唱片机一样的圆形区域上，如下图：
<center>
![][5]
 图2
</center>

我们想要识别硬币上的字符，这时需要使用OCR。OCR通常需要进行两步，第一是**字符分割**，第二是**字符识别**。对于此图而言，字符分割不容易，主要是由于我们需要识别的字符位于环形区域中，并不是一般意义上的水平排布，此时我们就可以使用如上的极坐标变换，先定位字符的圆环区域，再转变到水平的矩形区域。这时再采取阈值分割、blob分析等手段分割字符，进而就可以进行OCR了。

极坐标变换后的图是：
<center>
![][6]
 图3
</center>

# 其他示例

<center>
![][7]
</center>




# 参考文献
1. [基于HALCON的圆环区域字符识别实现][8]
2. [基于视觉技术的圆环外观缺陷检测算法研究][9]
3. [基于Halcon的指针式仪表的读数][10]
4. [图片极坐标效果揭秘][11]


  [1]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/2.JPG
  [2]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/1.JPG
  [3]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/3.JPG
  [4]: http://blog.csdn.net/andrew659/article/details/4818988
  [5]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/4.JPG
  [6]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/5.JPG
  [7]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/6.JPG
  [8]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/%E5%9F%BA%E4%BA%8EHALCON%E7%9A%84%E5%9C%86%E7%8E%AF%E5%8C%BA%E5%9F%9F%E5%AD%97%E7%AC%A6%E8%AF%86%E5%88%AB%E5%AE%9E%E7%8E%B0_%E9%BB%84%E5%89%91%E8%88%AA.caj
  [9]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/%E5%9F%BA%E4%BA%8E%E8%A7%86%E8%A7%89%E6%8A%80%E6%9C%AF%E7%9A%84%E5%9C%86%E7%8E%AF%E5%A4%96%E8%A7%82%E7%BC%BA%E9%99%B7%E6%A3%80%E6%B5%8B%E7%AE%97%E6%B3%95%E7%A0%94%E7%A9%B6_%E9%99%88%E8%87%B3%E5%9D%A4.caj
  [10]: http://oana7cw0r.bkt.clouddn.com/%E6%9E%81%E5%9D%90%E6%A0%87%E5%8F%98%E6%8D%A2/%E5%9F%BA%E4%BA%8EHalcon%E7%9A%84%E6%8C%87%E9%92%88%E5%BC%8F%E4%BB%AA%E8%A1%A8%E7%9A%84%E8%AF%BB%E6%95%B0_%E5%B0%B9%E7%BA%A2%E6%95%8F.caj
  [11]: http://www.zzwave.com/blog-2-30075.html