---

layout: post
title: 如何优雅的使用Googletrans
key: 2021-03-21-googletrans
tags: python
pageview: true
modify_date: 2020-11-22
sharing: true
show_author_profile: true
show_subscribe: true

---


# Python | 如何优雅的使用Googletrans

## Background

有万条文本，使用langid检测语言类型，然后使用googletrans翻译成英文。

## How googletrans works?
在googletrans的gtoken.py文件前面有一段注释：

```
Google Translate API token generator

translate.google.com uses a token to authorize the requests. If you are
not Google, you do have this token and will have to pay for use.
This class is the result of reverse engineering on the obfuscated and
minified code used by Google to generate such token.

The token is based on a seed which is updated once per hour and on the
text that will be translated.
Both are combined - by some strange math - in order to generate a final
token (e.g. 744915.856682) which is used by the API to validate the
request.

This operation will cause an additional request to get an initial
token from translate.google.com.

Example usage:
    >>> from googletrans.gtoken import TokenAcquirer
    >>> acquirer = TokenAcquirer()
    >>> text = 'test'
    >>> tk = acquirer.do(text)
    >>> tk
    950629.577246
```

大致解释了Googletrans的工作原理，并且在对应的[github page](https://github.com/ssut/py-googletrans)上面可以找到一些"注意事项"。比如:

```
Use this library if you don't care about stability

If you get HTTP 5xx error or erros like #6, it's probably because Google has banned your client IP address
```

## 问题
### Pt.1
```
code = unicode(self.RE_TKK.search(r.text).group(1)).replace('var ', '')

AttributeError: 'NoneType' object has no attribute 'group'
```

[参考](https://stackoverflow.com/questions/52455774/googletrans-stopped-working-with-error-nonetype-object-has-no-attribute-group)stackoverflow上面的回答挨个尝试。(然后并没有解决这个问题)我认为stackoverflow上面的解决方法是在以前的googletrans版本中出现的问题，在旧版本中会对任何文本都出现Nonetype的问题。


而在我的情况中是我的文本比较多，在循环里面会出现某个连接不稳定这样的问题，因为文本数量比较多，并且本来langid和googletrans也不是很稳定，所以决定放弃这些连接不好的文本。用try-except处理。

```
if language != 'en':
    try:
        info = translator.translate(info, dest='en')
        info = info.text
    except:
        cnt_error = cnt_error + 1
```

### Pt.2

在之前考虑过是不是翻墙的问题。尝试在命令行中使用proxychains4。
[参考](https://medium.com/@xiaoqinglin2018/mac-osx-使用proxychains-ng-91ba61472fdf)

也可以在[.zshrc中设置](https://zhuanlan.zhihu.com/p/47849525)

但是节点换来换去也会403 forbidden 不知道什么原因。


[参考](https://blog.csdn.net/ssjdoudou/article/details/84261341)发现我之前有一点问题，一共有三个文件需要把`translate.google.com`改成`translate.google.cn`。