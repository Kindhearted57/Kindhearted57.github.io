# 如何优雅的在jekyll md文件中插入html代码

今天遇到了一个问题，一篇blog的post中包含了一段html代码，很久之前写的一篇blog了，连这段代码什么用处都已经忘记了。

```

```

在我开开心心的push上去之后，被github发来的邮件吓了一跳。内容如下

```
The page build failed for the `master` branch with the following error:

The tag `load` on line 424 in `_posts/blog的名字.md` is not a recognized Liquid tag. For more information, see https://docs.github.com/github/working-with-github-pages/troubleshooting-jekyll-build-errors-for-github-pages-sites#unknown-tag-error.

For information on troubleshooting Jekyll see:

 https://docs.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can submit a request on the Contact GitHub page at https://support.github.com/contact?repo_id=255076380&page_build_id=210860342
```

