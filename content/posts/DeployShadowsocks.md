---
title: CentOS 7下安装Shadowsocks 搭建ss
date: 2018-03-05T21:36:00+08:00
comments: true
toc: true
tags:
 - 教程
---

每次搭ss都要上网查一下教程，但网上的教程都没有提到针对CentOS7下的防火墙的处理，这里便想自己写一篇留个档。

<!-- more -->

### 安装pip

pip是 python 的包管理工具。在本文中将使用 python 版本的 shadowsocks，此版本的 shadowsocks 已发布到 pip 上，因此我们需要通过 pip 命令来安装。

在控制台中执行以下命令安装pip

```bash
$ curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
$ python get-pip.py
```

### 安装配置Shadowsocks

在控制台中执行以下命令安装Shadowsocks

```bash
$ pip install --upgrade pip
$ pip install shadowsocks
```

安装完成后，创建配置文件`/etc/shadowsocks.json`，内容如下

笔者习惯创建多端口的配置，故此以多端口为例

```JSON
{
"server":"0.0.0.0",
"local_address": "127.0.0.1",
"local_port":1080,
"port_password":{
	"8381": "D77b73E578",
	"8382": "53AFf96aEf",
	"8383": "6E18a11eA2",
	"8384": "OTU0OWQ2Nz"
},
"timeout":300,
"method":"aes-256-cfb",
"fast_open": false
}
```

几点说明：

* `server`处填写你的服务器的地址
* `method`为加密方法，可选`aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table`等

ss的启动与停止都很简单

```bash
$ ssserver -c /etc/shadowsocks.json -d start #启动
$ ssserver -c /etc/shadowsocks.json -d stop #停止
```

### 解决CentOS7的防火墙问题

CentOS 7默认使用的是firewall作为防火墙，这里我们将使用iptables

在控制台中执行以下命令关闭及禁用firewall

```bash
$ systemctl stop firewalld.service #停止firewall

$ systemctl disable firewalld.service #禁止firewall开机启动
```

然后在控制台中执行以下命令安装iptables

```bash
yum -y install iptables-services
```

然后执行以下命令开放端口

```bash
iptables -A INPUT -p tcp -dport 此处填端口号 -j ACCEPT
```

最后重启防火墙使配置生效并设置防火墙开机启动

```bash
$ systemctl restart iptables.service

$ systemctl enable iptables.service
```

然后你就可以通过你的shadowsocks上网啦！