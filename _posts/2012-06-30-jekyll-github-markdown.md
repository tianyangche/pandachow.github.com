---
layout: post
category : technology
tagline: Moving from Wordpress to Jekyll+Markdown+Github
tags : [Jekyll, Markdown, Github, Blog]
---

近期看了很多人用 Jekyll 和 Vimwiki 搭建的 blog，界面漂亮和简洁让我这个骚年的心开始萌动，心里也在盘算着尽早从 Wordpress 迁过来。Wordpress 虽然功能非常强大，各种插件琳琅满目，界面异常友好，但……实在是有些臃肿了。
## Motivation
**主机**：毫无疑问的我将主机排在了第一名。你觉得谁会去访问一个速度很慢的网站呢～，这次托管在Github就不用理会这个问题啦（啥，Github如果被墙了怎么办？…………）  
**Git**：这个不用我介绍了。来吧，像黑客一样来写博客。  
**Cloud**：如上所说的，Github+Jekyll让我的一切在云端。尽可能地保证了数据的安全，等俺有钱了一定当Github的收费用户。  
**Markdown**：有了它，我们终于可以专注于文章的写作了，不用再去理会字体、行间距这些屁事啦。关于**Markdown**可以看看俺前段时间写的一篇水文[Markdown+R科技文写作](http://xiaoxiongmao.me/2012/06/22/markdown-r-technology-writing.html)。

## Process
<del>暂时文章还没有迁过来，目前的工作完全是在做好铺垫和前面的准备工作，所以说说新站的搭建过程好了。</del>

我的这次博客迁移过程相对来说比较麻烦，其主要原因就是原来自己 wordpress 里面所有的文章 url 都是中文……好吧，我简直肠子都悔青了。
主要导入过程按照 jekyll 给出的 [wiki](https://github.com/mojombo/jekyll/wiki/blog-migrations) 即可，基本上都没什么问题。由于 jekyll 的 blog 完全是静态页面，所以评论部分只能依靠第三方的社会化评论工具，比如 [DISQUS](http://www.disqus.com)，国内的[友言](http://uyan.cc)等等。原先的 wordpress 里面的评论可以通过 WP 插件将其导入，然后再将两边的评论页面做成 csv 文件对应即可，然后提交给DISQUS来 Migrate Threads。

### Github Pages
Github 允许我们来搭建一个个人或者组织的 pages 来做个人介绍，这样既能保证访问速度又能用 Git 来 push，实在是非常爽的啊。

1. 首先你得需要一个**Github**的帐号，如果你没有的话来[这里](https://github.com/plans)注册吧。  
2. 接下来那就可以创建一个 repo 了，不过要记得在 repo 的名字里面填上：**USERNAME.github.com**。  
3. 然后就是在你的机器上配置一下**Git**。不会配置吗？点[这里](https://help.github.com/articles/set-up-git)嘛。  

### Jekyll
Jekyll是用Ruby语言开发的博客引擎，功能上类似于的Wordpress，服务依赖于Github。

1. 首先当然是安装 Jekyll 啦，我们通过 RubyGems 来安装。
		sudo gem install jekyll
2. 如果在gem安装的时候遇到问题，八成是ruby的问题。
		sudo apt-get install ruby1.9.1-dev
3. 接下来就是布置站点结构了，当然了为了省事，你可以使用 [Jekyll-Bootstrap](http://jekyllbootstrap.com/) 或者去 Jekyll 的 wiki 里面[示例页面](https://github.com/mojombo/jekyll/wiki/sites)中找一个顺眼的 fork 即可。（像我这样>.<）
4. 布置完毕后终端进入项目的目录
		jekyll --server  --auto
然后开浏览器[http://localhost:4000/](http://localhost:4000/)即可本地运行啦，其中的auto参数可以让你看到本地改动。如果没有看到欢迎界面需要重新审查上述步骤是否正确。

### Markdown
Linux 下 Markdown 编辑器还真不多，名气最大的也就属 [ReText](http://sourceforge.net/p/retext/home/ReText/) 了吧，请允许我觊觎一下 Mac 平台的 Mou……

### Github来push
如果 Jekyll 安装顺利，本地正常，那么我们就可以 push 到 Github 上啦。这个我就不班门弄斧了，如果你没用过 git，尽情去 google 吧……

### 域名
好了，说到现在总算是把空间搞定了。手头没有域名的可以去买，我是在 [Godaddy](http://www.godaddy.com)上买的，[coupon](http://s.taobao.com/search?q=godaddy&initiative_id=staobaoz_20120726) 很多很便宜，更重要的是它支持支付宝。

但是在国内，我们还需要做些其他工作。Godaddy 的域名解析服务器被墙掉，导致域名无法访问，这事儿在本博诞生的一个月不到的时候就曾经发生过一次，实在是让我痛不欲生。不得已需要把域名解析服务迁移到国内比较稳定的服务商处，这个迁移对于域名来说没有什么风险，最终的控制权还是在 Godaddy 那里。

这方面我不是太了解，朋友当时给我推荐的 DNSPod 的服务，自己用了感觉还可以。免费版功能足够，收费版更全面一些。步骤简单易行：

1. 添加 A 记录、CNAME 记录。
2. 去 Godaddy 那里修改域名的DNS地址。

详细步骤和细节[DNSPod 的使用手册](https://www.dnspod.cn/support/index/fid/1#1)里面写得非常明白。需要注意的是我们的A记录地址是 Github Pages 的服务IP地址：207.97.227.245。
域名的配置部分完成，跪谢方校长。

关于域名的绑定我们直接 push 一个 CNAME 文件到项目的根目录即可，内容为你的域名。

## About
本站主题从 [Linghua Zhang](http://lhzhang.com/) 这里扒过来的，当然我知道这个站是因为 [Yihui Xie](http://yihui.name/) 也是从这扒的……俺是不是无耻了点。

---
总的来说，完全用 Markdown 来写作还需要一段时间的适应。这好这段时间一边练习一边来做好站点的搬家工作。:D

几天前回家了，看看老妈做的**黯然西红柿销魂蛋**。
![](/media/files/2012/06/30/tomato-egg.JPG)