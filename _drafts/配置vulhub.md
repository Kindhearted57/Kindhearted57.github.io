
# Docker简介
## docker Image介绍

# 配置vulhub
环境 ubuntu 18.04
## 安装docker
### 切换到root权限
`sudo -i`  
### 安装以下包使得apt可以通过https使用存储库
`apt-get install apt-transport-https ca-certificates curl software-properties-common`
### 添加Docker官方的GPG密钥
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
### 通过查找后八位数字确认key被添加到fingerprint里面
`sudo apt-get fingerprint 0EBFCD88`  
返回

```
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]

```

### 安装稳定存储库
`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`
### 更新apt包索引
`apt-get update`
### 下载
`sudo apt-get install docker-ce docker-ce-cli containerd.io`
### 检查安装是否成功
`docker version`  
`docker run hello-world`
## 安装docker-compose
### 安装
`pip install docker-compose`
### 检查安装是否成功
`docker-compose -v`
### 报错
## 安装Vulhub
### 下载Vulhub
`wget https://github.com/vulhub/vulhub/archive/master.zip -O vulhub-master.zip`
### 解压
`unzip vulhub-master.zip`
## Vulhub使用
### 进入vulhub目录
`cd vulhub-master`
### 进入某一个漏洞的目录
`cd flask/ssti`
### 开启漏洞环境
`docker-compose up -d`
### 测试完毕之后，记得结束服务
`docker-compose down`
## MetaDockers
⚠️一定要在python2的版本下安装，在python3尝试过，不可以。
这其实是一个一个界面，可以显示本地已经有的image。
### 修改controller/lib/config.conf
将vulhub_path改成自己的vulhub的目录地址。
### 安装相关依赖
`sudo pip install -r requirements.txt`
### 切换到MetaDocker所在文件夹
`python manage.py runserver 8000`

## 漏洞使用 
### dns-zone-transfer
#### 漏洞原理
DNS协议支持使用axfr类型的记录进行区域传送，用来解决主从同步的问题。如果管理员在配置DNS服务器的时候没有限制允许获取记录的来源，将会导致DNS域传送漏洞。  
AXFR请求，是从DNS服务器请求在主DNS服务器上更新信息的一类域名系统的请求。
* [ZoneTransfer简介](https://digi.ninja/projects/zonetransferme.php)
[这个域名](zonetransfer.me)，开启了zone transfer，其中的两个名称服务器是nsztm1.digi.ninja和nsztm2.digi.ninja。

#### 报错
[参考](https://askubuntu.com/questions/907246/how-to-disable-systemd-resolved-in-ubuntu)  

```
Starting dns-zone-transfer_dns_1 ... 
Starting dns-zone-transfer_dns_1 ... error

ERROR: for dns-zone-transfer_dns_1  Cannot start service dns: driver failed programming external connectivity on endpoint dns-zone-transfer_dns_1 (f5ba335fb9b781c9f95b19fc5d31c61af2edb15102a49b5afda6dc4b58b4859f): Error starting userland proxy: listen tcp 0.0.0.0:53: bind: address already in use

ERROR: for dns  Cannot start service dns: driver failed programming external connectivity on endpoint dns-zone-transfer_dns_1 (f5ba335fb9b781c9f95b19fc5d31c61af2edb15102a49b5afda6dc4b58b4859f): Error starting userland proxy: listen tcp 0.0.0.0:53: bind: address already in use
ERROR: Encountered errors while bringing up the project.
```  

是因为本地的DNS服务器被占用，需要关闭本机的DNS服务器。  
首先停止systemd-resolved service
`sudo systemctl disable systemd-resolved`
`sudo systemctl stop systemd-resolved`
然后在`/etc/NetworkManger/NetworkManager.conf`里面的`[main]`部分加上`dns=default`  
然后`rm /etc/resolv.conf`  
重新启动NetworkManager  
`sudo systemctl restart NetworkManager`
#### 漏洞利用
* 在linux下，使用dig命令发送dns请求，比如可以使用`dig @your-ip www.vulhub.org`获取域名`www.vulhub.org`在目标dns服务器上的A记录。
* 发送axfr类型的dns请求`dig @your-ip -t axrf vulhub.org`，可以获取到`vulhub.org`的所有子域名记录，这里存在着DNS域传送漏洞。
* 可以用nmap script扫描这个漏洞`nmap --script dns-zone-transfer.nse --script-args "dns-zone-transfer.domain=vulhub.org" -Pn -p 53 your-ip`
### ghostscript沙箱绕过（命令执行）漏洞(CVE-2018-16509)
#### 漏洞利用
环境启动之后，访问http://your-ip:8080可以看到一个上传组件。上传poc.png，将执行命令`id > /tmp/success && cat /tmp/success`。此时进入容器`docker-compose exec web bash`。
输入命令`ls /tmp/`，可以看到返回success。
也可以使用命令行测试该漏洞。
``docker run -it --rm --name im -v `pwd `/poc.png:/poc.png vulhub/imagemagick:7.0.8 convert /poc.png /poc.gif ``

## Ubuntu使用
Ubuntu 18.04
`sudo apt-get uninstall python3.6`
ping:command not found  
  
## markdown记录

### 在代码段中输入  
[参考文献](https://segmentfault.com/q/1010000012463978)


