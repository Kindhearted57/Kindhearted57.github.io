---
title: "Python Strip的一个小插曲"
key: 2021-05-11-python-strip
pageview: true
tags: python
modify_date: 2021-05-11
sharing: true
show_author_profile: true
show_subscribe: true
license: CC-BY-SA-4.0

---


# Python strip

今天发现了自己代码里的一个xiaobug，总在用strip这个函数但是还是第一次注意到。

我有一堆btc地址，但是都是带有'https://btc.com/'的前缀，于是我就用strip去掉这个前缀，然后奇妙的事情发生了，有这样一个地址：

`https://btc.com/11125y1tMto8KVU8diih62NyfRjmShVgYb`

输出却是：

`11125y1tMto8KVU8diih62NyfRjmShVgY`

找了半天发现原来不止能够把这个字符串对应的内容给strip掉，只要这个字符串里面有的东西都会strip掉，比如我这个字符串最后面的一个`b`也被strip掉了。无语😓