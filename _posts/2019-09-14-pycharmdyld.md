---
title: "Pycharm dyld:Library not loaded"
key: 2019-09-14-pycharmdyld
pageview: true
modify_date: 2020-09-14
shraring: true
show_author_profile: true
show_subscribe: true
---

# ERROR| MAC Pycharm_dyld: Library not loaded
## [尝试1](https://stackoverflow.com/questions/31497217/osx-dyld-library-not-loaded-reason-image-not-found-python-google-speech-re)
Stackoverflow的方法 先安装homebrew，然后
`brew install flac` 
## 尝试添加库
![pic1](https://note.youdao.com/yws/api/personal/file/WEBe75e7aaafe309de44d9282b6aeb83349?method=download&shareKey=73d3e83b85c08a76d234c60dd0b64455)
## File > Invalidate Caches/Restart..
无效
## Final solution
整不对了...重新建了一个project好了🙄
## FFinal solution
一年之后回头再看这个问题，发现之前完全是因为没有了解原因所以就以为无法解决。之所以出现问题是因为virtualenv出现了问题，而之所以新建一个project能够解决问题，是因为新建一个project可以创建一个新的virtualenv。
### Python Interpreter
Pycharm->preference->Project:project_name->Project Interpreter

在这个位置可以看到project interpreter，下面的package相当于用pip安装的包。

![](https://note.youdao.com/yws/api/personal/file/WEBe46c845e06aee5a9b7377bb963d6f6ae?method=download&shareKey=fc86c4519dadce5a1dc07898c26fc4b3)
当想新建一个python interpreter的时候可以看到两个选项，其中`location`指的是存放当前virtualenv所需要的环境，`base interpreter`指的是创建这个虚拟环境需要基于本地的python环境的位置。
![](https://note.youdao.com/yws/api/personal/file/WEB572d830251859217cc9e1f9388150947?method=download&shareKey=43f1f5fdefc2bf445a05ba751a6c5f1d)

所以上面出现这个问题的原因如下：假设之前使用的是3.7.1的版本，现在升级到3.7.3的时候，中间的一些链接会出错，而之所以会升级可能是Python的版本可能在某一次使用brew的时候被更新到了，[导致](https://www.jianshu.com/p/4968e2585a94)之前创建到的virtualenv损坏。所以需要换用virtualenv才能跑通原先的代码。


### Mac Python版本的说明
Mac自带的python版本为2.7，可执行文件的位置在；

`/System/Library/Frameworks/Python.framework/Versions/2.7/bin/python2.7`

在`/Versions`下面可能有多个python版本，其中`Current`存放系统当前python版本，在`Current/bin`目录下输入`./python --Version`，查看到的就是系统当前默认的python版本，其中如果使用`python --version`命令查看得到的是用户当前的python版本而不是系统的python版本。Homebrew安装python的路径是`/usr/local/Cellar`，由于我用Homebrew安装的是3.8的版本，所以路径是`/usr/local/Cellar/python@3.8`。

当输入`which python`的时候返回地址`/usr/bin/python`
[这个地址](https://stackoverflow.com/questions/48740260/osx-whats-the-difference-between-usr-bin-python-and-system-library-framewor)是一个wrapper的地址，用来选择在`com.apple.versioner.python.plist`中指定的版本号。这也是系统默认的命令路径。

`which python3`返回的地址是`/usr/local/bin/python3`这个地址是用户命令的默认路径。

如果存在相同的命令，会按照`/etc/paths`文件中的环境变量顺序依次查找，查看环境变量也可以在终端输入`echo $PATH`，左边路径的优先级优于右边的路径。

对于python2和python3的兼容问题，通常是保留python2，当需要使用3.X版本的时候，输入python3/pip3来区别。

我们可以看到，上面2.7的可执行文件的位置`/System/Library/Frameworks/Python.framework/Version`并没有3.X的版本。

