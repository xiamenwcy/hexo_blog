title: 多说自定义CSS动感头像和多说评论显示User Agent
date: 2015-06-27 08:17:50
categories: blog #文章文类
tags: [多说,Hexo]
---
本文不对以上话题做具体的阐述，因为文章《[多说自定义CSS动感头像和多说评论显示User Agent的那些小事][1]》讲的够详细了，我就是按着上面的步骤逐步添加的，感谢博主的详细讲解。
<!--more-->
下面我就自己在安装使用时遇到的一些问题和注意事项做一些阐述。
   
由于我的博客是基于Hexo，主题是[Jacman][2],所以以下说明仅适于以上主题。
   

## 本地修改embed.js
 注意修改e.user_id多说ID，可以自定义ssk前端显示昵称,即如下的ROOT你可以改成自己的。
以下是[原文章][1]的：
<img src="https://dn-xiamenwcy.qbox.me/duoshuo/css2.jpg" style="display:block;margin:auto"/>
以下是我修改的：
<img src="https://dn-xiamenwcy.qbox.me/duoshuo/css1.jpg" style="display:block;margin:auto"/>
## 修改多说调用地址
在\themes\jacman\layout\_partial\after_footer.ejs中修改多说调用的地址,方法见[原文章][1]。
## 增加Font Awesome
两种方法：
方法一、官方下载压缩包 - http://fontawesome.io/
解压其中的fonts和css，根据你的Blog类型上传至指定目录引入CSS链接即可生效。我是点击 -https://github.com/FortAwesome/Font-Awesome下载的，然后将解压的后的fonts里的文件添加到\themes\jacman\source\font，相同文件覆盖即可。将css中的文件直接添加到\themes\jacman\source\css中。
方法二、直接将如下代码加在 \themes\jacman\layout\_partial\head.ejs 里面的最后一行，`</head>`之前即可.
```  
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```
## 多说后台自定义CSS
[原文章][1]有两个添加css的地方，其实这两个分别对应着多说自定义CSS动感头像和多说评论显示User Agent.
可以直接将两段代码一起添加到多说后台的自定义CSS中，即如下：
```
/*Head Start*/
#ds-thread #ds-reset ul.ds-comments-tabs li.ds-tab a.ds-current {
    border: 0px;
    color: #6D6D6B;
    text-shadow: none;
    background: #F3F3F3;
}

#ds-thread #ds-reset .ds-highlight {
    font-family: Microsoft YaHei, "Helvetica Neue", Helvetica, Arial, Sans-serif;
    ;font-size: 100%;
    color: #6D6D6B !important;
}

#ds-thread #ds-reset ul.ds-comments-tabs li.ds-tab a.ds-current:hover {
    color: #696a52;
    background: #F2F2F2;
}

#ds-thread #ds-reset a.ds-highlight:hover {
    color: #696a52 !important;
}

#ds-thread {
    padding-left: 30px;
}

#ds-thread #ds-reset li.ds-post,#ds-thread #ds-reset #ds-hot-posts {
    overflow: visible;
}

#ds-thread #ds-reset .ds-post-self {
    padding: 10px 0 10px 10px;
}

#ds-thread #ds-reset li.ds-post,#ds-thread #ds-reset .ds-post-self {
    border: 0 !important;
}

#ds-reset .ds-avatar, #ds-thread #ds-reset ul.ds-children .ds-avatar {
    top: 15px;
    left: -20px;
    padding: 5px;
    width: 36px;
    height: 36px;
    box-shadow: -1px 0 1px rgba(0,0,0,.15) inset;
    border-radius: 46px;
    background: #FAFAFA;
}

#ds-thread .ds-avatar a {
    display: inline-block;
    padding: 1px;
    width: 32px;
    height: 32px;
    border: 1px solid #b9baa6;
    border-radius: 50%;
    background-color: #fff !important;
}

#ds-thread .ds-avatar a:hover {
}

#ds-thread .ds-avatar > img {
    margin: 2px 0 0 2px;
}

#ds-thread #ds-reset .ds-replybox {
    box-shadow: none;
}

#ds-thread #ds-reset ul.ds-children .ds-replybox.ds-inline-replybox a.ds-avatar,
#ds-reset .ds-replybox.ds-inline-replybox a.ds-avatar {
    left: 0;
    top: 0;
    padding: 0;
    width: 32px !important;
    height: 32px !important;
    background: none;
    box-shadow: none;
}

#ds-reset .ds-replybox.ds-inline-replybox a.ds-avatar img {
    width: 32px !important;
    height: 32px !important;
    border-radius: 50%;
}

#ds-reset .ds-replybox a.ds-avatar,
#ds-reset .ds-replybox .ds-avatar img {
    padding: 0;
    width: 50px !important;
    height: 50px !important;
    border-radius: 5px;
}

#ds-reset .ds-avatar img {
    width: 32px !important;
    height: 32px !important;
    border-radius: 32px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.22);
    -webkit-transition: .8s all ease-in-out;
    -moz-transition: .4s all ease-in-out;
    -o-transition: .4s all ease-in-out;
    -ms-transition: .4s all ease-in-out;
    transition: .4s all ease-in-out;
}

.ds-post-self:hover .ds-avatar img {
    -webkit-transform: rotateX(360deg);
    -moz-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    transform: rotate(360deg);
}

#ds-thread #ds-reset .ds-comment-body {
    -webkit-transition-delay: initial;
    -webkit-transition-duration: 0.4s;
    -webkit-transition-property: all;
    -webkit-transition-timing-function: initial;
    background: #F7F7F7;
    padding: 15px 15px 15px 47px;
    border-radius: 5px;
    box-shadow: #B8B9B9 0 1px 3px;
    border: white 1px solid;
}

#ds-thread #ds-reset ul.ds-children .ds-comment-body {
    padding-left: 15px;
}

#ds-thread #ds-reset .ds-comment-body p {
    color: #787968;
}

#ds-thread #ds-reset .ds-comments {
    border-bottom: 0px;
}

#ds-thread #ds-reset .ds-powered-by {
    display: none;
}

#ds-thread #ds-reset .ds-comments a.ds-user-name {
    font-weight: normal;
    color: #3D3D3D !important;
}

#ds-thread #ds-reset .ds-comments a.ds-user-name:hover {
    color: #D32 !important;
}

#ds-thread #ds-reset #ds-bubble {
    display: none !important;
}

#ds-thread #ds-reset #ds-hot-posts {
    border: 0;
}

#ds-reset #ds-hot-posts .ds-gradient-bg {
    background: none;
}

#ds-thread #ds-reset .ds-comment-body:hover {
    background-color: #F1F1F1;
    -webkit-transition-delay: initial;
    -webkit-transition-duration: 0.4s;
    -webkit-transition-property: all;
    -webkit-transition-timing-function: initial;
}
/*Head End*/
/*UA Start*/
span.ua {
    margin: 0 1px!important;
    color: #FFFFFF!important;
    /*text-transform: Capitalize!important;
    float: right!important;
    line-height: 18px!important;*/;
}

.ua_other.os_other {
    background-color: #ccc!important;
    color: #fff;
    border: 1px solid #BBB!important;
    border-radius: 4px;
}

.ua_ie {
    background-color: #428bca!important;
    border-color: #357ebd!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_firefox {
    background-color: #f0ad4e!important;
    border-color: #eea236!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_maxthon {
    background-color: #7373B9!important;
    border-color: #7373B9!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_ucweb {
    background-color: #FF740F!important;
    border-color: #d43f3a!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_sogou {
    background-color: #78ACE9!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_2345explorer {
    background-color: #2478B8!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_2345chrome {
    background-color: #F9D024!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_mi {
    background-color: #FF4A00!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_lbbrowser {
    background-color: #FC9D2E!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_chrome {
    background-color: #EE6252!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_qq {
    background-color: #3D88A8!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_apple {
    background-color: #E95620!important;
    border-color: #4cae4c!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.ua_opera {
    background-color: #d9534f!important;
    border-color: #d43f3a!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_vista,.os_2000,.os_windows,.os_xp,.os_7,.os_8,.os_8_1 {
    background-color: #39b3d7!important;
    border-color: #46b8da!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_android {
    background-color: #98C13D!important;
    border-color: #01B171!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_ubuntu {
    background-color: #DD4814!important;
    border-color: #01B171!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_linux {
    background-color: #3A3A3A!important;
    border-color: #1F1F1F!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_mac {
    background-color: #666666!important;
    border-color: #1F1F1F!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_unix {
    background-color: #006600!important;
    border-color: #1F1F1F!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.os_nokia {
    background-color: #014485!important;
    border-color: #1F1F1F!important;
    border-radius: 4px;
    padding: 0 5px!important;
}

.sskadmin {
    background-color: #00a67c!important;
    border-color: #01B171!important;
    border-radius: 4px;
    padding: 0 5px!important;
}
/*UA End*/
```



  [1]: http://wsgzao.github.io/post/duoshuo/
  [2]: https://github.com/wuchong/jacman