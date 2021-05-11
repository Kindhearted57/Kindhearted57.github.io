---

title: "如何优雅地在jupyter上使用php"
key: 2021-01-31-how-to-use-php-in-jupyter
tags: php
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
license: CC-BY-SA-4.0

---

#如何优雅地在jupyter上使用php

php零基础，奈何又要用，正好下午在用jupyter，想着直接搭个php的环境上去岂不美滋滋。结果没想到困难重重。

##php+jupyter

大致的步骤是这样的，[参考](https://segmentfault.com/a/1190000016107828):

### PHP安装
从[菜鸟教程](https://www.runoob.com/php/php-install.html)看过来的，直接安装PHP服务器组件，包含PHP、Apache、Mysql等服务。在MacOS下安装[XAMPP](https://www.apachefriends.org/zh_cn/index.html)

安装之后看一下：`php --version`

```
57MacBook-Pro:~ jiru$ php --version
PHP 7.3.9 (cli) (built: Nov  9 2019 08:08:13) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.9, Copyright (c) 1998-2018 Zend Technologies
```

###安装jupyter notebook

这个教程不少，没有遇到什么问题。想记下来的是在使用jupyter中间遇到了一个比较蠢的问题，jupyter上面能够运行代码的是ipynb格式，而不是.py格式。想要运行代码记得创建新notebook。

### jupyter的PHP内核安装

####全局安装Composer

```
curl -sS https://getcomposer.org/installer | php

mv composer.phar /usr/local/bin/composer
```
#### 安装PHP-ZMQ扩展
##### 下载编译

```
git clone git://github.com/mkoppanen/php-zmq.git

cd php-zmq

phpize && ./configure

make && make install
```

在phpize的时候，遇到问题

```
grep: /usr/include/php/main/php.h: No such file or directory
grep: /usr/include/php/Zend/zend_modules.h: No such file or directory
grep: /usr/include/php/Zend/zend_extensions.h: No such file or directory
Configuring for:
PHP Api Version:        
Zend Module Api No:     
Zend Extension Api No:  
```
经过一番探索，这个问题的推荐解决方法通常是这样的

```
sudo ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/usr/include /usr/include
```
而为了symlink这一步，就需要disable SIP，方法如下：

重启电脑，输入command+R，然后在登录账户输入密码之后，可以看到上方的菜单栏有实用工具。从这里打开终端，输入`csrutil  disable`

正常打开系统终端，输入`csrutil status`，查看SIP的情况。


参考[这一篇](https://zhile.io/2018/09/26/macOS-10.14-install-sdk-headers.html)写的很好的文章。

然而在我的情况下这样并不可以。探究原因是`/usr/include`文件夹不存在，前面提到的文章里面提到可能是`xcode command line tools`出了问题。

关于xcode command line tools 我们又参考到了[这一篇文章](https://liam.page/2020/10/13/install-XCode-Command-Line-Tools-manually/)，我决定向博主看齐，也去手动安装。我的xcode的版本是11.7，但是官网并没有11.7，于是我就下载一个11.5的康康怎么样。

不怎么样。

于是我放弃xcode这边问题的思路，转向一些别的解决方式。

看到这个网页：
[Missing /usr/include on MacOS 10.15 Catalina](https://bbqsoftwares.com/blog/xdebug-catalina-issue)

了解了应该是这样一个逻辑：Catalina更新之后





安装PEAR "PHP Extension and Application Repository"

```
57MacBook-Pro:~ jiru$ pear version
PEAR Version: 1.10.12
PHP Version: 7.3.9
Zend Engine Version: 3.3.9
Running on: Darwin 57MacBook-Pro.local 19.2.0 Darwin Kernel Version 19.2.0: Sat Nov  9 03:47:04 PST 2019; root:xnu-6153.61.1~20/RELEASE_X86_64 x86_64
```


```
So now the issue is, in order to compile XDebug, we need to either tell the compiler to use the headers from a different location, or actually put the required files in /usr/include...
```

```
Cannot find config.m4. 
Make sure that you run '/usr/bin/phpize' in the top level source directory of the module
```


```
Configuring for:
PHP Api Version:         20180731
Zend Module Api No:      20180731
Zend Extension Api No:   320180731

```

```
configure: error: Unable to find libzmq installation

```

`brew install zmq`
或者
`apt-get install libzmq-dev`

```
sh: Fusion.app/Contents/Public:/Library/TeX/texbin:/usr/local/share/dotnet:/opt/X11/bin:~/.dotnet/tools:/Library/Apple/usr/bin:/Library/Frameworks/Mono.framework/Versions/Current/Commands:/Applications/Wireshark.app/Contents/MacOS::/usr/local/Cellar/cassandra/3.11.6/libexec//bin:/Applications/010: No such file or directory
```

参考了github上面的[一个issue](https://github.com/Litipk/Jupyter-PHP-Installer/issues/20)，理解了大概的原因和解决方法，需要清空一下PATH变量，在这里我们的方法可以是使用一个临时的PATH变量。

这里面需要插播一个知识点，关于Mac的PATH的问题。
[](https://imstudio.medium.com/path-macos-best-practice-for-path-environment-variables-on-mac-os-35ec4076a486)
[How does Mac OS X set the value of $PATH](https://superuser.com/questions/653803/how-does-mac-os-x-set-the-value-of-path)



```
##### 修改配置

```
修改 php.ini 配置，添加 `extension=/path/modules/zmq.so`
```











