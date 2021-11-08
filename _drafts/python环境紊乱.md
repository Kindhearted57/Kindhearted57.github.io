

今天发现mac上的python3出了问题，不管什么库都显示

`ModuleNotFoundError: No module named xxx`

首先我们`pip3 install networkx`

路径为`/Library/Python/3.7/site-packages (2.6.2)`

我们看看python3 的路径

`which python3 `

给出的路径比较奇怪

`/opt/local/bin/python3`

从[这个](https://apple.stackexchange.com/questions/270165/macports-switching-back-to-the-system-default-version-of-python)stackexchange问题可以看出，这个路径应该是从macports 安装来的，唤起了我已经遗忘的记忆。

`port select --list python3`

```
Available versions for python3:
	none
	python37 (active)
	python37-apple
	python38-apple
	python39
```

想研究一下这里面的`-apple`是啥意思，并没有搞清楚。尝试了一下在这里面`sudo port select --set python37-apple`，竟然就可以了。。

猜测一下是之前用macports下载了某一个版本的python3，之后却又一通操作python pip的软链接导致的。Macports之类的包管理器方便是属实方便，但是因为过于方便也常常让我们忘记了曾经使用过。

然后我们看看import的路径从哪里来[参考这一篇](https://frostming.com/2019/03-13/where-do-your-packages-go/)


