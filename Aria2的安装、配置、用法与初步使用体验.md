###简介
Aria2是一个轻量级下载工具，它支持的协议包括HTTP、FTP、SFTP、BT/PT、Metalink的大多数协议。它的作用是最大化的利用带宽。更多详细的介绍，请看[Aria2项目主页](https://github.com/tatsuhiro-t/aria2)。

###安装
* 直接github主页下载安装包下载，这里不再赘述。
* 通过brew安装

```sh
$ brew install aria2
```

安装完成后，即可使用aria2c进行下载。但是，用aria2主要是为了使用RPC，用命令行直接下载并不能发挥aira2的妙用。

###配置
要使用aria2进行下载，需要做一定的配置，文件保存相关的、下载相关的、进度相关的、RPC相关的等等，这些可以在运行通过配置文件来设定，也可以通过bash中每次使用aria2c命令时，在后面添加具体的配置来设定。

首先，我们看常规的，通过配置文件在保存相关配置。
通常情况下，~目录下的一些隐藏文件，会保存配置信息。此方式，在此也作为aria2的配置保存方式。

```sh
$ cd ~
$ mkdir .aria2          //创建目录
$ cd .aria2
$ touch aria2.conf      //创建配置文件
```

然后即可编写配置文件。

网上关于aria2的配置文件很多，这里推荐一个条理相对清楚的。

```sh
## '#'开头为注释内容, 选项都有相应的注释说明, 根据需要修改 ##
## 被注释的选项填写的是默认值, 建议在需要修改时再取消注释  ##

## 文件保存相关 ##

# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=Users/XXX/Downloads/
# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
#disk-cache=32M
# 文件预分配方式, 能有效降低磁盘碎片, 默认:prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc和trunc则需要文件系统和内核支持
# NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
file-allocation=none
# 断点续传
continue=true

## 下载连接相关 ##

# 最大同时下载任务数, 运行时可修改, 默认:5
max-concurrent-downloads=1
# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=5
# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
min-split-size=10M
# 单个任务最大线程数, 添加时可指定, 默认:5
split=5
# 整体下载速度限制, 运行时可修改, 默认:0
#max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0
#max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0
#max-overall-upload-limit=0
# 单个任务上传速度限制, 默认:0
#max-upload-limit=0
# 禁用IPv6, 默认:false
disable-ipv6=true

## 日志、进度保存相关 ##

# 保存日志到文件
log-level=warn
log=/etc/aria2/aria2.log
# 从会话文件中读取下载任务
input-file=/etc/aria2/aria2.session
# 在Aria2退出时保存`错误/未完成`的下载任务到会话文件
save-session=/etc/aria2/aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
#save-session-interval=60

## RPC相关设置 ##

# 启用RPC, 默认:false
enable-rpc=true
# 允许所有来源, 默认:false
rpc-allow-origin-all=true
# 允许非外部访问, 默认:false
rpc-listen-all=true
# 事件轮询方式, 取值:[epoll, kqueue, port, poll, select], 不同系统默认值不同
#event-poll=select
# RPC监听端口, 端口被占用时可以修改, 默认:6800
#rpc-listen-port=6800
# 设置的RPC授权令牌, v1.18.4新增功能, 取代 --rpc-user 和 --rpc-passwd 选项
#rpc-secret=<TOKEN>
# 设置的RPC访问用户名, 此选项新版已废弃, 建议改用 --rpc-secret 选项
#rpc-user=<USER>
# 设置的RPC访问密码, 此选项新版已废弃, 建议改用 --rpc-secret 选项
#rpc-passwd=<PASSWD>

## BT/PT下载相关 ##

# 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
#follow-torrent=true
# BT监听端口, 当端口被屏蔽时使用, 默认:6881-6999
listen-port=51413
# 单个种子最大连接数, 默认:55
#bt-max-peers=55
# 打开DHT功能, PT需要禁用, 默认:true
enable-dht=false
# 打开IPv6 DHT功能, PT需要禁用
#enable-dht6=false
# DHT网络监听端口, 默认:6881-6999
#dht-listen-port=6881-6999
# 本地节点查找, PT需要禁用, 默认:false
#bt-enable-lpd=false
# 种子交换, PT需要禁用, 默认:true
enable-peer-exchange=false
# 每个种子限速, 对少种的PT很有用, 默认:50K
#bt-request-peer-speed-limit=50K
# 客户端伪装, PT需要
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
# 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
seed-ratio=0
# 强制保存会话, 即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
#force-save=false
# BT校验相关, 默认:true
#bt-hash-check-seed=true
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
bt-save-metadata=true
```

需要注意的是：
1. `dir=Users/XXX/Downloads/`此处需要更改为自己的下载地址

2. `input-file=/etc/aria2/aria2.session` `save-session=/etc/aria2/aria2.session`此处的session文件，可以更改为自己想要的位置。另外，文件需要自己手动创建，执行aria2并不会自动创建这两个文件。创建后注意，需保证**当前用户对其有读写权限**。

更多的配置，可到官方主页查看。

配置文件保存后，测试配置是否正确：

```sh
$ aria2c --conf-path=/Users/lifeng/.aria2/aria2.conf
```

如果配置文件有错误，执行后，会有提示。如果还是不放心，可以使用ps指令查看pid `ps aux|grep aria2c`。
如果配置正确，会看到RPC开始监听。

上述讲的是通过配置文件配置，下面看直接在aria2c命令后加参数的方式。

```sh
$ aria2c --input-file=/Users/Shared/aria2.session --save-session=/Users/Shared/aria2.session --save-session-interval=10 --dir=/Users/lifeng/Downloads/ --max-connection-per-server=10 --max-concurrent-downloads=10 --continue=true ...
```

这种方式，也可以使用。缺点就是，输入参数比较麻烦。

###使用

**前提：正确的配置文件**
####管理器一：Aria2 WebUI
可以使用[Aria2 WebUI](http://ziahamza.github.io/webui-aria2/)进行下载任务的管理。**配置文件正确，打开[Aria2 WebUI](http://ziahamza.github.io/webui-aria2/)时，会看到提示，RPC Server连接成功。**

Aria2 WebUI界面如下：
![web ui.png](https://ooo.0o0.ooo/2016/02/15/56c1be4a0e241.png)
可以添加相关的下载链接，查看进度，还有开始、暂停、取消等操作。
####管理器二：YAAW
可以使用YAAW进行下载管理。**配置文件正确，打开不会报错。**

YAAW的英文原生地址：[http://binux.github.io/yaaw/demo/#](http://binux.github.io/yaaw/demo/#)

YAAW的中文地址：[http://yaaw.ghostry.cn/](http://yaaw.ghostry.cn/)

YAAW(原生)界面如下：
![YAAW.png](https://ooo.0o0.ooo/2016/02/15/56c1bd928456b.png)
和Aria2 WebUI功能相同，添加下载链接，进度查看，以及对下载任务的相关操作。

---
###终极神器：Aria2GUI
值得注意的是：
>不管是Aria2 WebUI还是YAAW，他们运行运行之前，都要在本地命令行先开始RPC Server，也就是要执行：

```sh
$ aria2c --conf-path=/Users/你的用户名/.aria2/aria2.conf -D
```

保证了RPC Server的开启，才能通过相应的web页面连接。这样的话，不免让人觉得麻烦。而且之前还要先编写配置文件。那有没有更好的解决方案呢？能不能把这么多都整合在一起呢？答案是：有！它就是今天的主角：**Aria2GUI**

####Aria2GUI既不需要在命令行运行什么，也不需要配置什么文件，只要安装好了aria2，打开Aria2GUI即可使用。

Aria2GUI为一个MAC的本地App，其实是封装了YAAW，它界面和YAAW一样。既然他不需要配置文件，按照我们上面讲的，那就只剩下aria2c命令后加参数的方式了。查看Aria2GUI包内容，打开Resources下的startaria.sh：

```sh
#!/bin/sh
touch "/Users/Shared/aria2.session"
/Applications/Aria2GUI.app/Contents/Resources/aria2c --input-file="/Users/Shared/aria2.session" --save-session="/Users/Shared/aria2.session" --save-session-interval=10 --dir="$HOME/Downloads/" --max-connection-per-server=10 --max-concurrent-downloads=10 --continue=true --split=10 --min-split-size=10M --enable-rpc=true --rpc-listen-all=false --rpc-listen-port=6800 --rpc-allow-origin-all --check-integrity=true --bt-enable-lpd=true --follow-torrent=true --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/601.3.9 (KHTML, like Gecko) Version/9.0 Safari/601.3.9" -c -D
```

印证了我们的猜想，aria2c后加参数启动，不过这次是Aria2GUI写好的，用户什么都不需要管，打开直接使用就行。是不是较其他方式，方便很多呢？

---
###初步使用体验
目前，仅测试了网盘、BT、磁链部分资源，HTTP和FTP并没有测试。
* 可突破国内主流网盘的速度限制（百度云，115和迅雷离线）。从下图可以看到，下载速度都有明显提升！

百度云资源直接下载：
![baidu s.png](https://ooo.0o0.ooo/2016/02/15/56c1bd6a6cac3.png)
百度云资源aria2下载：
![baidu a.png](https://ooo.0o0.ooo/2016/02/15/56c1bd6a77ced.png)

115网盘资源直接下载：
![115 s.png](https://ooo.0o0.ooo/2016/02/15/56c1bd92713ee.png)
115网盘资源aria2下载：
![115 a.png](https://ooo.0o0.ooo/2016/02/15/56c1bd6a9cd57.png)

注：
>1. 网盘资源aria2下载，需安装相关插件。相应的Chrome插件分别是：[迅雷离线助手](https://chrome.google.com/webstore/detail/thunderlixianassistant/eehlmkfpnagoieibahhcghphdbjcdmen?hl=zh-CN)，[百度网盘助手](https://chrome.google.com/webstore/detail/baiduexporter/mjaenbjdjmgolhoafkohbhhbaiedbkno)，[115下载助手](https://chrome.google.com/webstore/detail/115exporter/ojafklbojgenkohhdgdjeaepnbjffdjf)
2. 通过迅雷离线因和迅雷会员相关，故非会员无法使用aria2下载迅雷离线资源。

* BT以及磁链下载速度反而下降了。这一点有待进一步观察。~~难道是我的打开方式不对？~~具体见下图：

迅雷直接下载BT任务：
![BT x.png](https://ooo.0o0.ooo/2016/02/15/56c1bd6a9ba50.png)
Aria2下载BT任务：
![BT a.png](https://ooo.0o0.ooo/2016/02/15/56c1bd6a9eb44.png)

---
###总结
Aria2是一种多协议，轻量级的工具。它有两种配置方式：加运行参数方式和加载配置文件的方式。可以使用Aria2 WebUI、YAAW和Aria2GUI对下载进行可视化的管理。最后介绍了初试使用情况，网盘加速明显，BT和磁链反而基本没有速度。所以就目前的使用情况看，aria2+迅雷，可谓是完美搭配，基本解决了所有下载的相关问题！