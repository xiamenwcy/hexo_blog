title: Mexopencv的使用
date: 2015-07-14 21:55:52
categories: Matlab #文章文类
tags: [Mexopencv]
---
下面我们将介绍Mexopencv的使用。
首先介绍几个常用的帮助链接：

 1. [MATLAB File Help: cv][1] ：这里列出了cv里的全部函数
 2. [OpenCV 2.3.2 documentation][2] :这里可以搜索到opencv的函数
 3. [mexopencv Github主页][3]
 4. [mexopencv主页][4]
 5. [mex interface for opencv library][5]
<!--more-->

##添加Mexopencv的路径
方法一：在matlab中找到Set Path选项，添加mexopencv路径;
方法二：addpath('/path to mexopencv');如我的路径为：
`addpath('D:/Program Files/mexopencv-2.4');`
## 命名符的使用
在程序中，我们既可以使用带有cv.*的函数，也可以先载入包名，然后直接使用函数，就像C++一样。如： 

```matlab
 img = imread('1.jpg');
 img2=im2double(img);
 myKnernel = [0, -1, 0;-1, 5, -1; 0, -1, 0];
 result = cv.filter2D(img2, myKnernel);  % with package name 'cv'
 import cv.*;
 result =filter2D(img2, myKnernel);  % with package name 'cv'
```

注意：一些函数如cv.imread 重载了MATLAB中的内建函数。如果你想避免命名冲突，可以使用作用域名字,即使用cv.*。
## 帮助
在Matlab可以通help command查找相关函数列表。
```matlab
>> help cv; % shows list of functions in package 'cv'

Contents of cv:

GaussianBlur                   - Smoothes an image using a Gaussian filter
Laplacian                      - Calculates the Laplacian of an image
VideoCapture                   - VideoCapture wrapper class
...

>> help cv.VideoCapture; % shows documentation of VideoCapture

VIDEOCAPTURE  VideoCapture wrapper class

 Class for video capturing from video files or cameras. The class
 provides MATLAB API for capturing video from cameras or for reading
 video files. Here is how the class can be used:
...
```
 mexopencv里面包含了一个简实用文档，可以生成MATLAB的HTML帮助文件。如下命令可以在doc/matlab/下生成用户手册。 
