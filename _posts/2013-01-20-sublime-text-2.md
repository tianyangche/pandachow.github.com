---
layout: post
categories: technology
tagline: 
tags: [Sublime Text, Editor, Software]
---

# Java
## 1. 添加 jdk 至环境变量
### 2. 创建链接脚本

新建 runJava.bat/sh, 并添加至 jdk/bin。

{% highlight bash %}
---Windows Version---
@ECHO OFF
cd %~dp1
ECHO Compiling %~nx1.......
IF EXIST %~n1.class (
DEL %~n1.class
)
javac %~nx1
IF EXIST %~n1.class (
ECHO -----------OUTPUT-----------
java %~n1
)

---Linux Version---
#!/bin/sh
[ -f "$1.class" ] && rm $1.class
for file in $1.java
do
echo "Compiling $file........"
javac $file
done
if [ -f "$1.class" ]
then
echo "-----------OUTPUT-----------"
java $1
else
echo " "
fi
---end---
{% endhighlight %}

### 3. 连接 jdk

找到 Sublime Text 2安装路径/Data/Packages/Java/JavaC.sublime-build，拖到 Sublime Text 2 里面，替换行：
{% highlight bash %}
---Windows Version---
"cmd": ["runJava.bat", "$file"]
---Linux Version---
"cmd": ["runJava.sh", "$file_base_name"]
---end---
{% endhighlight %}

## TeX
### 1. 安装Sublime Package Control
在 Sublime Text 2 中 Ctrl+\` 打开终端，输入：
{% highlight bash %}
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'
{% endhighlight %}
### 2. 安装 LaTeXTools 插件

在 Sublime Text 2 中打开命令面板选择 install Package，并搜索到 LaTeXTools 安装即可。
### 3. 连接 TeXLive

发行版选择 TeXLive 无疑是主流的，也是明智的，无论在 Windows，Linux 还是 OS X。

找到 Sublime Text 2安装路径 /Data/Packages/LaTeXTools/LaTeX.sublime-build，拖到 Sublime Text 2 里面，添加：
{% highlight java %}
// *** BEGIN TeXLive 2012 ***
 "cmd": ["latexmk", "-cd",
 	// replace the second pdflatex with xelatex or others to change the TeX engine.
 	"-e", "\\$pdflatex = 'pdflatex %O -interaction=nonstopmode -synctex=1 %S'",
	//"-silent",
 	"-f", "-pdf"],
 "path": "C:\\texlive\\2012\\bin\\win32;$PATH",
// *** END TeXLive 2012 ***
{% endhighlight %}
当然，其实在原来的 TeXLive 2009/2011 的基础上稍作修改即可，注意路径就好。
### 4. 设置预览

到这里下载并安装[SumatraPDF](http://blog.kowalczyk.info/software/sumatrapdf/download-free-pdf-viewer.html)，安装完毕后开终端并切换至 sumatrapdf.exe 所在目录：
{% highlight java %}
//请自行切换安装目录
sumatrapdf.exe -inverse-search "\"C:\Program Files\Sublime Text 2\sublime_text.exe\" \"%f:%l\""
{% endhighlight %}
### 5. reference

[LaTeXTools 的 README@github](https://github.com/SublimeText/LaTeXTools)