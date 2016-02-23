在文章[Aria2的安装、配置、用法与初步使用体验](https://github.com/MeiXXD/SourceTree/blob/master/Aria2%E7%9A%84%E5%AE%89%E8%A3%85%E3%80%81%E9%85%8D%E7%BD%AE%E3%80%81%E7%94%A8%E6%B3%95%E4%B8%8E%E5%88%9D%E6%AD%A5%E4%BD%BF%E7%94%A8%E4%BD%93%E9%AA%8C.md)以及[Aria2的拓展使用](https://github.com/MeiXXD/SourceTree/blob/master/Aria2%E7%9A%84%E6%8B%93%E5%B1%95%E4%BD%BF%E7%94%A8.md)中，介绍了Aria2的一些日常使用，包括本机下载、远程下载，以及一些相关GUI的使用。在使用过程中，作为一个重度chrome用户，我一直在想有没有Aria2的相关插件，这样使用过程中，无论在易用性（相关网盘的插件也都有chrome对应的插件）方面，还是操作（不用再去为了某一简单的需求而去安装具体app）方面，都简单了很多。然后就发现了今天的主角**[YAAW for Chrome](https://chrome.google.com/webstore/detail/dennnbdlpgjgbcjfgaohdahloollfgoc)**。下面着重介绍一下它的使用。

---
首先，从命名就可以看出，YAAW for Chrome是YAAW的插件版本，其界面与我们在笔记[Aria2的安装、配置、用法与初步使用体验](https://github.com/MeiXXD/SourceTree/blob/master/Aria2%E7%9A%84%E5%AE%89%E8%A3%85%E3%80%81%E9%85%8D%E7%BD%AE%E3%80%81%E7%94%A8%E6%B3%95%E4%B8%8E%E5%88%9D%E6%AD%A5%E4%BD%BF%E7%94%A8%E4%BD%93%E9%AA%8C.md)中的YAAW一样，这里不再赘述。

下面介绍插件的使用相关。需要考虑以下几个点：
* 因为是插件的形式，所以我们需要本地的配置文件。
* 并且我们需要保证aria2c服务已经开启，如果不想每次都手动开启此服务，需要将其加入开机启动。

###配置文件编写
首先，创建配置文件目录，然后写入配置文件。步骤在笔记中[Aria2的安装、配置、用法与初步使用体验](https://github.com/MeiXXD/SourceTree/blob/master/Aria2%E7%9A%84%E5%AE%89%E8%A3%85%E3%80%81%E9%85%8D%E7%BD%AE%E3%80%81%E7%94%A8%E6%B3%95%E4%B8%8E%E5%88%9D%E6%AD%A5%E4%BD%BF%E7%94%A8%E4%BD%93%E9%AA%8C.md)已经介绍过了，这里只示例一份简单的配置文件：

```sh
#用户名
#rpc-user=user
#密码
#rpc-passwd=passwd
#上面的认证方式不建议使用,建议使用下面的token方式
#设置加密的密钥
#rpc-secret=token
#允许rpc
enable-rpc=true
#允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true
#允许外部访问，false的话只监听本地端口
rpc-listen-all=true
#RPC端口, 仅当默认端口被占用时修改
#rpc-listen-port=6800
#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5
#断点续传
continue=true
#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
#文件保存路径, 默认为当前启动位置
dir=/Users/你的用户名/Downloads
#文件缓存, 使用内置的文件缓存, 如果你不相信Linux内核文件缓存和磁盘内置缓存时使用, 需要1.16及以上版本
#disk-cache=0
#另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
#enable-mmap=true
#文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
#所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支持
file-allocation=prealloc
```

>下载目录需更改为本机自己的用户名。

###开机启动aria2c服务
首先到/Library/LaunchDaemons目录下面创建对应的自启动配置文件

```sh
$ cd /Library/LaunchDaemons
...
$ touch aria2.plist
```

这样就创建了名为 `aria2.plist` 的自启动配置文件，然后在配置文件中写入：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>KeepAlive</key><true/>
	<key>RunAtLoad</key><true/>
	<key>Label</key><string>aria2</string>
	<key>ProgramArguments</key>
		<array>
			<string>/usr/local/bin/aria2c</string>
			<string>--conf-path=/Users/你的用户名/.aria2/aria2.conf</string>
		</array>
</dict>
</plist>
```

>配置文件中 `你的用户名` 需更改为本机自己的用户名。

这样开机就会自动启动aria2c服务。

###使用插件YAAW for Chrome下载
首先看一下YAAW for Chrome的设置页面：
![屏幕快照 2016-02-19 16.45.40.png](https://ooo.0o0.ooo/2016/02/22/56cbd8862bdf6.png)
这里的下载文件大于多少M将被导入aria2c，可以自己设置，默认为100M。也可以自己集成右键菜单，将下载链接导入aria2c。

相比于其他的配置形式，该配置更加简单，使用也最方便。因设备A可以开启了aria2c的服务，所以只要配置一设备B，即可轻松实现远程下载。具体远程下载的配置，请移步[Aria2的拓展使用](https://github.com/MeiXXD/SourceTree/blob/master/Aria2%E7%9A%84%E6%8B%93%E5%B1%95%E4%BD%BF%E7%94%A8.md)。

---
###总结
介绍了另外一种Aria2的使用方式，即配合chrome插件使用。该用法的好处是简单，易操作，即使远程下载，也能轻松解决，推荐chrome党使用。