首先设置matlab当前目录为 D:\Program Files\mexopencv-2.4
然后键入：
``` matlab
addpath('utils');
MDoc;
```
你可以运行由test路径下的UnitTest类生成的测试函数。里面包含了mexopencv支持的各种函数功能，一旦出现PASS,说明此功能正常，可以使用。
```matlab
addpath('test');
UnitTest;
```
执行完后，出现如下结果：
``` matlab
== TestANN_MLP ======
-- test_1 ------
PASS
== TestAdaptiveThreshold ======
-- test_1 ------
PASS
-- test_2 ------
PASS
-- test_3 ------
PASS
-- test_4 ------
PASS
-- test_5 ------
PASS
-- test_error_1 ------
PASS
...
```
## 举例说明
查看samples/文件夹，发现里面共有五个文件,均是mexopencv的具体应用。下面我们将具体讲解其中的三个例子。
以下示例均需先加入mexopencv的路径到matlab的path中，并且将samples/加入路径,用代码表达为：
```matlab
addpath('D:/Program Files/mexopencv-2.4');%载入Mexopencv路径
addpath('samples');%载入sample路径
```
### SURF_detector.m
用于显示图片特征点
```matlab 
function SURF_detector
%SURF_DETECTOR  feature detector demo
%
% Before start, addpath('/path/to/mexopencv');
%

%载入图像，并转化为灰度图
im = imread(fullfile(mexopencv.root(),'test','img001.jpg'));
%这里fullfile为定义文件的全路径，可以查看帮助
im = cv.cvtColor(im,'RGB2GRAY');

%设置特征检测方法，这里使用SUFT，也可以使用SIFT或者ORB
detector = cv.FeatureDetector('SURF');
keypoints = detector.detect(im);

%绘制特征点，并显示
im_keypoints = cv.drawKeypoints(im,keypoints);
imshow(im_keypoints);

end

```
运行 SURF_detector，即可出现特征点图。如下：
![][6]
### SURF_descriptor.m
用于计算特征点的特征向量(或者叫特征描述子）
```matlab
function SURF_descriptor
%SURF_DETECTOR  feature detector demo
%
% Before start, addpath('/path/to/mexopencv');
%

%载入图并转为灰度图
im1 = imread(fullfile(mexopencv.root(),'test','img001.jpg'));
im1 = cv.cvtColor(im1,'RGB2GRAY');
im2 = fliplr(im1);%将图像左右翻转


%使用SURF特征检测方法检测特征点
detector = cv.FeatureDetector('SURF');
keypoints1 = detector.detect(im1);
keypoints2 = detector.detect(im2);

%提取特征向量
extractor = cv.DescriptorExtractor('SURF');
descriptors1 = extractor.compute(im1,keypoints1);
descriptors2 = extractor.compute(im1,keypoints2);

%进行特征匹配，这里使用基于L2距离（即欧式距离）的暴力搜索
matcher = cv.DescriptorMatcher('BruteForce');
%  'BruteForce'             L2 distance
%  'BruteForce-L1'          L1 distance
%  'BruteForce-Hamming'     Hamming distance
%  'BruteForce-HammingLUT'  Hamming distance lookup table
%  'FlannBased'             Flann-based indexing
 
matches = matcher.match(descriptors1,descriptors2);

%绘制特征匹配图
im_matches = cv.drawMatches(im1, keypoints1, im2, keypoints2, matches);
imshow(im_matches);

end
```
运行SURF_descriptor，即可出线下图：
![][7]
### facedetect.m
```matlab
function facedetect
%FACEDETECT  face detection demo
%
% Before start, addpath('/path/to/mexopencv');
%
disp('Face detection demo. Press any key when done.');

% Load cascade file
xml_file = fullfile(mexopencv.root(),'test','haarcascade_frontalface_alt2.xml');
classifier = cv.CascadeClassifier(xml_file);

% Set up camera
camera = cv.VideoCapture;
pause(3); % Necessary in some environment. See help cv.VideoCapture

% Set up display window
window = figure('KeyPressFcn',@(obj,evt)setappdata(obj,'flag',true));
setappdata(window,'flag',false);

% Start main loop
while true
    % Grab and preprocess an image
    im = camera.read;
    im = cv.resize(im,0.5);
    gr = cv.cvtColor(im,'RGB2GRAY');
    gr = cv.equalizeHist(gr);%直方图均衡化
    % Detect
    boxes = classifier.detect(gr,'ScaleFactor',1.3,...
                                 'MinNeighbors',2,...
                                 'MinSize',[30,30]);
    % Draw results
    imshow(im);
    for i = 1:numel(boxes)
        rectangle('Position',boxes{i},'EdgeColor','g','LineWidth',2);
    end
    
    % Terminate if any user input
    flag = getappdata(window,'flag');
    if isempty(flag)||flag, break; end
    pause(0.1);
end

% Close
close(window);

end

```
运行facedetect，即可出线人脸框，可以进行人脸识别.
如图：
![][8]

## 发展一个新的 MEX 函数

你所需要做的就是在src/+cv/增加你的MEX源文件。如果你想要增加一个叫myfunc的MEX文件，创建src/+cv/myfunc.cpp. myfunc.cpp将看起来如下：
```matlab
#include "mexopencv.hpp"
void mexFunction(int nlhs, mxArray *plhs[],
                 int nrhs, const mxArray *prhs[])
{
    // Check arguments
    if (nlhs!=1 || nrhs!=1)
        mexErrMsgIdAndTxt("myfunc:invalidArgs", "Wrong number of arguments");

    // Convert MxArray to cv::Mat
    cv::Mat mat = MxArray(prhs[0]).toMat();

    // Do whatever you want

    // Convert cv::Mat back to mxArray*
    plhs[0] = MxArray(mat);
}
```

这个例子简单地复制input到**cv::Mat**对象，然后再复制到输出对象。注意到**MxArray**转化为**cv::Mat** 对象是通过**mexopencv**来实现的。当然了你想要做更多的事情。一旦你创建好一个文件后，键入**mexopencv.make**去建立你的新函数。编译好的MEX函数将会出现在**+cv/**中，你可以在MATLAB中使用**cv.myfunc**来使用它。

**mexopencv.hpp** 头文件包含了一个类 **MxArray** 去操作 **mxArray** 对象. 大多数情况下这个被用来进行 OpenCV 数据类型和  mxArray的转化。
``` matlab
int i            = MxArray(prhs[0]).toInt();
double d         = MxArray(prhs[0]).toDouble();
bool b           = MxArray(prhs[0]).toBool();
std::string s    = MxArray(prhs[0]).toString();
cv::Mat mat      = MxArray(prhs[0]).toMat();   // For pixels
cv::Mat ndmat    = MxArray(prhs[0]).toMatND(); // For N-D array
cv::Point pt     = MxArray(prhs[0]).toPoint();
cv::Size siz     = MxArray(prhs[0]).toSize();
cv::Rect rct     = MxArray(prhs[0]).toRect();
cv::Scalar sc    = MxArray(prhs[0]).toScalar();
cv::SparseMat sp = MxArray(prhs[0]).toSparseMat(); // Only double to float

plhs[0] = MxArray(i);
plhs[0] = MxArray(d);
plhs[0] = MxArray(b);
plhs[0] = MxArray(s);
plhs[0] = MxArray(mat);
plhs[0] = MxArray(ndmat);
plhs[0] = MxArray(pt);
plhs[0] = MxArray(siz);
plhs[0] = MxArray(rct);
plhs[0] = MxArray(sc);
plhs[0] = MxArray(sp); // Only 2D float to double
```



  [1]: http://kyamagu.github.io/mexopencv/matlab/
  [2]: http://www.opencv.org.cn/opencvdoc/2.3.2/html/index.html
  [3]: https://github.com/kyamagu/mexopencv/tree/v2.4
  [4]: http://kyamagu.github.io/mexopencv/
  [5]: http://kyamagu.github.io/mexopencv/html/
  [6]: https://dn-xiamenwcy.qbox.me/mex/img001.jpg
  [7]: https://dn-xiamenwcy.qbox.me/mex/surf_desc.jpg
  [8]: https://dn-xiamenwcy.qbox.me/mex/face.jpg