---
title: "如何优雅的使用matplotlib在同一张图上画柱状图和折线图"
key: 2020-11-07-matplotlib
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---


# 如何优雅的使用matplotlib在同一张图上画柱状图和折线图

老师找我画个图，要求在一个坐标系里面既有折线又有柱状图，隐藏条件是要有图例和双纵轴。本来以为挺简单一个事，结果搞了一个多小时，饭还没吃饿死我了。

首先我有以下一个简化的案例，可以单独画一个折线图或者是柱状图。

为了简化叙述这里面采用的数据集就瞎写一些。

```
import matplotlib.pyplot as plt
# 横纵轴数据，数据格式是list
x_data = [1,2,3,4,5]
y_data = [1,2,3,4,5]
# 设置字体
font = {'family':'Helvetica','weight':'normal','size':20}
# 绘制折线图
plt.plot(date, aggregate_report_num_list, color = 'green',linewidth = 1.0)
# 绘制柱状图
# plt.bar(date, address_num)
# 绘制图例
plt.legend(prop={'family':'Helvetica','size':17},loc="upper left")
# y轴文字叙述
plt.ylabel(u'times',font)
# x轴文字叙述
plt.xlabel(u'date',font)
# 设置x轴上需要显示的几个坐标
plt.xticks([1,3,5])
plt.grid(True)
plt.show()
```
[大概是看了这篇](https://www.cnblogs.com/lone5wolf/p/10870200.html)之后，觉得只要再plt.一个就可以实现目标，于是我把代码改成了以下的样子

```
import matplotlib.pyplot as plt
# 横纵轴数据，数据格式是list
x_data = [1,2,3,4,5]
y_data = [1,2,3,4,5]
y_data2 = [6,7,8,9,10]
# 设置字体
font = {'family':'Helvetica','weight':'normal','size':20}
# 绘制折线图
ln1 = plt.plot(x_data, y_data, color = 'green',linewidth = 1.0)
# 绘制柱状图
ln2 = plt.bar(x_data, y_data2)
# 绘制图例
plt.legend(handles=[ln1, ln2], labels=['first', 'second'],  loc='lower right')
# y轴文字叙述
plt.ylabel(u'times',font)
# x轴文字叙述
plt.xlabel(u'date',font)
# 设置x轴上需要显示的几个坐标
plt.xticks([1, 3, 5])
plt.grid(True)
plt.show()
```
然而此时会生成图片，但是会发现图片少了一个图例。我希望图例中有两个对图形的解释但是这里面只有一个，并且报了个warning错误：

```
UserWarning: Legend does not support [<matplotlib.lines.Line2D object at 0x11dc9ca90>] instances.
A proxy artist may be used instead.
See: https://matplotlib.org/users/legend_guide.html#creating-artists-specifically-for-adding-to-the-legend-aka-proxy-artists
  plt.legend(handles=[ln1, ln2], labels=['first', 'second'],  loc='lower right')
```

于是查阅了以下资料[参考这个将两个变量后面加了个逗号](https://blog.csdn.net/qq_38139159/article/details/90607981)，解决了上述问题，然而产生了新的问题，现在连图都没了。报错：

```
    ln2, = plt.bar(x_data, y_data2)
ValueError: too many values to unpack (expected 1)
```
实际上在实际操作里面这时候我就去找别的代码了。但是还是想搞懂这是咋回事。于是我们先看看加逗号是怎么回事。

[从官方文档](https://matplotlib.org/3.1.1/gallery/text_labels_and_annotations/legend_demo.html)的代码注释中发现了蛛丝马迹：`plot returns a list, but we want the handle - thus the comma o the left`

通过[这个stackoverflow上面的回答](https://stackoverflow.com/questions/16742765/matplotlib-2d-line-line-plot-comma-meaning)得到了解释。
在python中，可以通过一个逗号来扩展list中的元素。在matplotlib这个情况中，简而言之就是这个逗号能够：`unpack the value out of the tuple/list into a simple variable`

也就是说我们做的事相当于

```
ln1 = plt.plot(date, aggregate_report_num_list, color = 'green',linewidth = 1.0)[0]
ln2 = plt.bar(date, address_num)[0]
```

把代码改成这个成功了，现在回来看看为什么逗号会报错。

```
ln = plt.plot(x_data, y_data, color = 'green', linewidth = 1.0)
ln1 = ln[0]
ln2 = ln[1]
print(ln,ln1,ln2)
```

然后发现`ln2`打不出来，因为`list index out of range`

ln的内容是`[<matplotlib.lines.Line2D object at 0x119065a60>]` ln1的内容是`Line2D(_line0)`。ln的数据类型是`<class 'matplotlib.lines.Line2D'>`，ln1的数据类型是`<class 'list'>`

然后看看plt.bar

```
ln = plt.bar(x_data, y_data2)
```
打印ln所得是`<BarContainer Object of 5 artists>`，内容分别是

```
Rectangle(xy=(0.6, 0), width=0.8, height=6, angle=0)
Rectangle(xy=(1.6, 0), width=0.8, height=7, angle=0)
Rectangle(xy=(2.6, 0), width=0.8, height=8, angle=0)
Rectangle(xy=(3.6, 0), width=0.8, height=9, angle=0)
Rectangle(xy=(4.6, 0), width=0.8, height=10, angle=0)

```
所以之前所参考的内容都是对折线图而言的，对这个柱状图根本也不需要加逗号，因为不需要"unpack"



## 双坐标轴

[参考](https://blog.csdn.net/Leige_Smart/article/details/79583470?utm_source=blogxgwz6)了这篇之后，我把我的代码改成了

```
import matplotlib.pyplot as plt
# 横纵轴数据，数据格式是list
x_data = [1,2,3,4,5]
y_data = [1,2,3,4,5]
y_data2 = [6,7,8,9,10]
# 设置字体
font = {'family':'Helvetica','weight':'normal','size':20}
fig = plt.figure()
ax1 = fig.add_subplot(111)
# 绘制折线图
ax1.plot(x_data, y_data,label = 'I\'m dataset 1')
ax1.set_ylim([1,5])
ax1.legend(prop = {'family':'Helvatica','size':8})
ax1.set_ylabel(u'I\'m dataset',font)
ax2 = ax1.twinx()
# 绘制柱状图
# legend是根据这里的label来写图例的！！
ax2.bar(x_data, y_data2,label='I\'m dataset 2')
# 绘制图例
ax2.legend(prop = {'family':'Helvatica','size':8})
ax2.set_ylim([4,11])
# y轴文字叙述
ax2.set_ylabel(u'I\'m dataset2',font)
# x轴文字叙述
plt.xlabel(u'date',font)
# 设置x轴上需要显示的几个坐标
plt.xticks([1,3,5])
plt.grid(True)
plt.show()

```
在这个测试用例中没啥问题，但是在实际中，我的数值比较大，数据量也比较大。就出现了一个问题，我的折线没有到达它实际应该到达的数据，中间被截断了一块。但是当我删掉`ax.set_ylim`这个限制，而是让它自己找到数据范围的时候，就不再有这个问题了。我没有找到原因...暂时将它归结为数据太大了，如果看到这篇还知道原因请告诉我🥺