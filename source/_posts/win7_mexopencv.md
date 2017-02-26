title: Win7 64bit下MexOpenCV的安装，Matlab和C++&OpenCV的完美结合
date: 2015-07-06 07:33:15
categories: 机器学习 #文章文类
tags: [机器学习，安装]
---
首先说明一下我的安装环境：
**操作系统**：Win7 64位 SP1
**Matlab**: 2012a 64位& 2013a 64位（两个版本均试验过）
**Visual Studio**:2010 sp1旗舰版
下面介绍具体的安装方法：
<!--more-->

##Windows SDK 7.1 的安装（Win 7 64bit，x64平台）
闲话少说，先准备素材。这里我已经为您准备好了一切。

 1. [Windows SDK 7.1 安装包][1]
 2. [vs2010 sp1][2]
 3. [VC-Compiler-KB2519277][3]

> 注意：在安装VS2010时还自动安装了 Microsoft Visual C++ 2010 x86 Redistributable - 10.0.30319 及更高版本，一定要先卸载比 Microsoft Visual C++ 2010 x86 Redistributable - 10.0.30319 更高的版本（不包括Microsoft Visual C++ 2010 x86 Redistributable - 10.0.30319 ）。否则在安装windows sdk会出现错误。


现在终于可以开始安装Windows SDK 7.1了，注意在安装时不要选择安装 VC-Compiler，其它选项默认即可。
然后，安装vs2010 sp1，
最后，安装我们事先下载好的 VC-Compiler-KB2519277 安装包，到此完成安装。

---------------------------------------分割线----------------------------------------

如果没有安装vs 2010 sp1，按照如下顺序安装

1> 安装vs2010
2> 安装 windows sdk v7.1, 安装之前确保所有vc++ x86/x64 runtime/redistributable 版本不能大于 10.0.30319. 存在则卸载
3> 安装vs2010 sp1
4> 安装VC-Compiler-KB2519277

---------------------------------------分割线----------------------------------------
对于一些步骤的说明：
1.卸载比 Microsoft Visual C++ 2010 x86 Redistributable - 10.0.30319 更高的版本的原因：
Windows SDK 7.1不支持Microsoft Visual C++ 2010 x86 Redistributable - 10.0.30319 以上版本。
2.安装Windows SDK 7.1时不直接选择安装 VC-Compiler 的原因：
在安装VS2010的SP1补丁时，VC-Compiler就出现了安装问题，没有解决，直接安装VC-Compiler会出错。所以跳过VC-Compiler的安装，待安装完Windows SDK 7.1后再用KB2519277安装包补上。

##OpenCV的安装

   我这里安装的是OpenCV2.4.9，解压缩安装在C:\opencv
将C:\opencv\build\x64\vc10\bin和C:\opencv\build\x86\vc10\bin都加入到系统的Path里面,修改Path路径我推荐使用一个叫[Rapid Environment Editor][4]的编辑器，选择管理员启动即可修改系统环境变量。

 >**注意1**：这里仅仅是设置了了opencv的路径，如果你想要真正在vs中使用opencv，你还需要在vc2010的属性中设置include,lib路径。可以参考[这篇文章。][5]
>**注意2**： 有些文章还强调要自己编译Opencv源码，本人也曾经用Cmake编译过，但是问题很多，后来看MexOpencv的[官网][6]，发现直接使用opencv二进制版本也是可以的，经过本人亲测，确实可以。
>**注意3**： opencv选择(x86 or x64)取决于你的matlab版本是64位还是32位的，不是取决于你的windows系统。比如你运行在Windows 7 64-bit 上运行MATLAB 32-bit 和 Visual Studio 2010 Express,你应该使用 x86 的opencv. 你可能需要重启才能设置路径有效。

##MexOpenCV的安装

下载[mexopencv][7]，如果你使用git，你也可以键入：

> git clone git://github.com/kyamagu/mexopencv.git

或者下载zip,然后解压缩，假设安装到D:\Program Files\mexopencv-2.4
将此文件夹加到Matlab的Path里面并保存。

PS：Matlab必须是2011a及以后的版本。
然后键入：

> \>> mex -setup  

选择编译器为： **Microsoft Software Development Kit (SDK) 7.1 in C:\Program Files (x86)\Microsoft Visual Studio 10.0** ，而不是选择：
 Microsoft Visual C++ 2010 in C:\Program Files (x86)\Microsoft Visual Studio 10.0 

如果以前编译过MexOpenCV，记得先运行
> \>> mexopencv.make('clean', true)

清理一遍。

然后运行
> \>>mexopencv.make('opencv_path', 'C:\opencv\build')


注意后面的路径中一定要包含include文件夹
将所有的cpp文件用mex编译一下。

至此收工，可以试着去运行MexOpenCV\samples里面的例程了。
##MexOpenCV的测试
这时你可以进入mexopencv[主页][8]中开始Getting started
比如：你可以调用Opencv的camera操作：

    % Connect to a camera
    camera = cv.VideoCapture();
    pause(2);
    for i = 1:50
        % Capture and show frame
        frame = camera.read;
        imshow(frame);
        pause(0.3);
    end




参考文献：
  1: http://www.cnblogs.com/youth0826/archive/2013/01/27/2878370.html
  2: https://github.com/kyamagu/mexopencv/tree/v2.4
  3: http://www.cnblogs.com/youth0826/archive/2013/01/26/2878177.html


  [1]: https://dn-xiamenwcy.qbox.me/win7/winsdk_web.exe
  [2]: https://dn-xiamenwcy.qbox.me/win7/VS10sp1-KB983509.exe
  [3]: https://dn-xiamenwcy.qbox.me/win7/VC-Compiler-KB2519277.exe
  [4]: http://www.cr173.com/soft/6681.html
  [5]: http://blog.csdn.net/pinbodexiaozhu/article/details/39889995
  [6]: https://github.com/kyamagu/mexopencv/tree/v2.4
  [7]: https://github.com/kyamagu/mexopencv/tree/v2.4
  [8]: http://kyamagu.github.io/mexopencv/




