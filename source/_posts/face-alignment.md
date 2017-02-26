title: 3000fps&SDM方法资源摘录
date: 2016-03-19 00:47:31
tags: [人脸对齐]
toc: false
categories: [机器学习,人脸识别]#
---

去年在美图公司实习的时候，研究了一段时间的SDM方法，写了一系列的博客，见《[Supervised Descent Method and its Applications to Face Alignment][1]》等，今年来到了杭州，进入了图片社交领域的佼佼者in实习，主要还是做人脸对齐。最近一段时间，一直在研究《[Face Alignment at 3000 FPS via Regressing Local Binary Features][2]》这篇文章，也为此搜集了很多资料，特整理在此，以供参考。

## paper: 

   3000fps论文链接:
  
-  [Face Alignment at 3000 FPS via Regressing Local Binary Features](http://7xrqgw.com1.z0.glb.clouddn.com/3000fps.pdf)
  
  SDM论文链接:

-  [Supervised Descent Method and its Applications to Face Alignment](http://7xrqgw.com1.z0.glb.clouddn.com/sdm.pdf)
-   [Supervised Descent Method ](http://7xrqgw.com1.z0.glb.clouddn.com/thesis-xiong-2015.pdf)
-   [Global Supervised Descent Method](http://7xrqgw.com1.z0.glb.clouddn.com/Xiong_Global_Supervised_Descent_2015_CVPR_paper.pdf)
- [Supervised Descent Method for Solving Nonlinear Least Squares Problems in Computer Vision](http://7xrqgw.com1.z0.glb.clouddn.com/Supervised%20Descent%20Method%20for%20Solving%20Nonlinear%20Least%20Squares%20Problems%20in%20Computer%20Vision.pdf)
- [我总结的sdm方法ppt](http://7xrqgw.com1.z0.glb.clouddn.com/sdm/sdm.pptx)
- [我总结的sdm方法文档](http://7xrqgw.com1.z0.glb.clouddn.com/sdm/%E6%8A%A5%E5%91%8A.pdf)


## github code linking:

 **3000fps:**
 
- [luoyetx/face-alignment-at-3000fps(C++)](https://github.com/luoyetx/face-alignment-at-3000fps)
- [freesouls/face-alignment-at-3000fps(C++)](https://github.com/freesouls/face-alignment-at-3000fps)
- [yulequan/face-alignment-in-3000fps(C++)](https://github.com/yulequan/face-alignment-in-3000fps)
- [jwyang/face-alignment(matlab)](https://github.com/jwyang/face-alignment)
- [jwyang/face alignment in 3000fps with C++](https://github.com/jwyang/face-alignment-cpp)
- [soundsilence/FaceAlignment](https://github.com/soundsilence/FaceAlignment)

**SDM:**

- [patrikhuber/superviseddescent(C++11)](https://github.com/patrikhuber/superviseddescent)
- [patrikhuber/superviseddescent说明](http://patrikhuber.github.io/superviseddescent/)
- [IntraFace(SDM官网)](http://humansensing.cs.cmu.edu/intraface/)
- [SDM作者主页](http://xiong828.github.io/index.html)
- [IntraFace说明](https://rafaeltibaes.wordpress.com/tag/cc-code/)
- [tntrung/impSDM(matlab)](https://github.com/tntrung/impSDM)
- [IntraFace 官方给的代码，可直接运行](http://7xrqgw.com1.z0.glb.clouddn.com/%E3%80%90C%2B%2B%E7%89%88%E3%80%91intraFacev1.2.rar)

## other linking

- [Face Alignment 最近几年paper收集](https://sites.google.com/site/yanghengcv/face-alignment)
- [An Empirical Study of Recent Face Alignment Methods](https://www.cl.cam.ac.uk/~hy306/FaceAlignment.html#An-Empirical-Study-of-Recent-Face-Alignment-Methods)
- [Heng Yang @ Cambridge](https://sites.google.com/site/yanghengcv/home)
- [flandmark : Open-source implementation of facial landmark detector](http://cmp.felk.cvut.cz/~uricamic/flandmark/)
- [Supervised Descent Method Face Alignment 代码下载和算法研究](http://blog.csdn.net/xp215774576/article/details/46052323)
- [浅谈随机森林在人脸对齐上的应用~](http://blog.csdn.net/wsj998689aa/article/details/45204599)
- [Face Alignment at 3000 FPS via Regressing Local Binary Features(luoyetx'blog)](http://blog.luoyetx.com/2015/08/face-alignment-at-3000fps/#)
- [Face alignment at 3000FPS via Regressing Local Binrary features 理解](http://www.xlgps.com/article/63528.html)
- [C++实现和解读Face Alignment at 3000fps via Local Binary Feature](http://freesouls.github.io/2015/06/07/face-alignment-local-binary-feature/)
- [SDM ppt](http://7xrqgw.com1.z0.glb.clouddn.com/sdmppt.pdf)
- [3000fps ppt](http://7xrqgw.com1.z0.glb.clouddn.com/LBF_slides.pdf)
- [Face Alignment at 3000FPS（C++版）工程配置](http://blog.csdn.net/sunshine_in_moon/article/details/49838245/)
- [Face Alignment at 3000FPS(matlab)工程配置](http://blog.csdn.net/wangjian8006/article/details/42004717)

## 开源库链接及评价
 
1. dlib ：https://github.com/davisking/dlib/tree/v18.18 
**评价**：速度快，可商用，有些时候不太准确
2. CLM-framework: https://github.com/TadasBaltrusaitis/CLM-framework，
新版已经改为**OpenFace**，见：https://github.com/TadasBaltrusaitis/OpenFace
**评价**:很准确，不可商用
3. Face Detection, Pose Estimation and Landmark Localization in the Wild :http://www.ics.uci.edu/~xzhu/face/ 
**评价**:Very slow (~10 seconds an image after hyper threading on a 8-core CPU), but very accurate when it comes to high pose variations
4. SDM patrikhuber/superviseddescent:https://github.com/patrikhuber/superviseddescent
**评价**:Nicely written C++ code, though not very robust
5.  Robust face landmark estimation under occlusion：http://www.vision.caltech.edu/xpburgos/ICCV13/
**评价**:Specially designed for handling occlusions(遮挡区域), but slow on account being written in MATLAB.
6. 应用了CLM的项目：https://www.technologyreview.com/s/541866/this-car-knows-your-next-misstep-before-you-make-it/
**评价**:I actually explored a large number of open-source facial landmark detectors for a project, and found the CLM framework to outperform everything else (in terms of both speed and accuracy). We eventually used it in our project: www.technologyreview.com/news/...
7. clandmark：https://github.com/uricamic/clandmark
8. kylemcdonald/FaceTracker：https://github.com/kylemcdonald/FaceTracker
9. ++[Android appfor facial landmark tracking++][3](点击下载)：https://github.com/ajdroid/facetrackerapp
**评价**：安装该app需要OpenCV Manager，我已提供[链接][4].。
至于效果，不是很好，需要优化。不过上面提供了编写jni的代码，对于编写C++的API应该会有帮助。
10. kylemcdonald/FaceTracker：https://github.com/kylemcdonald/FaceTracker
11. ofxFaceTracker:https://github.com/kylemcdonald/ofxFaceTracker

##参考文献
http://www.learnopencv.com/facial-landmark-detection/#comment-2471797375



  [1]: http://blog.csdn.net/xiamentingtao/article/details/47306887
  [2]: http://7xrqgw.com1.z0.glb.clouddn.com/3000fps.pdf
  [3]: http://7xs15g.com1.z0.glb.clouddn.com/FaceTrackerJS.apk
  [4]: http://7xs15g.com1.z0.glb.clouddn.com/org.opencv.engine.3.00.apk