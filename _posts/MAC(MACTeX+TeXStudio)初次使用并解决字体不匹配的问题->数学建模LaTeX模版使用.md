---

title: "MAC(MACTeX+TeXStudio)初次使用并解决字体不匹配的问题->数学建模LaTeX模版使用"
key: 2019-07-19-how-to-win-national-mathematic-modeling-competition
tags: Ubuntu
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true

---

 
# MAC(MACTeX+TeXStudio)初次使用并解决字体不匹配的问题->数学建模LaTeX模版使用

从在Mac上使用LaTeX说起，首先需要安装一个MacTeX，两种方法

* 使用brew安装 -> [参考内容](https://www.douban.com/note/652845898/)
* 去官网安装 -> [链接]()

安装之后发现多了这些东西。

![](https://note.youdao.com/yws/api/personal/file/WEBa393c195f3392ad26c89f75ce695c511?method=download&shareKey=9d2a1d16f86c725e920488836e007b82)

我们暂时别管这些是干嘛的，下一步是需要一个LaTeX的编辑器，我用的是TeXStudio，因为之前在Windows上就用的这个比较熟悉，似乎上图的TeXShop也是一个编辑器，但是我不太懂怎么用qwq

目前为止我的LaTeX能力还仅限于套用模板......因为是国赛，所以使用的是这个模板[cumcmthesis](https://github.com/latexstudio/CUMCMThesis)

如果是美赛的话，可以使用这个文档。（偷偷说这个链接是目前维护者的个人主页）[mcmthesis](https://liam.page/2016/01/27/how-to-use-mcmthesis/)

以我现在使用的国赛模版为例，下载的zip打开之后是这个样子的

![](https://note.youdao.com/yws/api/personal/file/WEB517b46123d75dc4bab641c942fc24b45?method=download&shareKey=5887c53b8901a20d3bd3bb0945cc535c)

如果你已经安装好了比如TeXStudio之类的编辑器的话，双击.tex文件就可以直接跳转到TeXStudio的编辑界面了。其中的一些图片文件是示例图片，如果你需要插入别的图片的话，也要先放到这个文件夹里面。

这个文件夹的example.pdf文件是会随着编辑器里面的运行改变的，也就是最后生成的pdf就是example.pdf。

好的下面说我今天遇到的问题。打开之后运行，报错报了很久。如下

![](https://note.youdao.com/yws/api/personal/file/WEB71064e37ab8f7a60903724098267b4c8?method=download&shareKey=c80110e41e1830a0bf9c801ec7525ead)

在运行的过程中有如下显示

![](https://note.youdao.com/yws/api/personal/file/WEBe5147127329e9f7e65ed5cc62d203cc6?method=download&shareKey=cc9590809f31063858005e521cfd805a)

刚开始一直以为自己的MacTeX安装的有问题，后来仔细看了一下，发现似乎是缺失了某种字体的意思。

于是就去安装字体，在网上找了安装包直接安装了几种常见的中文字体 

.ttf或者.ttc文件，记得下载过后双击安装。

但是安装了之后并没有解决问题，我这出现的情况是，宋体的error不见了，但是楷体还是一直在报error。

于是看到了这篇https://bbs.pku.edu.cn/v2/post-read.php?bid=134&threadid=14993727，2014年北大bbs上面的帖子。

哦。原来原因是windows和mac下面的字体名字不一样。

两种查看字体名称的方法。

1.brew install fontconfig->fc-list :lang=zh

记得冒号前面有个空格。这种方法里面lang=zh是查看全部中文字体。

2.在启动台找到这个东西
![](https://note.youdao.com/yws/api/personal/file/WEBe7492a5d2367bc51892bb1ce310e9a86?method=download&shareKey=5ba5569507d6d49c08d5838e56017d19)

打开之后会发现有各种字体的信息
![](https://note.youdao.com/yws/api/personal/file/WEBdff5d33ffe9976eb636d4b5ea7e57da6?method=download&shareKey=5b2fc99354bd4b5560b3b4647ab188d7)

比如一直困扰我的楷体，使用command+i查看详细信息:
![](https://note.youdao.com/yws/api/personal/file/WEBb968e4a1e8ecbc4d8459858e8804bbd3?method=download&shareKey=65c347cdbbf2bf96f6f3aab0bfcac8ab)

注意喔，框框起来的才是他的名字，而不是.ttf前面的那个文件名（其实.ttf前面是他在Windows下面的名字）

 

本来照着楼主的路径找了好几次（还翻了一些别的博客），也没找到正确的文件。后来想了想，其实也就是在刚刚那个压缩包里面的.cls文件里面吧，然后就打开看了一下。

![](https://note.youdao.com/yws/api/personal/file/WEB434fe9a5c7e249d5bc8eb26428490545?method=download&shareKey=b927d7e975ae62f1d687af38ac7eaf34)

果然在哦！找到你了。

![](https://note.youdao.com/yws/api/personal/file/WEB0adcd11b4c7099cce78ca8c0c23f1cc6?method=download&shareKey=242da351d993576ccd98594e33de9239)

前面疯狂报错的来源就是

![](https://note.youdao.com/yws/api/personal/file/WEBb3515be59c4233a1f0bdce1d8c4e0ccd?method=download&shareKey=fb6bebf0347749557fa47a19afa40aee)

（测试了一下这个地方写不写.ttf好像都可以，不明原理）

改之，记得保存

![](https://note.youdao.com/yws/api/personal/file/WEB925fdd9dc2064857ff70e21a55426ff0?method=download&shareKey=4ecca01e802164268a620dde839ec07f)

然后就可以运行辣！

![](https://note.youdao.com/yws/api/personal/file/WEB3feb793a169e2821fad939bdaddf8451?method=download&shareKey=eb14a9c6112db8180ae7d8cd703384e8)

ps:似乎汉字都会被画上红线...英文就不会，可能是语言的问题，不要在意。

Happy texing:)
