---
layout: post
title: "CSV超过限制行数"
key: 2020-02-29-CSV
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---

# CSV问题
## 背景
需要处理一个10w余行的csv文件，其中存在大量的文本信息，文本信息本身也比较大。有一些粘贴的邮件内容。csv文件是,分割，然而邮件中有大量的,和双引号。
## 问题描述
　　遇到了一个超级让人头大的问题，csv的行数超过了numbers的限制行数，且表格中存在双引号的使用问题。CSV实际上是用逗号来分隔不同的数据的，但是由于在正常的文本里面存在了双引号，这个双引号会和整个文本的外边引号结合，因此逗号就会暴露在外面。所以需要将单个双引号的使用改成两个双引号。比如：  
`"这是一句话" `  
要修改成  
`""这是一句话""`  
　　实际操作表明，用numbers修改挺方便的，就是修改过后会丢个几万行。（最后只会保留65535行）这个时候就想起来了Excel，但是用Excel打开，又出现了大量的乱码。

查找发现有一种将csv导入excel的高级方法，打开一个空白的excel，数据->获取外部数据->导入文本文件。
![]()
## 解决方法
　　有以下这么几种解决方法：  

* 我的数据处理量其实还不算大，所以可以切割这个csv文件，然后处理的时候调用多个csv文件。  

* 使用其他的csv处理器，发现，其实Pycharm就可以使用，Pycharm的全局替换快捷键是`command+R`。然而因为文件太大，`command+R`无法使用。下载了两个Pycharm中的plugin，然而由于[某些问题](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360006749739--IntelliJ-Plugin-Error-Plugin-Python-is-incompatible-target-build-range-is-192-7142-to-192-)，也是无法使用，所以打算尝试别的软件。

* 最后在Ubuntu的虚拟机上面做了...很顺畅 LibreOffice，但是也存在一定程度上的压缩，原文件有102998行，在替换之后有102986行。少掉了12行，在我这个需求里面还算无关紧要，所以也就没有太放在心上。

另：最后到底是怎么解决的已经记不住了...不过若干月之后再打开代码的时候发现阅读csv文件的方法是

```
reader = csv.reader(file)
for i in reader:
    ....
```
然后似乎想起来好像用pandas才会出现上述的问题，用这种方法可以避免（不能确认，等后面确认了回来补充）

## 问题描述2

```
Traceback (most recent call last):
  File "/Users/jiru/Documents/GitHub/Bitcoinabuse_analysis/analysis/category_distribution.py", line 13, in <module>
    for i in reader:
  File "/usr/local/Cellar/python/3.7.6_1/Frameworks/Python.framework/Versions/3.7/lib/python3.7/codecs.py", line 322, in decode
    (result, consumed) = self._buffer_decode(data, self.errors, final)
UnicodeDecodeError: 'utf-8' codec can't decode bytes in position 3938-3939: invalid continuation byte
```
这个问题把csv导入文件的时候的代码修改成
`file = open("/Users/jiru/Documents/GitHub/Bitcoinabuse_analysis/data/records_forever-3.csv",'r',encoding='UTF-8',error='igonre')`就好了，[根据](https://www.learnbyexample.org/python-open-function/)这个得知`error='ignore'`是直接忽略无法识别的字符，然而在我使用的过程中并没有出现字符缺失。

## 问题描述3
在我的CSV中存在一些问题，有一些行有多余的字符，或者有一些双引号打的不太对，这个我认为得个别问题个别对待，这个CSV是从网站的api上面下载下来的，所以猜测是网站的问题。因为也不多，所以我决定手工修正他们，如果使用表格软件，excel,numbers的话，会出现如上所述的问题。于是我直接用python脚本修改。