---
title: "一件没有什么意义但是有点意思的Python小事"
key: 2021-6-1-pythonbalabala
pageview: true
tags: Python
modify_date: 2021-06-01
sharing: true
show_author_profile: true
show_subscribe: true
---

# 一件没有什么意义但是有点意思的Python小事

六一快乐。

一个list，想边遍历边删除，类似于这样
```
list1 = [1,2,3,4,5,6,7,8]
for i in range(len(list1)):
	del list1[i]
	print(i)
```
惊讶的发现输出的结果是

```
0
1
2
3
```

这时候list的内容是

`[2,4,6,8]`

`IndexError: list assignment index out of range`

hhhhh读到这里就浪费了1分钟的阅读时间和半分钟的脑力思考。

六一快乐。