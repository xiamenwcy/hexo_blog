title: 编程相关资料
---

## 数学系列
### 数值分析
1. 山东理工大学数值分析精品课程：http://sxyd.sdut.edu.cn/shuzhifenxi/
2. 北京大学数值分析主页：http://www.math.pku.edu.cn/teachers/tanghz/private/homepage/numericalmethods.htm
3. Numerical Recipes(数值分析方法库)主页：http://numerical.recipes/ 
4. Numerical Recipes(数值分析方法库) C/C++下载：http://bbs.sciencenet.cn/thread-1394978-1-1.html ,
http://bbs.sciencenet.cn/thread-1130371-1-1.html
源代码：http://z.download.csdn.net/detail/brightliadan/760959
或者百度网盘：http://pan.baidu.com/s/1jIPRk6U 密码：bxvd

## C++系列
### C++库
<font color="#8E388E">**线性代数**：</font> 

 1.**Armadillo**：http://arma.sourceforge.net/
<font color="#FF0000">**简介**：</font> 
Armadillo C++ Library是一种C++的线性代数库（矩阵数学）以取得良好的平衡速度与易用性。整数，浮点，而复杂的数字支持，以及一个子集，三角和统计功能。各种矩阵分解是通过可选的集成 与LAPACK和Atlas 库。延迟评价方法，基于模板元编程，使用（在编译时）结合几个行动之一，并减少或消除需要临时量。
<font color="#FF0000">**相关链接**：</font> 

- csdn教程：http://blog.csdn.net/jnulzl/article/category/5634263
- [C++矩阵运算库armadillo配置笔记][1]

<font color="#FF0000">**推荐指数：**</font>
$\bigstar\bigstar\bigstar\bigstar\bigstar $

2.**Eigen**：http://eigen.tuxfamily.org/index.php?title=Main_Page 
<font color="#FF0000">**简介**：</font> 
Eigen是一个易用的C++线性代数库，类似于matlab的使用。Eigen库直接提供了源代码。用的时候只需要包含源代码头文件即可。
<font color="#FF0000">**相关链接**：</font> 

- Eigen安装：http://blog.csdn.net/abcjennifer/article/details/7781936
- [Eigen+suitesparse for windows 安装][2]

<font color="#FF0000">**推荐指数：**</font>
$\bigstar\bigstar\bigstar\bigstar\bigstar $

<font color="#8E388E">**图像处理**：</font> 

1.**CImg Library**：http://cimg.eu/
<font color="#FF0000">**简介**：</font> 
CImg是一个跨平台的C++的图像处理库，提供了加载、处理、显示、保存等一系列功能，其中的图像处理功能尤其强大。
<font color="#FF0000">**相关链接**：</font> 

 - [CImg库介绍][3]

<font color="#FF0000">**推荐指数：**</font>
$\bigstar\bigstar\bigstar\bigstar\bigstar $
2.**C++ Bitmap Library **：http://partow.net/programming/bitmap/index.html
<font color="#FF0000">**简介**：</font> 
The C++ Bitmap Library consists of simple, robust, optimized and portable processing routines for the 24-bit per pixel bitmap image format.

**亮点：** 仅仅只需包含一个头文件`#include "bitmap_image.hpp"`即可使用全部的功能，非常轻量级，但是只可以处理24位的bmp彩色图像。

**注意事项：** 
由于源文件是一个hpp文件，所以当我们<font color="#FF0000"> 多次包含该头文件时会出现重复定义</font>的错误。主要原因是**其内部包含了全局函数和全局变量**。参考：http://baike.baidu.com/item/HPP/4448301, 有如下描述：<font color="#8E388E">
由于hpp本质上是作为.h被调用者include，所以当hpp文件中存在全局对象或者全局函数，而该hpp被多个调用者include时，将在链接时导致符号重定义错误。要避免这种情况，需要去除全局对象，将全局函数封装为类的静态方法。</font>

为了消除错误，我改造了该文件为.h和.cpp联合使用，并将全局变量和全局函数的定义和声明分离。 我上传了改造好的文件，请[点击这里][4]下载，使用时与上面是一样的，只需将`#include "bitmap_image.hpp"`改为`#include "bitmap_image.h"`即可。


## Matlab系列
 1. Mupad工具箱
 
<font color="#FF0000">**简介**：</font> 
Mupad是Matlab的一个工具箱，在Matlab下通过命令mupad即可进入。
Mupad可以做一个超级计算器、化简，解微分方程，画图像！总之，一切数学相关的都可以，而且非常优雅！**强大的符号运算功能。**
<font color="#FF0000">**相关链接**：</font> 
 - [Mupad使用小结][5]



 


  [1]: http://www.cnblogs.com/wacc/p/5031373.html
  [2]: http://blog.csdn.net/xiamentingtao/article/details/50100549
  [3]: http://blog.sina.com.cn/s/blog_6aca4f3e0100tmfp.html
  [4]: http://oana7cw0r.bkt.clouddn.com/bitmap_image.zip
  [5]: http://blog.csdn.net/nghuyong/article/details/51871985