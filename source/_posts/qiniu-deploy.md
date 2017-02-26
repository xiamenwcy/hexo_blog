title: 七牛云存储使用
date: 2015-06-25 11:14:44
tags: [七牛,Hexo]
categories: blog #
---

[七牛][1]是国内专为移动时代开发者打造的数据管理平台，为互联网网站和移动App提供数据的在线托管、传输加速以及图片、音视频等富媒体的云处理服务。使用七牛云存储，可以在线托管图片、视频，可以在线生成外接地址，供博客使用。你可以通过点击[链接][11]
注册并成为标准用户，这样我将获得5GB超大下载流量！十分感谢。
<!--more-->

## 新建空间
![][2]
选择公开空间，通用。当然了选择其它也可以。如果选择私有空间，在后面的生成外部链接不是很方便。
待生成空间后，我们可以去**空间设置**里设置空间的一些属性。
尤其是我们最好设置一个https域名。待设置好https域名后我们
选择默认域名后，在七牛控制台中引用的URL以此域名显示。 
![][3]
![][4]
## 上传资源
**方法1.** 我们可以在空间-->内容管理中上传资料，这种方法是手动上传，且不能使用文件夹上传，一旦数量过多，速度很慢，不方便。
**方法2.** 使用[QRSBox][5]上传资料。 QRSBox有windows版本，但是官网提供的在我的电脑上不能使用，因此我使用了
[格子啦][6]网上提供的资源或者也可以点击链接[QRSBox下载][7]下载使用。
使用方法可以参考[http://developer.qiniu.com/docs/v6/tools/qrsbox.html][8]
和[http://www.mfbuluo.com/5372.html][9]。需要注意的是：
**Windows 平台上路径的表示格式为：盘符:/目录，比如 E 盘下的目录 data 表示为：e:/data 。**
## 图片使用
当我们设置空间为公开空间时，我们上传的资料会自动生成外部链接：
如 `https://dn-***.qbox.me/**/***.png`，当我们设置空间为私有空间时，需要自己设置外部链接，不方便。
我们还可以使用空间的数据处理来修改图片（如图片大小等）。
![][10]
使用范例如下：
`https://dn-portal-files.qbox.me/sample1.jpg?imageView2/1/w/100/h/100/q/75`，即在你设计新的图片样式后将原有外部链接加上**?**号，后面再加上样式df即可。


  [1]: https://portal.qiniu.com/
  [2]: https://dn-xiamenwcy.qbox.me/qiniu/register.jpg
  [3]: https://dn-xiamenwcy.qbox.me/qiniu/qidomain1.jpg
  [4]: https://dn-xiamenwcy.qbox.me/qiniu/qiniudamain2.jpg
  [5]: http://developer.qiniu.com/docs/v6/tools/qrsbox.html
  [6]: http://www.gezila.com/ruanjian/wangluo/74872.html
  [7]: https://dn-xiamenwcy.qbox.me/QRSBox.zip
  [8]: http://developer.qiniu.com/docs/v6/tools/qrsbox.html
  [9]: http://www.mfbuluo.com/5372.html
  [10]: https://dn-xiamenwcy.qbox.me/qiniu/img.jpg
  [11]: https://portal.qiniu.com/signup?code=3le1u161eh7iq