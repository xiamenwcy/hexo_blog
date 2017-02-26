title: 将Github提供的二级域名与自己的域名绑定
date: 2015-06-25 11:01:53
categories: blog #文章文类
tags: [GoDaddy,DNSpod,Hexo]
---
Github page允许将自己的域名与它提供的二级域名绑定，这样，我们可以在访问自己的域名时直接跳转到我们的Github博客主页。下面我们将阐述具体的方法。（以下按照先后顺序进行）
<!--more-->

## GitHub Pages的设置
方法一：在Repository的根目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名，比如wangcaiyong.com。

方法二：到我的[github仓库][1]，点击右下角的「Download ZIP」，下载源文件，解压，找到CNAME文件，用记事本打开，将wangcaiyong.com修改成你的域名，放进Hexo\source目录下，用hexo命令提交上去。

    $ hexo d -g
部署后，点击xiamenwcy.github.io项目Settings选项，然后进入如下页面：
![][2]

![][3]
可以看出github page已经绑定到自己的域名上了。
## Godaddy注册商域名修改DNS地址
参考文章《[Godaddy注册商域名修改DNS地址][4]》设置dnspod来解析域名。文章可能和实际的操作有些区别，原因是GoDaddy改版了，但大致上是一样的。

## DNS设置
用DNSpod，快，免费，稳定。
注册[DNSpod][5]，添加域名，如下图设置。
![][6]
其中A的两条记录指向的ip地址是github Pages的提供的ip

 - 192.30.252.153
 - 192.30.252.154

如博客不能登录，有可能是github更改了空间服务的ip地址，记得及时到在[GitHub Pages][7]查看最新的ip即可.
www指定的记录是你在github注册的仓库。
**注意**：
修改 DNS 服务器需要 0-72 小时的全球生效时间，如果发现某些地方记录没有生效，并且修改 DNS 的时间还不到 72 小时，请耐心等待。我是等了一晚上才发现弄好的。

 我们可以如何确定添加是否生效方法1、登陆你的DNSPod账户后，观察解析的域名，红色是还未生效，蓝色是已生效。方法2、进入命令提示窗口ping一下域名，看看是否解析到指定IP，是则生效。
![][8]

有时候，我们的主域名正在使用着，需要先新建一个博客绑定到子域名，比如: blog.wangcaiyong.com,可以参考《[Hexo在github上构建免费的Web应用][9]》
来设置，最后我们就可以使用自有域名访问github博客了。


  [1]: https://github.com/xiamenwcy/xiamenwcy.github.io
  [2]: https://dn-xiamenwcy.qbox.me/dns/gitpage1.jpg
  [3]: https://dn-xiamenwcy.qbox.me/dns/gitpage2.jpg
  [4]: https://support.dnspod.cn/Kb/showarticle/tsid/42/
  [5]: https://www.dnspod.cn/
  [6]: https://dn-xiamenwcy.qbox.me/dns/dns1.jpg
  [7]: https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/
  [8]: https://dn-xiamenwcy.qbox.me/dns/ping.jpg
  [9]: http://blog.fens.me/hexo-blog-github/