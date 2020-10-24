---
layout: post
title: "Python list 赋值"
key: 2020-09-18-Python-list-赋值
pageview: true
modify_date: 2020-09-18
shraring: true
show_author_profile: true
show_subscribe: true
---

# Python拷贝
今天写代码，有两个list，其中一个是原始的list，另外一个排序之后拿着数值到第一个去找数，但是发现数据出了问题，找了半天发现是因为自己用了`list1 = list2`这种赋值方法，当给第二个list排序的时候，第一个list也被排序了。
[参考](https://www.cnblogs.com/caoj/p/7868844.html)

首先，在Python中变量之间的拷贝有三种：赋值，浅拷贝，深拷贝。

数据类型可以分为基本数据类型（整型、字符串、布尔...）和基本数据类型所组成（列表、元组、字典）

## copy库

copy库可以提供深浅拷贝的函数如下：
```
import copy
list = [1,2,3]
list1 = copy.copy(list)
list2 = copy.deepcopy(list)

```
在使用copy库的深浅拷贝的时候，list都不会随拷贝过来的发生改变，然而如果直接赋值的话会改变。更加深入的东西参考上面参考的链接。

##