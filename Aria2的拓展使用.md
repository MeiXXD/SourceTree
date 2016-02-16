上篇笔记，介绍了[Aria2的安装、配置、用法与初步使用体验](https://github.com/MeiXXD/SourceTree/blob/master/Aria2%E7%9A%84%E5%AE%89%E8%A3%85%E3%80%81%E9%85%8D%E7%BD%AE%E3%80%81%E7%94%A8%E6%B3%95%E4%B8%8E%E5%88%9D%E6%AD%A5%E4%BD%BF%E7%94%A8%E4%BD%93%E9%AA%8C.md)。今天，介绍一下，Aria2更为广泛的用法。
###远程下载
作为一个下载工具，在本机下载固然可以。那么，在机器A上开启了aria2，在机器B上能不能控制A上的aria2进行下载呢？答案是：可以。

下面，先看一下本机使用的配置信息，这对远程控制下载有帮助。以YAAW为例：
![屏幕快照 2016-02-16 10.07.16.png](https://ooo.0o0.ooo/2016/02/15/56c28ddb9949c.png)
RPC Server的配置信息是：`http://127.0.0.1:6800/jsonrpc`，可以看到，配置是IP地址+端口号的方式，进一步推广到远程下载，有：`http://host:port/jsonrpc`，这是最普通的远程控制下载方式。远程主机A启动RPC服务，在主机B配置`http://host:port/jsonrpc`进行连接，并控制下载。

下面看一下其他更为安全的远程控制方式：
* 使用 --rpc-secret=xxxxxx 选项设置为: `http://token:xxxxxx@host:port/jsonrpc`
* 使用 --rpc-user=user --rpc-passwd=pwd 选项设置为: `http://user:pwd@host:port/jsonrpc`

注：
>host: 指运行Aria2所在机器的IP地址或者名字
>port: 使用 --rpc-listen-port 选项设置的端口, 未设置则是6800

###aria2命令行下载
下面介绍一下，aria2命令行下载方式，给出几个官方的示例：
* web下载：

```sh
$ aria2c http://example.org/mylinux.iso
```

* 同时下载两个资源：

```sh
$ aria2c http://a/f.iso ftp://b/f.iso
```

* 每个主机使用两个连接：

```sh
$ aria2c -x2 http://a/f.iso
```

* BT下载：

```sh
$ aria2c http://example.org/mylinux.torrent
```

* BT磁链URL下载：

```sh
$ aria2c 'magnet:?xt=urn:btih:248D0A1CD08284299DE78D5C1ED359BB46717D8C'
```

* 磁链下载：

```sh
$ aria2c http://example.org/mylinux.metalink
```

* 批量下载文件中的URI：

```sh
$ aria2c -i uris.txt
```
