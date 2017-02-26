title: VS2010和Matlab混合编程 ---调试Mex文件
date: 2015-07-14 21:35:32
categories: Matlab #文章文类
tags: [Matlab]
---

首先说明我的软件配置：
**Computer**: Windows7 SP1 64位
**VS2010**:旗舰版 SP1 
**Matlab**:R2012a ，64位
下面我们将具体讲述调试Mex文件的全过程。（<font color="#FF0000">有图有真相哟！</font> ）
<!--more-->

> **注意1**：我们下面的做法是在64位基础上进行的，对于32位同适用，只要你注意减少某些操作即可。我们会在合适的地方进行相应的说明的，所以请放心操作。
>**注意2**：由于这里建立的Mex文件里包含了opencv 2.4.9，所以如下的配置也会涉及到opencv的部分设置，如include,lib路径和附加依赖项，但是如果你也想使用opencv的话，你还需设置环境变量，在Path中添加bin路径。

### MEX的说明
写MEX程序其实就是写一个DLL程序，所以你可以使用C，C++，Fortran等多种编程语言来写。

编写MEX程序的编辑器可以使用MATLAB的代码编辑器，也可使用自己的C++编辑器，如VS2008等。

用MATLAB的编辑器的好处是，MEX函数会加粗高亮显示，这给程序编写带来便利，<font color="#FF0000">可惜无法动态调试</font>。如用VC，<font color="#FF0000">即可编译也可调试</font>，比较方便。mex的编译结果实际上就是一个带输出函数mexFunction 的dll文件，所以会用VC编写和调试dll，就会用VC编写和调试MEX程序。

### 新建一个win32 dll空项目
打开vs2010, 文件，新建项目，选择Visual C++，点击Win32,选择Win32控制台应用程序，填写程序名字和相应位置，如我这里填：MexTest,点击下一步，直到出现如下图：

---
![][1]

按照图示操作，点击完成，则新建了一个空的win32 dll项目.

### 配置环境
#### 32位平台上操作步骤：
点击 【项目】,选择【属性】，在【配置属性】选择【VC++目录】, 在【包含目录】，加入matlab下安装目录下\extern\include和opencv的include路径:
C:\opencv\build\include
C:\opencv\build\include\opencv
C:\opencv\build\include\opencv2 这三个目录。(**注意**:修改你自己的opencv路径)
【库目录】加入\extern\lib\win32\microsoft和opencv的lib路径:
C:\opencv\build\x86\vc10\lib
【链接器】->【输入】->【附加依赖项】输入：

 

    libmx.lib 
    libeng.lib
    libmat.lib 
    libmex.lib
      
    opencv_ml249d.lib
    opencv_calib3d249d.lib
    opencv_contrib249d.lib
    opencv_core249d.lib
    opencv_features2d249d.lib
    opencv_flann249d.lib
    opencv_gpu249d.lib
    opencv_highgui249d.lib
    opencv_imgproc249d.lib
    opencv_legacy249d.lib
    opencv_objdetect249d.lib
    opencv_ts249d.lib
    opencv_video249d.lib
    opencv_nonfree249d.lib
    opencv_ocl249d.lib
    opencv_photo249d.lib
    opencv_stitching249d.lib
    opencv_superres249d.lib
    opencv_videostab249d.lib


最后【链接器】->【常规】->【输出文件】里改成
`$(OutDir)$(TargetName).mexw32`

#### 64位平台上操作步骤：

点击 【项目】,选择【属性】，【链接器】->【高级】->【目标计算机】设置成**MachineX64 (/MACHINE:X64)**，然后在【生成】->【配置管理器】->【活动解决方案平台(P):】,点击，选择【新建】，然后在打开的对话框中，
按照如下操作：
![][2]
点击确定。
然后继续打开项目属性，这时发现平台已改成64位，这时我们同样按照如上32平台的方法设置相应的环境。具体如下：

 1. 【链接器】->【常规】->【输出文件】里改成
 `$(OutDir)$(TargetName).mexw64`
 2. 输入include,lib路径，添加附加依赖项，如下图：
 ![][3]
![][4] 
![][5]
### 添加源文件
在源文件中新建mex文件,(<font color="#FF0000">这里的文名必须与工程名相同</font>)
如我的MexTest.cpp文件如下：

