---
title: "如何优雅的在jekyll md文件中插入html代码"
key: 2020-10-24-how-to-inserthtml-gently
pageview: true
modify_date: 
sharing: true
show_author_profile: true
show_subscribe: true
---
# 如何优雅的在jekyll md文件中插入html代码

今天遇到了一个问题，一篇blog的post中包含了一段html代码，很久之前写的一篇blog了，连这段代码什么用处都已经忘记了。但是由于我的html代码也有`{%balabala%}`所以由于符号转义的问题，会报错。


在我开开心心的push上去之后，被github发来的邮件吓了一跳。内容如下

```
The page build failed for the `master` branch with the following error:

The tag `load` on line 424 in `_posts/blog的名字.md` is not a recognized Liquid tag. For more information, see https://docs.github.com/github/working-with-github-pages/troubleshooting-jekyll-build-errors-for-github-pages-sites#unknown-tag-error.

For information on troubleshooting Jekyll see:

 https://docs.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can submit a request on the Contact GitHub page at https://support.github.com/contact?repo_id=255076380&page_build_id=210860342
```

而报错的部分的代码如下：
{%%}
[参考](https://blog.cotes.info/posts/jekyll-code-snippet/)

## 另外一个插曲
今晚发现了另外一个问题，就是每篇博客中的主题显示都有一些问题，排查了一下发现是`modify_date`的格式对应错了。
