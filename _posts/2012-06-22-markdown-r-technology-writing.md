---
layout: post
category : technology
tagline: Writing technology paper with Markdown and R language.
tags : [Markdown, R Language, LaTeX, Typeset]
---

### 不得不说的废话
谈到科技/学术文章的排版，这一直是众多学者、科学家、理工科大学生们所面临的难题，目前来说还比较棘手，而且缺乏相对统一解决办法。就目前我所了解的情况来说，普通青年 Windows Office 流派（X版）、WPS 流派，文艺青年 iWork 流派、LibreOffice/OpenOffice 流派和在线 docs.google 是主要的几类解决方案。而这些方案都没法胜任或者很难胜任科技文的写作，这其中的一个重要的问题就是公式排版。公式排版难看的原因是这些都不属于真正意义上的排版，而是纯文字的堆砌。所以当各式各样的希腊字母和数学符号被当成文字码放在文章中时候，排版效果只能用惨不忍睹来形容。

写到这里的时候，我已经隐隐约约猜到会有人跳出来大叫：我们有 LaTeX！

没错，LaTeX 非常非常好，排版效果简直无懈可击。可是就算是在科技文排版中，LaTeX 的贡献量也没见能超过 Word 多少。推广不利和难以推广并不是最直接的原因，根本原因是TeX不是一般人能精通的。从09年冬天至今我用了2年多 LaTeX，自认为对它还比较熟，但也仅限于使用，至今最有成就的事情也就是读懂和改写了一个学位论文的 template。要是让我去读 LaTeX 包的源代码，就目前的水平和时间来说，读懂的可能性微乎其微，因为代码实在是太复杂。当然了，除此之外，还有一些别的原因导致 LaTeX 久久深入不得众人心，比如前端选择性太多，各种中文文档和手册也相对稀少等等。

Word学习很简单（相对 LaTeX），可是排版能力就非常有限；LaTeX 排版能力极优秀，但是彻底弄明白又绝非一日之工。大部分时候我们对平常技术文章的格式要求远远达不到期刊论文的级别，所以动辄 LaTeX 这么强大的东西实在是伤筋动骨。那究竟有没有一种相对折中的办法呢？

答案当然是肯定的，那就是 Markdown+R。

## Markdown
好，废话写得足够多了，下面开始正文部分。

Markdown 不是软件，而是一种语法非常简单的标记语言。简单到什么程度呢？一个比较流行的说法是5分钟可以彻底学会。当然，如果你会写一点 HTML，我觉得这个时间甚至可以更短的。 ^_^ 好，我们来几个例子吧：

1. 在文字前面打上星号就会成为列表。就像这样 * i * ii 这样子。
2. 多个#号来区分标题级别。###后面跟上几个字就可以作为三号标题了。

怎么样，很简单是不是？为了帮助 Markdown 的语法学习，在后面会推荐一些资料和文章供参考。 好，了解完 Markdown，我们再回过头来看看它究竟比 Word 强在哪儿。我们先来看看一些 Word 用户都会遇到的尴尬，在此我想引用yangzhiping的文章：
	
>* 难以专心：写Word文档的时候，我们经常浪费大量时间在Word本身上，特别是那80%我们用不到的功能。比如，找借口，Word又出问题了；或者，又要升级了。其实，在内心偷笑，哈哈，可以偷懒了
>* 浪费力气在排版上：使用Word时，我们会花费大量力气去排版，试图让文档变得漂亮一些。是粗体还是斜体，是宋体还是黑体，对创作来说，有那么重要吗？
>* 难以自动的版本跟踪：每一位自杀的写作者的电脑文档里面，都必然有一个Word文档，从V1.0到V20.0的无数版本...
>* 难以共同协作：想想你让一位合作的编辑帮你改书有多么痛苦，一个Word文档来，一个Word去，极其难用的修订与审阅功能，你就理解了；

那么相比之下，Markdown 是如何解决的呢？关于排版，哦对不起，我忘了说了，Markdown 生成的文件可以完美转成 HTML，所有的排版工作交给CSS模板即可。关于版本跟踪，我们可以将文档放到 github 或者其他支持 git 版本跟踪的服务器上。所以你可以想见，对于文科论文这样的比较长的文章，Markdown 完全可以秒杀 Word。

