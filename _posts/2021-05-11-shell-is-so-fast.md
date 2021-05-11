---
title: "Shell改变生活"
key: 2021-02-12-bitcoin-address
pageview: true
tags: bitcoin
modify_date: 2021-02-12
sharing: true
show_author_profile: true
show_subscribe: true
license: CC-BY-SA-4.0

---


目标：有一份五千万行的地址txt文件，需要去重，于是直接暴力上了python：

```
list = []
readlines..
for i in readlines:
    if i not in list:
        list.append(i)
...
```
脚本跑起来发现不知道哪年可以跑完，于是想到这一定是一个十分愚蠢的方法，就查了一下大文本查重的方法。
（其实in就是一个速度十分慢的操作）

查到一个十分快的方法：

`sort original.txt | uniq > sorted.txt`

速度快了不知道多少倍，最后也就花了不到半分钟的时间吧。

[为什么这个命令这么快](https://stackoverflow.com/questions/930044/how-could-the-unix-sort-command-sort-a-very-large-file)

说实话这个回答我没有仔细看...但是大概是利用了系统中其他的存储空间以及将文件分块。

[一些其他的讨论](https://stackoverflow.com/questions/7074430/how-do-we-sort-faster-using-unix-sort)