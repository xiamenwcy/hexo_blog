title: Hexo静态博客使用不蒜子添加计数功能
date: 2015-06-26 22:40:16
categories: blog #文章文类
tags: [不蒜子,Hexo]
toc: false
---
静态网站建站现在有很多快速的技术和平台，但静态是优点也有缺点，由于是静态的，一些动态的内容如评论、计数等等模块就需要借助外来平台，评论有“多说”，计数有“[不蒜子官网][1]”！
<!--more-->

使用方法很简单，只需要简单的两行代码，搞定计数。
 基本模式是：一行脚本+一行标签
一、安装脚本（必选）
打开themes/你的主题/layout/_partial/footer.pejs添加如下脚本即可，当然你也可以添加到 header 中。
```
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js">
</script>
```
二、安装标签（可选）
1、显示站点总访问量
要显示站点总访问量，复制以下代码添加到你需要显示的位置。可以打开themes/你的主题/layout/_partial/footer.ejs添加即可。
有两种算法可选：

算法a：pv的方式，单个用户连续点击n篇文章，记录n次访问量。
```
<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>
```
算法b：uv的方式，单个用户连续点击n篇文章，只记录1次访客数。
```
<span id="busuanzi_container_site_uv">
  本站访客数<span id="busuanzi_value_site_uv"></span>人次
</span>
```
2、显示单页面访问量
要显示每篇文章的访问量，复制以下代码添加到你需要显示的位置。

算法：pv的方式，单个用户点击1篇文章，本篇文章记录1次阅读量。
```
<span id="busuanzi_container_page_pv">
  本文总阅读量<span id="busuanzi_value_page_pv"></span>次
</span>
```
3、显示站点总访问量和单页面访问量

上面两种标签代码都安装。

4、只计数不显示

只安装脚本代码，不安装标签代码。

综上，你也可以使用极简模式：
```
本站总访问量<span id="busuanzi_value_site_pv"></span>次
本站访客数<span id="busuanzi_value_site_uv"></span>人次
本文总阅读量<span id="busuanzi_value_page_pv"></span>次
```
或者个性化一下：
```
Total <span id="busuanzi_value_site_pv"></span> views.
您是xxx的第<span id="busuanzi_value_site_uv"></span>个小伙伴
<span id="busuanzi_value_page_pv"></span> Hits
```
具体可以参考《[不蒜子][2]》
我的hexo博客是基于Jacman主题，我想实现如下效果：
<img src="https://dn-xiamenwcy.qbox.me/busuanzi/she.jpg" style="display:block;margin:auto"/>
最后我在themes/jacman/layout/_partial/footer.ejs中添加如下代码：
```
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js">
 </script>
 </br>本站总访问量<span id="busuanzi_value_site_pv"></span>次，本站访客数<span id="busuanzi_value_site_uv"></span>人次，本文总阅读量<span id="busuanzi_value_page_pv"></span>次

```
添加位置如下图：
![][3]
注意：

 这里的`</br>`是[换行][4]的意思，因为我们需要在
 ``Powered by hexo and Theme by Jacman © 2015 Caiyong Wang ``
 之后显示访问语句，所以需要换行，不添加`</br>`的话，将显示为一行。


  [1]: http://service.ibruce.info/
  [2]: http://ibruce.info/2015/04/04/busuanzi/
  [3]: https://dn-xiamenwcy.qbox.me/busuanzi/add.jpg
  [4]: http://www.hsyx.net/909.html