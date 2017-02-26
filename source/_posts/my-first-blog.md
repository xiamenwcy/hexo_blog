title: 使用Hexo搭建主题为Jacman的博客
date: 2015-06-21 16:02:15
categories: blog #文章文类
tags: [博客，文章]
---
这里仅仅列出我搭建网站时用的一些链接。大致上按照先后顺序。
>1. [史上最详细“截图”搭建Hexo博客并部署到Github](http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html)
>2.  [hexo系列教程](http://zipperary.com/categories/hexo/)
>3.  [如何使用 Jacman 主题](http://wuchong.me/jacman/2014/11/20/how-to-use-jacman/)
>4.  [使用Landscape Plus主题](http://jasonxiang.com/landscape-plus/2015/05/07/Landscape-plus/#ds-thread)
>5.  [Hexo官网](https://hexo.io/zh-cn/docs/index.html)
>6.  [Jacman Github地址](https://github.com/wuchong/jacman)
>7.  [如何搭建一个独立博客——简明Github Pages与Hexo教程](http://www.jianshu.com/p/05289a4bc8b2)
>8.  [使用hexo搭建博客](http://yangjian.me/workspace/building-blog-with-hexo/)

推荐两个markdown在线和线下编辑器：
>- [Cmd Markdown 在线编辑阅读器](https://www.zybuluo.com/mdeditor)
>-  [CuteMarkEd线下编辑器](http://cloose.github.io/CuteMarkEd/)
>-  [Markdown语法说明1](http://www.markdown.cn/)
>-  [Markdown语法说明2](http://blog.chinaunix.net/uid-21712186-id-3483852.html)
<!--more-->
注意事项：

>1. 安装新浪微博秀时，除了必须填上author属性下tsina和weibo_verifier的值，前者是您微博ID，后者是您微博秀的验证码，访问 http://app.weibo.com/tool/weiboshow 可以获得您的 verifier。
如下图：
![](http://7xjv9y.com1.z0.glb.clouddn.com/sina_show.JPG?attname=&e=1434985163&token=4MEaKHFpIyQCXFDjZQ9f0DmE0TMUFXIys9b85eTo:CcfzXKR_ukn2WzhnGPIZ5-GR7S4)
其中uid即为tsina, verifier也就是我们需要的 verifier。
填完后，还需要在在themes/jacman/_config.yml中，
添加如下：
> > widgets:
 >  >- weibo
 >2. 使用hexo new photo "your titile"建立图片类文章,相关方法见
[如何使用 Jacman 主题](http://wuchong.me/jacman/2014/11/20/how-to-use-jacman/) 
>3. 使用hexo new "my new post"建立普通blog
>4. 使用hexo new page "pageName" #新建页面
>5. 关于网站的优化见：《[Hexo 静态博客加速][1]》、《[Hexo折腾笔记（一）博客加速以及解决instantclick的兼容问题][2]》、《[Hexo输出内容优化][3]》


  [1]: http://lukang.me/2014/hexo-page-speed.html
  [2]: http://kevinsfork.info/2015/02/25/hexo-speedup-instantclick/
  [3]: https://github.com/FlashSoft/hexo-console-optimize