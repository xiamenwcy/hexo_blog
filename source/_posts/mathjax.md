title: 在 Jacman使用 Mathjax 输出数学公式
date: 2015-06-25 23:54:28
categories: blog #文章文类
mathjax: true
tags: [Mathjax,Hexo]
---
## 配置Mathjax
由于Jacman主题支持写 LaTex 数学公式,因此只需要做到下面两步，即可使用。
1、在主题Jacman的_config.yml加入mathjax: true，即
```
close_aside: false  #close sidebar in post page if true
mathjax: true      #enable mathjax if true
```
2、在文章文件开头的前言中，加上一行mathjax: true，即可在文中写 LaTex 公式。
```
title: 测试Mathjax
date: 2014-2-14 23:25:23
tags: Mathmatics
categories: Mathjax
mathjax: true
---
```
<!--more-->
也可参考文章
《[在 Hexo 中完美使用 Mathjax 输出数学公式][1]》，但是这篇文章中讲的问题在Jacman中没有出现。下面举例说明：

3、原文章中有下面一段话 
```
博主你好，你这样用mathjax，在用markdown写博客时能正常书写带下标的公式吗？
比如下面一段话：
This is an example for $x_mu$ and $y_mu$.
两个`—`会被看成markdown中的斜体。
```
这个的解决办法可以使用转义符, 即如下输出即可
```
This is an example for $x\_mu$ and $y\_mu$.
```
经在Jacman中试验：
效果：
      This is an example for $x_mu$ and $y_mu$.
      This is an example for $x\_mu$ and $y\_mu$.
效果是一样的。
## 安装Hexo-math
[Hexo-math][2]插件在Jacman中作用很大，可以实现：

- 自动部署MathJax
- 添加MathJax Tag

安装也十分方便：
```
npm install hexo-math --save
```
然后在全局_config.yml文件中添加：
```
plugins:
- hexo-math
```
新版hexo-math不再需要运行 
```
hexo math install
```
如下举例说明hexo-math的用法：

**MathJax Inline：**

```markdown
Simple inline $a = b + c$.
```
效果：

Simple inline $a = b + c$.

**MathJax Block：**
```markdown
$$\frac{\partial u}{\partial t}
= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
\frac{\partial^2 u}{\partial y^2} +
\frac{\partial^2 u}{\partial z^2}\right)$$
```
效果：
$$\frac{\partial u}{\partial t}
= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
\frac{\partial^2 u}{\partial y^2} +
\frac{\partial^2 u}{\partial z^2}\right)$$

**Tag inline：**
```markdown
This equation {% math \cos 2\theta = \cos^2 \theta - \sin^2 \theta =  2 \cos^2 \theta - 1 %} is inline.
```
效果：
This equation {% math \cos 2\theta = \cos^2 \theta - \sin^2 \theta =  2 \cos^2 \theta - 1 %} is inline.

**Tag Block：**
```markdown
{% math_block %}
\begin{aligned}
\dot{x} & = \sigma(y-x) \\
\dot{y} & = \rho x - y - xz \\
\dot{z} & = -\beta z + xy
\end{aligned}
{% endmath_block %}
```
效果：

{% math_block %}
\begin{aligned}
\dot{x} & = \sigma(y-x) \\
\dot{y} & = \rho x - y - xz \\
\dot{z} & = -\beta z + xy
\end{aligned}
{% endmath_block %}


  [1]: http://lukang.me/2014/mathjax-for-hexo.html
  [2]: https://github.com/akfish/hexo-math
