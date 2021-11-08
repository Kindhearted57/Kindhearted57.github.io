---

title: "JS Learning"
key: 2021-08-16-bytecode
pageview: true
tags: python
modify_date: 2021-08-16
sharing: true
show_author_profile: true
show_subscribe: true

---

#JS Notebook

曾经突击学过Javascript，但是过于仓促很多东西都没有好好理解，现在趁着做ethernaut的功夫复习一下javascript，这里面做一些笔记。

1 修改元素

```
document.getElementById("demo").innerHTML = "段落已修改。";
```
用`getElementsByClassName`修改发现改不掉，之后发现这个返回的是一个list，如下方法可以修改。

```
document.getElementsByClassName("intro").innerHTML="test";
x = document.getElementsByClassName("intro");
x[0].innerHTML="test"
```

2 Javascript 对大小写敏感

3 javascript中用分号来结束语句是可选的

4 注释

单行注释 `\\`

多行注释 `/**/`

5 let/const/var

6 向未声明的Javascript变量分配值，该变量自动作为window的一个属性。

`carname = "Volvo";`

将声明window的一个属性carname。



7 关于node.js和javascript的区别，参考廖雪峰的[js教程](https://www.liaoxuefeng.com/wiki/1022910821149312/1023025635916288)

8