---
title: "Mac catalina mysql"
key: 2019-12-17-catalina-mysql
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---
# Mac catalina mysql
## 问题梳理
首先是maven项目里面的jetty:run无法连接，显示
![jettyrun](https://note.youdao.com/yws/api/personal/file/WEBb7e0809e68c19400224697bddb7b8ce6?method=download&shareKey=8e42f4437199950ae3a3f002926fc0c7)
向上查错，发现是数据库的连接出现了错误。
## brew install mysql
经过查错，发现是mysql.server start之后，随即会被关闭。因为某些原因放弃了使用brew install下载的mysql，转到官网下载mysql。
## 网站安装
### mysql显示
从网站上面安装下来的mysql会在系统偏好设置中显示。
![系统偏好设置显示](https://note.youdao.com/yws/api/personal/file/WEB60db962a49bac4781cc5db1d6c48fa07?method=download&shareKey=9d80ffdbd00f31201a4923aa641cf3f0)
在这里面尝试会发现，的确是开启了之后会短暂的连接，随后就会断开
![preference_mysql](https://note.youdao.com/yws/api/personal/file/WEB7d97807053248b9d315037c6b18dcc61?method=download&shareKey=edbdb9998d90ee508bc3dfd51086a910)
initialize database里面有两个选项
![initialize](https://note.youdao.com/yws/api/personal/file/WEB693ce43922d37f2931d2fd3d8e13d90c?method=download&shareKey=d6a4196ab757329a5a5b5d0b1e9d5636)
这里面选择了legacy password，因为如果选择strong password的话，在连接workbench的时候会连接不上。
[reference](https://www.jianshu.com/p/2c2a2e0955f0)
### 在命令行输入mysql显示command not found
#### 首先配置环境变量
`echo $SHELL`
终端返回内容

`bin bash`  
输入`cd /usr/local/mysql`  
之后输入`sudo vim .bash_profile`  
在文档中写入`export PATH=${PATH}:/usr/local/mysql/bin`随后保存。  
输入`source .bash_profile`运行环境变量  
现在可以在终端使用mysql了，命令如下   
`mysql -u root -p`

![终端显示](https://note.youdao.com/yws/api/personal/file/WEB3d384cac4a45f71fa72fe31a8db69413?method=download&shareKey=754341fa7f65fd60a09c126f7675d1f8)

不过现在还有一个warning，应该是etc文件出了问题。  
查看my.cnf的权限  
`ls -l /etc/my.cnf`  
返回`-rw-r--rw-@ 1 777  wheel  150 12 16 16:53 /etc/my.cnf`  
说明任何一个用户都可以修改my.cnf。  
取消这条warning的方法  
`chmod 644 /etc/my.cnf`  

### 解决方法探索
* 卸载原版本，安装[community版本](https://dev.mysql.com/downloads/mysql/)。
这种方法经验证不可以。
* 给root用户赋予足够的权限  
`mysql -u root -p`，进入mysql命令行  
`grant all on *.* to 'root'@'localhost';`
* 数据库短暂连接的问题通过重启电脑解决掉了
* 后来发现是数据库被清空了，报错变成了找不到表..所以估计是因为给用户赋予了权限所以做成的吧。