## Markdown 与 R
Markdown 的任务就是完成文字部分的排版，并提供非常好的版本跟踪基础。那么接下来的问题就是如何在Markdown排版的文章里面美妙地插入公式和图表。那么它究竟是如何做到的呢？

> 每位试图解决 LaTeX 的不便，又试图保留它的优点的人们，都走上了一条不归路。

>直到有一天，极其熟悉 LaTeX，也熟悉 Markdown 的 yihui 同学，意识到了，LaTeX它可以作为最终格式生成。但是，我们中间的写作过程，完全可以用Markdown这么简单明了的语法来写，我们真正需要的，就是一堆数学公式、图表与参考文献而已。前2者，恰恰是R的强项。后者，则留给开源社区，下一步解决。（可参考[线索1](https://github.com/inukshuk/citeproc-ruby)、[线索 2](https://github.com/inukshuk/jekyll-scholar)）

>于是，在他的新作R包[knitr](http://yihui.name/knitr/)中，果断提供了Markdown支持。并说服R社区主流编辑器厂家，开源软件[RStudio](http://rstudio.org/) 提供 Markdown支持，从而使得Rmd这种新格式开始流行。我们有幸看到这个重要格式的诞生，国人的贡献如此重要。

可以看到，yihui的knitr包提出的是一个让我们感到万分惊喜和兴奋的解决方案。什么，你要问排版效果？[点此](http://rpubs.com/Ailurus/564)。如你所见，效果已经比 Word 胜出好几个档次。

## 我该从哪儿学起
好了，Markdown以及R的铺垫部分说得差不多了，那么对于没接触过它们的同学们来说，如何才能尽快上手并进行创作？

### 关于 Markdown
1. 语法：可以参考 [Markdown](http://markdown.tw/)，当然我们的 [riku](http://riku.wowubuntu.com/)同志已经翻译成了[简体中文版](http://wowubuntu.com/markdown/)。
2. 讨论：[V2EX节点](http://v2ex.com/go/markdown)。
3. 编辑器：Mac下广受好评的Mou；Linux下选择就很多了，RStudio/ReText 等等；Windows 不知道。
4. RMD：格式详述可以参考yihui本人的[自动化报告](https://github.com/yihui/r-ninja/blob/master/11-auto-report.md)。

### 关于 R 语言
1. R学习笔记：[http://statlab.nchc.org.tw/rnotes/?page_id=44](http://statlab.nchc.org.tw/rnotes/?page_id=44)
2. 统计之都：[http://cos.name/cn/](http://cos.name/cn/)
3. 丕子的博文：[http://www.zhizhihu.com/html/y2011/3157.html](http://www.zhizhihu.com/html/y2011/3157.html)
4. 当然你也可以常来我的blog逛逛，没准我也会写点 ^_^

### 如何搭建Markdown+R的写作环境
可以参考阳志平的文章：[为什么Markdown+R有较大概率成为科技写作主流？](http://www.yangzhiping.com/tech/r-markdown-knitr.html)，里面对RStudio的配置说得非常详细，关于knitr的安装可以参考knitr的[README](https://github.com/yihui/knitr#readme)。<del>不过需要注意的是，我自己在knitr在线安装的时候报错，原因是[highlight](http://cran.r-project.org/src/contrib/Archive/highlight/)和[parser](http://cran.r-project.org/src/contrib/Archive/parser/)这两个依赖包在官方源中已经被移除了，需要从archive里面找到自行安装。</del>

## 最后一点废话
Markdown+R 是排版解决方案，Word和LaTeX也是。它们学习曲线有差异，排版能力也有差异。而我们要做的，是明白自己的排版需求，并以此来选择最合适自己的解决方案。它们终究只是工具，目的是为你营造一个舒适并方便的写作环境，让你用尽量少的时间去关注格式和字体，专心写作。所以，在折腾这些的同时你需要思考自己的写作需求，选择一个不适合你的写作工具而浪费大量本该花在写作上时间，是本末导致、得不偿失的。