---
title: "Graphsense Legacy"
key: 2020-12-30-graphsense-legacy
tags: BUPT-homework
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---

# Graphsense legacy


因为一直在摸鱼，今天终于打开了许久都没有用过的主机，发生了几件灵异事件。

1） 数据本来是docker中有一份，docker外有一份，docker内的不知道去哪了，docker外的还在。
2）不知道为啥，我的用户名下面的文件(原本应该在/home/jr)都被放在了/media/jr/(一长串长的像序列号的数据)/下面。

在这期间我做过的事情：

关机（因为脚踩到电源线了）

改了jr的密码（因为123456输着不方便）

用过u盘？（或许这个media和u盘有点关系？）


[关于/media 或许有用的信息](https://help.ubuntu.com/community/Partitioning/Home/Moving)

后来我发现桌面上有一个名为4.0TB Volume的东西，这不就是我的HHD吗。仔细一看挂载的位置是

`/media/jr/8303xxxx-xxxxx-xxxxxxxx`

然后我重启了一下。发现它没有挂载。然后一顿操作，它又自动挂载在了这个位置。

综上所述，我分析我遇到的问题是，在装系统挂载完这块盘之后没有设置开机自动挂载，所以在我踢到电源线断电之后就自动挂载在了这个位置。

然而我已经忘了最开始挂载在哪里了 呜呜。


现在需要重新挂载一下这块硬盘。

首先需要备份一下硬盘目前的数据。
[参考](https://askubuntu.com/questions/19901/how-to-make-a-disk-image-and-restore-from-it-later)

（我似乎想起来挂载在哪里了..）
需要设置一下开机自动挂载，以免我再踩到线。

`sudo vim /etc/fstab`

加入
`UUID = 8xxxxx-xxx-xxx-xxx /d1 ext4 defaults 0 1`



当我遇见
`Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`的时候

解决方法

`service docker restart`

然后发现隔个几秒钟就会重复出现上面的问题，最后重启就好了

发现parser搞出来的东西没有addressesDb，实在记不得是从啥时候开始没有的了，于是回到文档，并且寄希望于能够重新parse一遍反正都是覆盖的。就在这时候看到了这一句warning

`warning: Open files limit of 1024 is potentially too low. This could lead to a corruption.`

并且在文档中也有对于`open files limit`的说明。[参考](https://ro-che.info/articles/2017-03-26-increase-open-files-limit)docs里面提供的友情教程。最开始

```
ulimit -Sn
ulimit -Hn
```
返回的分别是

```
1024
4096
```
在把教程上所有能改的都改了之后，返回的结果是

```
20000
20000
```


## docker不断重新重启

在重新同步btc-client的时候，发现docker永远处于restarting的状态

`sudo docker exec bitcoin ps aux`

返回的是

`Error response from daemon: Container xxxxxx(container的编号) is restarting, wait until the container is running`

破案了...稍微有点点复杂。但还是解释一下。穷折腾一通。

为了重新同步btc-client （我觉得graphsense的client还算比较方便，于是从github上面重新clone下了repo，重新build了image。然而通过我的摸查我发现，graphsense/btc-client在过去的几个月里面发生了一个小变化，而最巧的是，这个变化刚刚好让我没法使用这个btc-client来同步。

原本在`docker-compose.yml`中的

```
service:
    bitcoin-client:
        command: ["bitcoind", "-conf=/opt/graphsense/bitcoin.conf", "-datadir=/opt/graphsense/data", "-rest"]
```

被挪到了`Dockerfile`中，成为了

```
CMD ["bitcoind", "-conf=/opt/graphsense/bitcoin.conf", "-datadir=/opt/graphsense/data", "-rest"]
```
然而不管是哪个版本，对于restart这件事都是如此安排：`restart: always`

然后我几个月前第一次build这个image的时候，就生成了一个名为bitcoin的image，这个image里面没有带有CMD的命令。然而后一版本的docker-compose.yml也没有提及这个CMD的问题，于是这整个docker的启动就没有带命令。本来restart设置成always的目的是随时同步数据，但是现在这样就变成了原地反复空翻。

虽然后来重新build了一个带CMD的新image，但是也无济于事，这个新image(通过ID分辨)并没有被列入到`docker image ls`中，虽然successfully built，不想探索为啥了。后面我想重新同步的时候，又启动了几个月前的bitcoin image./


## config.json

小丑竟是我自己，花了不少时间同步的bitcoin，又花了更多的不少时间用blocksci解析，结果在我想重新解析一下的时候，翻了一个宇宙无敌错误，把待解析和解析后的目标路径整反了，于是现在两边的文件格式十分混乱。我来捋捋

### Bitcoin core

| banlist.dat

| bitcoind.pid

| debug.log

| peers.dat

| blocks

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;| blkxxx.dat 存储source data


&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;| revxxx.dat 包含所有由blocks的input花费的UTXO(undo data:在blk.dat中, blocks是aggregated data, 而在rev.dat中包含了所有data，方便回滚)
                                 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;| 4 bytes: network magic(0xf9, 0xbe, 0xb4)
			   
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;| 4 bytes: size of the CBlockUndo record(LE32)

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;| data_size bytes: CBlockUndo record (consists of a serialized vector of CTxUndo records 

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;| 32 bytes: double-SHA256 of the serialized CBlockUndo record


| chainstate(LevelDB database)compact representation of all currently unspent transaction outputs and some metadata about the transactions they are from. 用于验证新进区块使用

| wallets

| indexes


### Blocksci data
| addressesDb

| btc.cfg

| chain

| hashIndex

| mempool

| parser

| scripts


这次直接从bitcoin core客户端重新同步，可能如果一开始就这样就好了，可以省去很多学习docker的成本。如果什么时候真的需要学习docker的话，可以从头开始的。