```c++
// Interface: convert an image to gray and return to Matlab
// Author : zouxy
// Date   : 2014-03-05
// HomePage : http://blog.csdn.net/zouxy09
// Email  : zouxy09@qq.com

#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include "mex.h"
#include <string>
#include <iostream>
using namespace std;
using namespace cv;

/*******************************************************
Usage: [imageMatrix] = RGB2Gray('imageFile.jpeg');
Input: 
    a image file
OutPut: 
    a matrix of image which can be read by Matlab

**********************************************************/


void exit_with_help()
{
    mexPrintf(
    "Usage: [imageMatrix] = DenseTrack('imageFile.jpg');\n"
    );
}

static void fake_answer(mxArray *plhs[])
{
    plhs[0] = mxCreateDoubleMatrix(0, 0, mxREAL);
}

void RGB2Gray(char *filename, mxArray *plhs[])
{
    // read the image
    Mat image = imread(filename);
    if(image.empty()) {
        mexPrintf("can't open input file %s\n", filename);
        fake_answer(plhs);
        return;
    }
    
    // convert it to gray format
    Mat gray;
    if (image.channels() == 3)
        cvtColor(image, gray, CV_RGB2GRAY);
    else
        image.copyTo(gray);
    
    // convert the result to Matlab-supported format for returning
    int rows = gray.rows;
    int cols = gray.cols;
    plhs[0] = mxCreateDoubleMatrix(rows, cols, mxREAL);
    double *imgMat;
    imgMat = mxGetPr(plhs[0]);
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            *(imgMat + i + j * rows) = (double)gray.at<uchar>(i, j);
    
    return;
}

void mexFunction(int nlhs, mxArray *plhs[], int nrhs, const mxArray *prhs[])
{
    if(nrhs == 1)
    {
        char filename[256];
        mxGetString(prhs[0], filename, mxGetN(prhs[0]) + 1);
        if(filename == NULL)
        {
            mexPrintf("Error: filename is NULL\n");
            exit_with_help();
            return;
        }

        RGB2Gray(filename, plhs);
    }
    else
    {
        exit_with_help();
        fake_answer(plhs);
        return;
    }
}
```
添加def文件
代码：
```def
LIBRARY
EXPORTS mexFunction
```
按F7或者选择【生成】->【生成解决方案】,生成成功，我们就可以在
`E:\opencv\c++\MexTest\x64\Debug`找到所需要的MexTest.mexw64了。

### MATLAB设置
(1) mex 命令设置 
运行 Matlab ，在 Matlab 的命令窗口 (Command Window) 键入
`mex -setup`
命令后，按回车键，安装 Matlab 编译器； 
(2) mbuild 命令设置 
运行 Matlab ，在 Matlab 的命令窗口 (Command Window) 键入
`mbuild -setup`
命令后，按回车键，安装 Matlab 编译器； 
(3) 在 Matlab 的命令窗口 (Command Window) 键入
`cd(prefdir);`
`savepath prefdir;`
启动 MATLAB add-in 工具条.
### 调试

将matlab的current folder 设置成mexw64文件所在的路径,
在vs2010的源代码MexTest.cpp设置断点,然后
vs2010-【工具】-【附加到线程】-选择MATLAB.exe,点击附加。
matlab下输入代码或者函数（即mexw64文件的文件名），即会跳转到vs的断点处。如这里我输入：
` img = MexTest('E:\opencv\c++\1.jpg'); `
就会马上跳转到vs2010的源代码中，你可以使用F10进行调试了。

**参考文献：**

 - http://blog.sina.com.cn/s/blog_a7e72e940101cti9.html
 - http://www.cnblogs.com/avril/archive/2012/09/12/2681192.html
 - http://www.cnblogs.com/lukylu/p/4042306.html
 - http://www.cnblogs.com/avril/archive/2012/09/12/2681192.html
 - http://www.blogbus.com/shijuanfeng-logs/106781870.html
 - http://blog.csdn.net/zouxy09/article/details/20553007
 


  [1]: https://dn-xiamenwcy.qbox.me/mex/dll.jpg
  [2]: https://dn-xiamenwcy.qbox.me/mex/x64.jpg
  [3]: https://dn-xiamenwcy.qbox.me/mex/include.jpg
  [4]: https://dn-xiamenwcy.qbox.me/lib.jpg
  [5]: https://dn-xiamenwcy.qbox.me/mex/fujia.jpg
  [6]: http://blog.sina.com.cn/s/blog_a7e72e940101cti9.html