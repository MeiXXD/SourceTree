###简介
 DIG，全称Domain Information Groper，中文叫域信息搜索器。原本是Linux平台上BIND服务器诊断的工具。它执行DNS搜索，显示从受请求的域名服务器返回的答复。DNS管理员常利用dig作为DNS问题的故障诊断，因为它灵活性好、易用、输出清晰。

###使用
dig的详细使用，可以通过man dig来获取，这里不再赘述。

下面主要介绍dig的典型使用，具体如下：
>dig @server name type

其中：
* server代表将要使用的DNS服务器，可以是域名或者IP地址
* name代表要查询的具体域名地址
* tpye代表查询类型，比如A、MX、SIG，以及任何有效查询类型等。默认是A。

下面看一个示例：

```c
$ dig @8.8.8.8 baidu.com A

; <<>> DiG 9.8.3-P1 <<>> @8.8.8.8 baidu.com A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51605
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;baidu.com.			IN	A

;; ANSWER SECTION:
baidu.com.		343	IN	A	220.181.57.217
baidu.com.		343	IN	A	111.13.101.208
baidu.com.		343	IN	A	123.125.114.144
baidu.com.		343	IN	A	180.149.132.47

;; Query time: 85 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Thu Feb 11 12:24:13 2016
;; MSG SIZE  rcvd: 91
```

示例中使用谷歌的DNS服务器去查询baidu.com，自上而下的信息分别是：
* dig版本 `<<>> DiG 9.8.3-P1 <<>>`
* 状态信息 `->>HEADER<<- ...`
* 问题部分 `QUESTION SECTION ...`
* 查询结果部分 `ANSWER SECTION ...`
* 统计信息 `Query time ...`

###使用+trace参看详细过程
dig中经常使用的参数，有两个需要注意：
1. **+short** 简短地输出查询结果
2. **+trace** 详细输出整个查询过程 

使用+short，用法如下：

```c
$ dig +short baidu.com
220.181.57.217
180.149.132.47
123.125.114.144
111.13.101.208
```

使用+trace，用法如下：

```c
$ dig +trace baidu.com

; <<>> DiG 9.8.3-P1 <<>> +trace baidu.com
;; global options: +cmd
.			56108	IN	NS	d.root-servers.net.
.			56108	IN	NS	e.root-servers.net.
.			56108	IN	NS	a.root-servers.net.
.			56108	IN	NS	m.root-servers.net.
.			56108	IN	NS	c.root-servers.net.
.			56108	IN	NS	g.root-servers.net.
.			56108	IN	NS	b.root-servers.net.
.			56108	IN	NS	k.root-servers.net.
.			56108	IN	NS	h.root-servers.net.
.			56108	IN	NS	l.root-servers.net.
.			56108	IN	NS	j.root-servers.net.
.			56108	IN	NS	f.root-servers.net.
.			56108	IN	NS	i.root-servers.net.
;; Received 228 bytes from 202.99.216.113#53(202.99.216.113) in 38 ms

com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
;; Received 499 bytes from 192.36.148.17#53(192.36.148.17) in 90 ms

baidu.com.		172800	IN	NS	dns.baidu.com.
baidu.com.		172800	IN	NS	ns2.baidu.com.
baidu.com.		172800	IN	NS	ns3.baidu.com.
baidu.com.		172800	IN	NS	ns4.baidu.com.
baidu.com.		172800	IN	NS	ns7.baidu.com.
;; Received 197 bytes from 192.31.80.30#53(192.31.80.30) in 511 ms

baidu.com.		600	IN	A	180.149.132.47
baidu.com.		600	IN	A	220.181.57.217
baidu.com.		600	IN	A	111.13.101.208
baidu.com.		600	IN	A	123.125.114.144
baidu.com.		86400	IN	NS	ns3.baidu.com.
baidu.com.		86400	IN	NS	ns4.baidu.com.
baidu.com.		86400	IN	NS	dns.baidu.com.
baidu.com.		86400	IN	NS	ns2.baidu.com.
baidu.com.		86400	IN	NS	ns7.baidu.com.
;; Received 261 bytes from 202.108.22.220#53(202.108.22.220) in 27 ms
```

可以看到，从根域到com域再到baidu.com域，最后到具体的IP地址的整个查询过程。

要注意的是，有时候dig的结果，显示的并不是A记录，而是CNAME别名记录这时候只需要再dig这个别名就可以得到IP地址了。

下例中出现了gitcafe.io别名记录，此时我们再dig gitcafe.io就可以了。

```c
...
leaver.me.		10	IN	CNAME	gitcafe.io.
leaver.me.		86400	IN	NS	f1g1ns2.dnspod.net.
leaver.me.		86400	IN	NS	f1g1ns1.dnspod.net.
;; Received 115 bytes from 112.90.82.194#53(112.90.82.194) in 67 ms
...

$ dig gitcafe.io.

; <<>> DiG 9.8.3-P1 <<>> gitcafe.io.
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11532
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;gitcafe.io.			IN	A

;; ANSWER SECTION:
gitcafe.io.		600	IN	A	119.81.161.100

;; Query time: 49 msec
;; SERVER: 202.99.216.113#53(202.99.216.113)
;; WHEN: Thu Feb 11 13:13:44 2016
;; MSG SIZE  rcvd: 44
```

###dig的小应用
日常生活中，我们应用比较多的就是获取外网IP地址，下面介绍两个办法获取外网的IP：
* 使用curl指令，缺点是比较慢

```c
$ curl ifconfig.me
118.72.101.137
```

* 使用dig指令，速度快

```c
$ dig +short @resolver1.opendns.com myip.opendns.com
118.72.101.137
```

这里其实是使用了opendns的API：使用opendns的dns服务器解析myip.opendns.com，就会得到外网的IP地址。
~~不要问我为什么，API就是API~~

###总结
dig是从DNS服务器获取IP地址的工具，因其可输出详细信息，所以也常用于DNS问题诊断。另外，也介绍了dig在日常生活中的一点小应用。

如A记录，CNAME记录等术语不清楚，请查看[DNS名词解释](quiver:///notes/80CBF1D9-4DE7-4054-B20A-6BDDF3687EC3)
