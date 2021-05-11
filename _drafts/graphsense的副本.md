# Background
## Prequisition
‼️截止7月26日，你至少需要300GB的硬盘和60G的RAM来跑graphsense。配置十分重要。

查看硬盘大小：`df -h`

查看内存大小：`free -h`

查看用户对相应目录的权限： `ls -l filename `


### My background

花了大量的时间弄这个东西，首先没有接触过类似的图数据库，完全不知道这是啥，最开始在自己的虚拟机上尝试Ubuntu18.04，四处碰壁，并且花费了许多时间等待⌛️，在某一环节发现配置严重不足导致无法编译。后来借用了一个还可以的服务器，一切就都变的容易得多了。但是中间服务器本身出了一些问题，所以大家对服务器进行了大规模的休整，最后重新装了Ubuntu20.04的系统，在服务器修改好之后的一段时间内还没有给root权限，是后来才有了root权限，所以中间还花了一些时间探索如何在没有root权限的情况下使用服务器。
### What is this
阅读graphsense简洁的[docs](https://graphsense.info/documentation.html)，可以看出graphsense是由几个部分组成的。其中每一个部分都被打包📦成为一个docker🚢。

* btc-client/bch-client/ltc-client...
针对bitcoin来说，其实就是bitcoin core，一个全节点，将全节点的data同步到本地之后供后续使用。

* graphsense-blocksci
将BlockSci数据连接到Apache Cassandra，BlockSci用来解析row btc数据的格式。

* graphsense-tagpack 

### Apache Spark
[完全参考](https://phoenixnap.com/kb/install-spark-on-ubuntu)
其中spark的[版本](https://www.apache.org/dyn/closer.lua/spark/spark-2.4.6/spark-2.4.6-bin-hadoop2.7.tgz)目前应该是spark-2.4.6
### Cassandra
[一切以官网为准](https://cassandra.apache.org/download/)

首先输入命令`echo "deb https://downloads.apache.org/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list`

或者将`deb https://downloads.apache.org/cassandra/debian 311x main` 添加到`/etc/apt/sources.list.d/cassandra.sources.list`

然后添加密钥`curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -`

更新apt`sudo apt-get update`

如果遇到错误：`GPG error: http://www.apache.org 311x InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY A278B781FE4B2BDA`

添加命令`sudo apt-key adv --keyserver pool.sks-keyservers.net --recv-key A278B781FE4B2BDA`

安装`sudo apt-get install cassandra`

启动cassandra的方法`sudo service cassandra start`

关闭cassandra的方法`sudo service cassandra stop`

查看链接状态`nodetool status`

这里报错

`nodetool: Failed to connect to '127.0.0.1:7199' - ConnectException: 'Connection refused (Connection refused)'.`

可能是因为java的版本过高，学到了一个方法快速切换java版本`sudo update-alternatives --config java`

修改/etc/cassandra下面的cassandra-env.sh文件即可，取消注释
`JVM_OPTS="$JVM_OPTS -Djava.rmi.server.hostname=127.0.0.1/7199`

官方的方法中有指出：要在cassandra关闭的情况下再修改配置文件，中间因为没有关掉cassandra就修改花了很多时间，最后还是重启的。
![](https://note.youdao.com/yws/api/personal/file/WEB26cdff2464aad880f7e511ab82c1a5d0?method=download&shareKey=a50e909e63c8f4e724ec0a986ae9f51b)


No module name cassandra，所以看来是pip3 install cassandra-driver

```

```

```
docker inspect [docker-name]
```
## Running Ubuntu on mobile hard disk
VMware Ubuntu18.04。最开始以为只要有.vmx文件放在移动硬盘里面就可以了，由于硬盘是NTFS格式，在MAC上面使用的mounty挂载，后来发现用mounty挂载就只能在finder中打开，而不是把移动硬盘当成一个独立的存储空间了。

![mobile hard disk](https://note.youdao.com/yws/api/personal/file/WEB58ac6a9022c4b6f72e401b424b18111f?method=download&shareKey=f604c652bfe89734f61f0e680deebd41)

于是直接把硬盘格式化成ExFAT，这样之后就会出现这个磁盘空间了。

![exfat](https://note.youdao.com/yws/api/personal/file/WEB69fff34a75c8cb7e309260fc94b1b235?method=download&shareKey=b4a85e82cd8771aa088224a55d114b75)
## Linux安装docker-compose

```
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

# BTC-client
## 首先要创建用户
在本地系统里面创建一个用户`dockeruser`，uid为10000，如果没有这个用户的话，`docker-compose logs`的时候会报错。

创建用户用到的命令：

`adduser`可以自动为创建的用户指定主目录，系统shell版本，可以在创建的时候输入用户密码。

`useradd`需要使用参数选项指定基本设置，如果不使用任何参数，那么创建的用户无密码、无主目录、没有指定的shell版本。

我们这里需要添加uid为10000的用户，至于用户名和gid其实不会产生多大的影响，但是为了上下文统一这里还是写成一致的样子。

创建指定uid的用户所用的命令：
`useradd dockeruser -u 10000`

关于docker容器内和容器外的[用户权限](https://cloud.tencent.com/developer/article/1480017)问题

总结下来也就是和用户名的关系是只要uid保持一致即可，但是我在docker之外创建了一个dockeruser，uid:gid为10000:10000，但是仍然会报着个错误。用`docker-compose build --no-cache`重新`build`了之后仍然如此。


```
bitcoin           | ************************
bitcoin           | EXCEPTION: N5boost10filesystem16filesystem_errorE       
bitcoin           | boost::filesystem::create_directory: Permission denied: "/opt/graphsense/data/blocks"       
bitcoin           | bitcoin in AppInit()       
```

[当出现](https://docs.docker.com/engine/install/linux-postinstall/)

```
WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
```
的时候，解决方法：

```
$ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
$ sudo chmod g+rwx "$HOME/.docker" -R
```

两天过去了，期间尝试了许多种方法，看了无数的帖子，一开始的思路就是权限没给够，需要给权限：

* 在实际操作中使用另外一个用户进行docker-compose up->是否是此原因，应该用dockeruser进行build和up才对？（build所需的时间比较多，而最后我也没有尝试）然而如果如此的话，那还是根据用户名而不是uid进行的docker内外用户相连。 ❌
* dockeruser的用户分组是不是应该放到docker这一组里才可以？但这又跟docker和host主机通过uid,gid进行交互相悖，如果相悖的话是否group并没有起到影响作用。❌
* [这个issue](https://github.com/bitcoin/bitcoin/issues/432)中认为


后来在[这个帖子](https://www.reddit.com/r/litecoin/comments/86ysrk/problem_using_litecoincli/)发现了蛛丝马迹，说

```
for some reason it was trying to use a different data directory when running it in the web browser.
```

然后我又在
[bitcoin的wiki页](https://en.bitcoin.it/wiki/Data_directory)发现linux的默认data目录是

```
~/.bitcoin/
```
最开始最开始在我翻GitHub上相关的issue的时候就看到了bitcoin维护者的[一个回答](https://github.com/bitcoin/bitcoin/issues/11373)：

```
This is a permissions issue, as is implied by the error message boost::filesystem::create_directory: Permission denied: .....
```

所以我的思路就转变到：这就是一个简单的权限问题。只要找到对应的文件夹给够权限就可以了。既然在dockerfile里面已经给了/opt/graphsense/data权限，就说明实际需要权限的不是这个文件夹。
兜兜转转在docker-compose.yml中找到了如下：

```
volumes:
  - ./data/:/opt/graphsense/data
```

虽然还不知道具体的作用，但是这个看起来像是一个地址的映射。在docker-compose的[docs](https://docs.docker.com/compose/compose-file/#volume-configuration-reference)里面找到。

总而言之volume是为了docker数据的持久化，这让我想到了另外一个我探索了的问题，我是通过ssh连接到服务器，然后运行了docker，（以下是猜测）这样的话当ssh断连的时候，docker-compose up的进程还在服务器运行，也就是如果这时候docker-compose ps的话还是会显示正在运行的docker程序，因为毕竟我是连到服务器，就像我用ssh连到服务器打开一个notepad，ssh断开之后，notepad还是打开的状态。然而在搜索资料的时候发现，docker是随着terminal的[存在而存在的](https://blog.csdn.net/qq_34464926/article/details/84982561)。

[volume](https://deepzz.com/post/docker-compose-file.html)的映射就是主机的地址到docker中地址的映射。docker持久化的意义就是使数据不随着cotainer的结束而消失，数据存放在host主机上。Docker的数据持久化主要有：[三种方式](https://michaelyou.github.io/2017/09/17/Docker%E6%95%B0%E6%8D%AE%E7%AE%A1%E7%90%86-Volume%EF%BC%8C-bind-mount%E5%92%8Ctmpfs-mount/)

![](https://note.youdao.com/yws/api/personal/file/WEBe392735e34341f0ff6a6f57030dbccbc?method=download&shareKey=277914efc8023cfe563b4e4f7b0c7bf5)

* bind mount

数据可以存放在宿主机的任何地方，可以是重要的系统文件或目录，非docker应用程序可以改变这些数据。

* tmpfs mounts

数据只存储在宿主机的内存中，不会写到宿主机的文件系统。

* volume

由docker管理，存储在宿主机的某个地方，非docker应用程序不能改动这一位置的数据。

所以我尝试着给了./data/权限
`chown -R dockeruser opt/graphsense/....放着btc-client的地址/`

成功了！

### 开始同步
按照github上面的[指示](https://github.com/graphsense/btc-client)即可。

###其他的一些参考到的资料

[软链接硬链接](https://www.ibm.com/developerworks/cn/linux/l-cn-hardandsymb-links/)

[umask的概念](https://langzi989.github.io/2017/09/13/Linux中umash深入理解/)


###经验
![遇见问题](https://note.youdao.com/yws/api/personal/file/WEB18fece5ed7fd9945bdfc27f2041a3cb6?method=download&shareKey=c9d0d5ed0884a2f387d98db8e1c2c2a9)
alpine这个包一直报错temporary error(try again later)。在网上查到了很多的解决方法，比如在/etc/docker下面放一个deamon.json用来表示DNS的关系，重启docker，启动docker的时候加上--network host。后来想到，我在用的是虚拟机，使用的是NAT网络，上面的问题都在说跟网络有关，于是我就换成了桥接模式，问题解决。
[关于三种网络模式的介绍](https://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646007.html)
后来又报了这个错误
![](https://note.youdao.com/yws/api/personal/file/WEB9e7e9d2bf378f4ba637028adbe9cccce?method=download&shareKey=0f7865926e9895b8aebb9c4a70d074e8)
稍微分析了一下，这个错误应该是gitclone断开了，本来bitcoin core就贼大，不断才怪，所以把这个直接从github的page上面下载下来，这个地方改成mv试试，然而还是在报错
![](https://note.youdao.com/yws/api/personal/file/WEB156500b78b0927d5bb4b4fcd2c987fb6?method=download&shareKey=8ccbfb2f9690c94e7fd51b9a0ad53a8e)
后来想到。git clone命令和直接下载的不一样的，git下载的会多.git这个文件夹，之前知道git下载的是包含分支目录的，没想到是以这种形式包含的。而我在mac上面找了一个repo测试了一下并没有看到git下来多的内容，原来是在finder/ls中没有显示，通过以下命令显示：

`defaults write com.apple.finder AppleShowAllFiles TRUE`
重启Finder应用
`killall Finder`

然而gitclone总会断开，download zip又少东西，在stackoverflows上面找到了[解决方法](https://stackoverflow.com/questions/15681643/how-to-clone-git-repository-from-its-zip)[一篇参考](https://www.worldhello.net/gotgit/02-git-solo/100-git-clone.html)。

然而我所需要的repo即使用如上方法也是很慢，经常断。在dockerfile里面看到git clone下来一个repo之后，又切换到相应版本的分支，所以我直接就`git clone -b 版本号 https://github.com/user/repo_name`。

log message 

```
bitcoin           | 2020-07-25T13:21:18Z tor: Thread interrupt
bitcoin           | 2020-07-25T13:21:18Z Shutdown: In progress...
bitcoin           | 2020-07-25T13:21:18Z opencon thread exit
bitcoin           | 2020-07-25T13:21:18Z torcontrol thread exit
bitcoin           | 2020-07-25T13:21:18Z addcon thread exit
bitcoin           | 2020-07-25T13:21:18Z msghand thread exit
bitcoin           | 2020-07-25T13:21:18Z net thread exit
bitcoin           | 2020-07-25T13:21:18Z scheduler thread exit
bitcoin           | 2020-07-25T13:21:19Z Dumped mempool: 0.019646s to copy, 0.365033s to dump
bitcoin           | 2020-07-25T13:21:19Z FlushStateToDisk: write coins cache to disk (2884329 coins, 440613kB) started
bitcoin           | 2020-07-25T13:21:27Z FlushStateToDisk: write coins cache to disk (2884329 coins, 440613kB) completed (8.68s)
bitcoin           | 2020-07-25T13:21:27Z FlushStateToDisk: write coins cache to disk (0 coins, 70758kB) started
bitcoin           | 2020-07-25T13:21:27Z FlushStateToDisk: write coins cache to disk (0 coins, 70758kB) completed (0.06s)
bitcoin           | 2020-07-25T13:21:34Z Ignoring unknown configuration value rpcclienttimeout
bitcoin           | 2020-07-25T13:21:34Z Bitcoin Core version v0.20.0 (release build)
bitcoin           | 2020-07-25T13:21:34Z Assuming ancestors of block 0000000000000000000f2adce67e49b0b6bdeb9de8b7c3d7e93b21e7fc1e819d have valid signatures.
bitcoin           | 2020-07-25T13:21:34Z Setting nMinimumChainWork=00000000000000000000000000000000000000000e1ab5ec9348e9f4b8eb8154
bitcoin           | 2020-07-25T13:21:34Z Using the 'sse4(1way),sse41(4way),avx2(8way)' SHA256 implementation
bitcoin           | 2020-07-25T13:21:34Z Using RdSeed as additional entropy source
bitcoin           | 2020-07-25T13:21:34Z Using RdRand as an additional entropy source
bitcoin           | 2020-07-25T13:21:35Z Default data directory /home/dockeruser/.bitcoin
bitcoin           | 2020-07-25T13:21:35Z Using data directory /opt/graphsense/data
bitcoin           | 2020-07-25T13:21:35Z Config file: /opt/graphsense/bitcoin.conf
bitcoin           | 2020-07-25T13:21:35Z Config file arg: rpcport="8332"
bitcoin           | 2020-07-25T13:21:35Z Config file arg: server="1"
bitcoin           | 2020-07-25T13:21:35Z Config file arg: txindex="1"
bitcoin           | 2020-07-25T13:21:35Z Command-line arg: conf="/opt/graphsense/bitcoin.conf"
bitcoin           | 2020-07-25T13:21:35Z Command-line arg: datadir="/opt/graphsense/data"
bitcoin           | 2020-07-25T13:21:35Z Command-line arg: rest=""
bitcoin           | 2020-07-25T13:21:35Z Using at most 125 automatic connections (1048576 file descriptors available)
bitcoin           | 2020-07-25T13:21:35Z Using 16 MiB out of 32/2 requested for signature cache, able to store 524288 elements
bitcoin           | 2020-07-25T13:21:35Z Using 16 MiB out of 32/2 requested for script execution cache, able to store 524288 elements
bitcoin           | 2020-07-25T13:21:35Z Script verification uses 11 additional threads
bitcoin           | 2020-07-25T13:21:35Z scheduler thread start
bitcoin           | 2020-07-25T13:21:35Z No wallet support compiled in!
bitcoin           | 2020-07-25T13:21:35Z libevent: getaddrinfo: nodename nor servname provided, or not known
bitcoin           | 2020-07-25T13:21:35Z Binding RPC on address ::1 port 8332 failed.
bitcoin           | 2020-07-25T13:21:35Z HTTP: creating work queue of depth 16
bitcoin           | 2020-07-25T13:21:35Z No rpcpassword set - using random cookie authentication.
bitcoin           | 2020-07-25T13:21:35Z Generated RPC authentication cookie /opt/graphsense/data/.cookie
bitcoin           | 2020-07-25T13:21:35Z HTTP: starting 4 worker threads
bitcoin           | 2020-07-25T13:21:35Z init message: Loading banlist...
bitcoin           | 2020-07-25T13:21:35Z Using /16 prefix for IP bucketing
bitcoin           | 2020-07-25T13:21:35Z Cache configuration:
bitcoin           | 2020-07-25T13:21:35Z * Using 2.0 MiB for block index database
bitcoin           | 2020-07-25T13:21:35Z * Using 56.0 MiB for transaction index database
bitcoin           | 2020-07-25T13:21:35Z * Using 8.0 MiB for chain state database
bitcoin           | 2020-07-25T13:21:35Z * Using 384.0 MiB for in-memory UTXO set (plus up to 286.1 MiB of unused mempool space)
bitcoin           | 2020-07-25T13:21:35Z init message: Loading block index...
bitcoin           | 2020-07-25T13:21:35Z Opening LevelDB in /opt/graphsense/data/blocks/index
bitcoin           | 2020-07-25T13:21:35Z Opened LevelDB successfully
bitcoin           | 2020-07-25T13:21:35Z Using obfuscation key for /opt/graphsense/data/blocks/index: 0000000000000000

```


### 在这个部分中常见的一些命令
查看用户

`cat /etc/passwd`

查看用户组
`cat /etc/group`


## graphsense_blocksci

### 问题
#### 将宿主机中的文件复制到docker里
在blocksci中遇到的第一个问题，就是如何把宿主机中的文件复制到docker里，这个时候发现build docker中的 `.`代表的是根目录
![](https://note.youdao.com/yws/api/personal/file/WEBcd983c9b54d7e8f878135b18807b85bc?method=download&shareKey=d6612433099c63633e3b29b78c10c9cc)


#### git速度太慢
这部分可以直接跳过不看，毕竟主要还是因为一开始头铁在本地尝试搭graphsense造成的。

下面遇到的第二个问题是因为代码里`git submodule update --recursive`[引发](https://git-scm.com/book/zh/v2/Git-工具-子模块)的。首先会`retry scheduled`然后就会`failed to clone a second time, aborting`。这个问题深究下去就是git clone速度太慢了，不一定什么时间就断开了。
![](https://note.youdao.com/yws/api/personal/file/WEBb48a63b6f400d4382a7b4c23200c9aa8?method=download&shareKey=23e6be72c49a3d9acb351425a8ea1974)

下面尝试在ubuntu里面加代理，看能不能够提升速度和稳定性。如果可以的话就可以直接`docker build...`来运行。

目前我的情况是主机Mac，虚拟机Ubuntu18.04，主机ss的代理，目的是在ubuntu里面设置代理。网上的方法首先有用一些小工具，比如proxychains，然而经过一番尝试不太行，所以使用了一些别的方法。

[参考1](https://www.xiebruce.top/796.html)[参考2](https://liushiming.cn/article/mac-virtual-machine-proxy-through-host-machine-shadowsocks.html)
首先先ifconfig以下拿到主机的ip地址，这里面写成ip1，然后在ss的偏好设置中设置，socks5和HTTP的监听地址都写0.0.0.0，表示代理所有ip，而不只是127.0.0.1（本机）

![](https://note.youdao.com/yws/api/personal/file/WEBc60959e364b9342ce16aa85f1cf02aa4?method=download&shareKey=36a1d347bd80e099e1d65d8fa824cb22)
![](https://note.youdao.com/yws/api/personal/file/WEB47f752407486caa4b42a857be28b5c50?method=download&shareKey=abf9614015388b40c7512bb0375d606d)

以下方式都进行了设置，所以不知道最后是哪一种设置成功的。

* 如果想要在虚拟机中访问一些外网网页的话，可以在setting中进行设置，setting -> network -> network proxy
* 有一部分博主认为git clone特别慢是因为github.global.ssl.fastly.net的域名被限制了，所以找到这个域名对应的ip地址，然后在hosts文件中加上ip->域名的映射即可。[参考](https://www.linuxidc.com/Linux/2019-05/158461.htm)
`nslookup github.global.ssl.fastly.Net`

`Non-authoritative answer`下面拿到address1。

`nslookup github.com`

`Non-authoritative answer`下面拿到address2。

`sudo vim /etc/hosts`在文件末尾添加两行

```
address1 github.global-ssl.fastly.net
address2 github.com
```
* 还有一些在~/.bashrc中配置
![](https://note.youdao.com/yws/api/personal/file/WEB1024f735d9b4641e73744ea9fbde4123?method=download&shareKey=b2b190f02fcfc68b807b4919e5cd8f51)

遇到问题，这个问题出现的原因应该是虚拟机和主机的链接问题
![](https://note.youdao.com/yws/api/personal/file/WEBa7fff6b8425405aea377a0c2d17e092c?method=download&shareKey=2a8301d0a639ed44b4ce2354e1e9cc98)
这个问题一直没有解决，后来试了一下`sudo /etc/init.d/networking restart`重启网络配置，有效。
![](https://note.youdao.com/yws/api/personal/file/WEB563c4508b11b5e423b34e07cea5ab73c?method=download&shareKey=b9189df98100ce46ff05eb4503559806)

看到这个git clone的速度快流泪了...快了大概十倍
![](https://note.youdao.com/yws/api/personal/file/WEB48bad262d50fe1bc11c39112573af144?method=download&shareKey=735014a81b837fda26c05dad8e0e65a3)
graphsense-blocksci的文件结构，其中citp是我后面加的，用来放blocksci的原始项目代码，在本来的graphsense中的代码如下
![](https://note.youdao.com/yws/api/personal/file/WEB3bac395c5208541db19832f114315a25?method=download&shareKey=e049fcf525086dc1a2bcfb608e03ee99)

```
# add BlockSci
RUN cd /opt && \
  git clone https://github.com/citp/BlockSci.git && \
  cd BlockSci && \
  git checkout "v0.6" && \
  git submodule init && \
  git submodule update --recursive
```

因为git出问题所以修改如下：

```
# add BlockSci
COPY  citp/BlockSci /opt/BlockSci
RUN  cd /opt/BlockSci && git submodule init && git submodule update --recursive
```
其中BlockSci项目是git clone下来的，直接下载了v0.6的分支。
`git submodule update`出现问题，还是git的原因，并且由于有太多库了所以总会出差错，于是我就人工一个个下载然后丢在`citp/BlockSci`相应的文件夹下，具体文件怎么放置要参考`.gitmodules`文件，我认为这可以和`git submodule update`起到一样的作用，这个文件夹会被拷贝到`/opt/BlockSci`
![](https://note.youdao.com/yws/api/personal/file/WEB90eedd8c31fd54a89cec54b793a756e8?method=download&shareKey=873669dc92223b9bcf92367cfc932dee)

[git submodule](https://bitmingw.com/2018/08/19/git-submodule-tutorial/)
然而在`pip install -e blockscipy`的时候又出了问题
![](https://note.youdao.com/yws/api/personal/file/WEBaeea24d2c556e8fb0d3d5b36b49f4bd5?method=download&shareKey=1e2a799152b330cfa43b48aa7c9c1e07)

检查发现range-v3这个库的gh-pages是空的，我想可能是因为在后面的步骤里面检查发现这个是空的，然后因为某些问题直接把range-v3下载到了gh-pages中，于是我挪了一下gh-pages之后重新`docker build`


今天打开电脑下载速度又慢了十倍，流泪，未知原因，待我侦查一番。ss重新启动了一下就好了，看来是玄学问题。

#### 内存不足

![](https://note.youdao.com/yws/api/personal/file/WEB9961f5c0ad959de5cf598193e6b64f8e?method=download&shareKey=7ae97871c70aff1fb4b74c2a532d966b)
出现了新问题，[参考](https://stackoverflow.com/questions/30887143/make-j-8-g-internal-compiler-error-killed-program-cc1plus)首先测试`dmesg`，发现原因是内存不足，[参考](https://blog.csdn.net/u011897411/article/details/89742008)解决方法是临时使用交换分区。

![](https://note.youdao.com/yws/api/personal/file/WEB1899f877d528a4245d839fd2594a1aea?method=download&shareKey=d4492a7ecc84af03aa36cacfece59b76)

![](https://note.youdao.com/yws/api/personal/file/WEBa5c5f08d411796ee4d928b1f2bb01e40?method=download&shareKey=be0788b06b722bd3cacdfec99f0fb5a2)
首先创建分区

```
sudo dd if=/dev/zero of=/swapfile bs=1M count=1024
sudo mkswap /swapfile
sudo swapon /swapfile
```

运行第一步的时候出现问题
`dd: failed to open '/swapfile': Text file busy`
解决方法
`sudo swapoff -a`

查看内存使用

`free -m`

注意编译之后要关闭交换分区

```
sudo swapoff /swapfile
sudo rm /swapfile
```

还是不够用 ram开到6G试试，这时候打开显示找不到可以连接的有效对等进程。最后在系统偏好设置->安全性与隐私中解决的。

首先build docker image
`docker build -t blocksci .`

创建user-defined bridge network
`docker network create graphsense-net`

启动docker容器
`./docker/start.sh blocksci_btc /home/why/graphsense/btc-client/data/ /home/why/graphsense/graphsense-blocksci/data /home/why/graphsense/graphsense-blocksci/scripts`

最开始在`BLOCKCHAIN_DATA_DIR`这个位置我写的是`/home/why/graphsense/btc-client/data/blocks`
在docker容器里面发现`docker_parser`创建config文件失败，没有报错，但是config文件并没有创建出来。从[这里](https://github.com/citp/BlockSci/issues/342)看到可能是因为空间不足造成的。突然想到btc-client那边好久没看了，去看了一下果然，之前图方便给放在/opt文件夹下面了，但是/opt下面的空间不够用。于是给放回原位。

在解决了存储空间大小的问题之后还是出现无法创建config的问题，提交了一个[issue](https://github.com/citp/BlockSci/issues/424)，发现是文件夹的权限问题（但是竟然没有返回报错555）

`sudo chown -R dockeruser /home/graphsense`

```
fully synced Bitcoin node
```

要怎么理解呢
[blk.dat](ChainDiskConfiguration)
总看到说btc同步需要几天时间，然而我的情况下btc运作了几十分钟之后就开始循环，从2008-2020，这个还不知道是什么原因造成的。

![](https://note.youdao.com/yws/api/personal/file/WEB0e231d7d46b9a4a291630665b2d0b44f?method=download&shareKey=5f2e333731cc9b030b41358ba2486a28)

#### 将程序放在后台运行
最开始没有意识到将程序放到后台的重要性，直接头铁放前台运行。结果运行了一个下午之后ssh断开了。
```
A PID file exists in the data directory, another parser instance might already be running. Aborting.
```

删除了PID文件之后

```
terminate called after throwing an instance of 'std::runtime_error'
  what():  Could not open hash index with error: While lock file: /var/data/blocksci_data/hashIndex/LOCK: Resource temporarily unavailable
Aborted (core dumped)

```

输入`jobs`没有反应，只有在`top`中能看到进程，于是如何将这个sleep的进程唤醒，实在是没整明白，在[askubuntu](https://askubuntu.com/questions/1262580/how-to-wake-up-a-sleeping-process)提了个问题，但是还没有得到回答...但是大量浏览了一些资料之后发现可能根本也没办法唤醒这个进程。顶多只能kill掉。

```
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND    
   31 dockeru+  20   0 15.969g 8.911g 3.111g S  22.7 14.2  31:51.12 blocksci_p+
    1 dockeru+  20   0   18508    292      0 S   0.0  0.0   0:00.30 bash       
   16 dockeru+  20   0   18500    460      8 S   0.0  0.0   0:00.29 bash       
 2111 dockeru+  20   0   18628   3540   3040 S   0.0  0.0   0:00.37 bash       
 2332 dockeru+  20   0   36600   3116   2680 R   0.0  0.0   0:00.00 top   
```

PID文件和lock文件的[介绍](https://unix.stackexchange.com/questions/12815/what-are-pid-and-lock-files-for)
PID文件重新开始的方法

因为blocksci_parser所需要的时间太长了，ssh总是断开，所以需要找一个办法在ssh断开的时候能够继续上次的运行，所以我们需要看看整个运行的恢复过程。

首先docker是以attach的形式运行的：

```
docker exec -ti blocksci_btc /bin/bash
```


在ssh重新连接之后，docker ps 显示：

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                            NAMES
930de9581877        blocksci            "/bin/bash"         34 hours ago        Up 8 hours                                           blocksci_btc
dabdcc53d6a7        cc                  "/bin/bash"         3 weeks ago         Up 27 hours         3000/tcp, 0.0.0.0:8022->22/tcp   distracted_perlman

```

如此看来，ssh连接的断开并没有使docker断开，使用[-it](https://blog.csdn.net/halcyonbaby/article/details/46884605)的时候，和平时操作的console界面类似，也不会像attach方式因为退出导致整个容器退出。（这里面稍微有些误导的是有个attach.sh的文件，但是其实并不是attach类型）

查看`/var/data/parser`的大小，发现随时间在不断增大，说明`blocksci_parser update`还是在不断运行。

从redhat[关于proocess的一份文档](https://access.redhat.com/sites/default/files/attachments/processstates_20120831.pdfs)中知道，可以使用命令`ps -el`来找到系统上的所有进程。

```
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S 10000     1     0  0  80   0 -  4627 poll_s pts/0    00:00:00 bash
4 S 10000    10     0  0  80   0 -  4627 wait   pts/1    00:00:00 bash
0 S 10000    26    10 18  80   0 - 13025074 futex_ pts/1 04:46:03 blocksci_parser
4 S 10000   394     0  0  80   0 -  4627 poll_s pts/2    00:00:00 bash
4 S 10000   406     0  0  80   0 -  4626 wait   pts/3    00:00:00 bash
0 S 10000   417   406  0  80   0 - 18513 ep_pol pts/3    00:00:00 vim
4 S 10000   505     0  0  80   0 -  4626 poll_s pts/4    00:00:00 bash
4 S 10000   532     0  0  80   0 -  4627 wait   pts/5    00:00:00 bash
0 R 10000   551   532  0  80   0 -  6487 -      pts/5    00:00:00 ps

```

`WCHAN`是`wait channel`，可以看到`blocksci_parser`在等待的资源是[futex](https://zh.wikipedia.org/wiki/Futex)

尝试方法[reptyr](https://github.com/nelhage/reptyr)不太行，报错：

```
dockeruser@softsec-9608:~$ sudo reptyr 16767
[sudo] password for dockeruser: 
[-] Unable to open the tty in the child.
Unable to attach to pid 16767: No such file or directory
```

#### tmux改变生活
[参考1](http://louiszhai.github.io/2017/09/30/tmux/)
[参考2](https://www.jianshu.com/p/fd3bbdba9dc9)
因为电脑总是卡住(...)，所以使用两个插件`Tmux Resurrect`和`Tmux Continuum`来保证开机之后还能恢复。


首先Tmux Resurrec：

在mac上brew install下载的，下载到的位置是/usr/local/Cellar/tmux
```
cd ~/.tmux
mkdir plugins
git clone https://github.com/tmux-plugins/tmux-resurrect.git
```

Ctrl+B的prefix总是没有反应，一直以为是电脑上的默认设置有问题，直到看到这个[thread](https://superuser.com/questions/266725/tmux-ctrlb-not-working)，原来Ctrl+B之后要松开再按其他的键..


##### tmux中复制模式
输入`+[进入复制模式，按下 空格键 开始复制，光标选择复制区域，回车键 复制选中文本并退出复制模式，&#124;+]粘贴文本。

#### 如果不想使用tmux
可以将进程直接放在后台运行`nohup command &`，无论是否将nohup命令输出重定向到终端，输出都会附加到当前目录的nohup.out文件中。但是在nohup使用的时候，只能够进行[一次性的命令输入](https://stackoverflow.com/questions/17475098/getting-sudo-and-nohup-to-work-together)，无法进行后续的命令输入，docker需要sudo权限输入密码可能无法使用，所以还是tmux好用一点...

###  ERROR

`error checking context: 'can't stat '/home/graphsense-blocksci/.git''.`


## graphsense-tagpack

## graphsense-REST
切换python版本的方法，跟java的比较像，[参考](https://blog.csdn.net/u013894834/article/details/75305752)

`update-alternatives --list python`，然后选择就可以了，然而我这里出了一个小错误：
`update-alternatives:error: no alternatives for python`
原来是因为python的替代版本没有被update-alternatives识别，因为我要换到3.7版本，所以我就只把3.7放到里面
`update-alternatives -- install /usr/bin/python python /usr/bin/python3.7 1`

## Swap分区
[参考](https://blog.csdn.net/wo18237095579/article/details/80367201)
## 经验
弄这个东西太痛苦了，不过每一个component跑起来的瞬间还是蛮开心的，一开始压根没想到能够跑起来，对于graphsense所用的各部分内容都没有任何了解，甚至在移动硬盘上面装虚拟机还花了大半天的时间。中间在btc-client的一个简单的权限问题上卡了好几天。以后在遇到这种情况的时候先捋清思路，把可能出现的问题写下来然后排列组合都试试。（虽然在这个例子里面并不是排列组合的情况的问题）











## Docker 安装
