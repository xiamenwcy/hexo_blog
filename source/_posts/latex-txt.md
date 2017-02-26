title: 用 LaTeX 排版技术书籍
date: 2016-10-10 19:16:18
categories: 常用技巧
toc: false
tags: [Latex]
---
最近要写技术文档，里面包含大量的数学公式，本来想用markdown，但是
markdown不适合提交到公司，于是想着还是用自己学过的Latex吧。

于是在网上去搜使用Latex写技术文档/技术书籍的模板，还真找到了一篇，即[《用 LaTeX 排版技术书籍》][1]。本来他提示使用tex studio编译，但我使用tex studio编译后可以运行，却不能导出pdf，总是提示：
```tex 
<span style="font-size:18px;">** WARNING ** Obsolete four arguments of "endchar" will be used for Type 1 "seac" operator.
** ERROR ** This font using the "seac" command for accented characters...

Output file removed.
 )
(see the transcript file for additional information)
Error 1 (driver return code) generating output;</span>
```
查了半天原因。最后定位在一个list环境中。查了一下google，一下子就找到了原来也有人犯过[同样的毛病][2]。具体的改正方法竟然是将一个**“,”**移到**“}”**外面去，结果一下子就编译成功了。
<!--more-->

下面叙述一下我的安装步骤（仅限在Windows平台），以供后来者借鉴。

1. 在[Github][3]上下载源文件
2. 下载所需字体，见百度网盘：http://pan.baidu.com/s/1qXKFSxY ,下载完后直接点击安装即可。
3. 安装ctex,可以在[这里][4]下载。或者也可以安装tex studio，安装tex studio，应该先安装ctex或者MikTex,再安装tex studio。
**注意：** MikTex才是tex核心（包管理器），而tex studio或者ctex是编辑器而已。也可以参考《[TeX Live & TeXstudio 安装手记][5]》使用TeX Live作为包管理器，只是我在windows下安装失败了。
所以我还是选择了ctex和MikTex+tex studio。
4. 安装好以后打开下载好的文件，在里面找到chapLayout.tex,打开，定位到190行左右，
``` tex
\begindot
\item[] {\small \fontspec{Latin Modern Mono} printf("Hello \%s\bs n", name);} [cmtt]
\item[] {\small \fontspec[Mapping=tex-text-tt]{Inconsolata} printf("Hello \%s\bs n", name); }[Inconsolata]
\myenddot
```
将第二行的**";"**写到**“}”**外面去，即：
```tex
\begindot
\item[] {\small \fontspec{Latin Modern Mono} printf("Hello \%s\bs n", name);} [cmtt]
\item[] {\small \fontspec[Mapping=tex-text-tt]{Inconsolata} printf("Hello \%s\bs n", name)}; [Inconsolata]
\myenddot
```
5.编译运行即可。

  [1]: https://github.com/chenshuo/typeset
  [2]: http://www.voidcn.com/blog/u012176591/article/p-1556407.html
  [3]: https://github.com/chenshuo/typeset
  [4]: http://www.ctex.org/HomePage
  [5]: http://www.cnblogs.com/joyeecheung/p/3596255.html
