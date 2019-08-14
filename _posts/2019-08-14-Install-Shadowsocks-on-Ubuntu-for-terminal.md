---
layout: post
title: Install Shadowsocks on Ubuntu for terminal
categories: Tools
description: 本来为了给孙小宝下载数据集，结果发现用浏览器也可以下
keywords: Tools, Shadowsocks, Ubuntu
---

## 命令行客户端

新买的服务器一直没有进行Shadowsocks相关设置，而学校的核心网更新过后实验室服务器连接有线下载速度贼快，于是倒腾了半天在Ubuntu安装了Shadowsocks的命令行客户端，并熟悉了相关操作，记录如下

### 1.安装
以Python版本的Shadowsocks为例

Ubuntu:
```
apt-get install python-pip
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```
Linux distributions with snap:
```
snap install shadowsocks
```
### 2.创建Shadowsocks配置文件
创建一个 /etc/shadowsocks.json文件，格式如下
```
    "server":"服务器 IP 或是域名,如域名：us1-sta93.1ebnz.website",
    "server_port":端口号,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"密码",
    "timeout":300,
    "method":"加密方式 (chacha20-ietf-poly1305 / aes-256-cfb)",
    "fast_open": false
```
### 3.启动Shadowsocks
Python 版客户端命令是 sslocal，可以先用find或者locate命令查找一下sslocal的位置
```
sudo /home/imimo/anaconda3/bin/sslocal -c /etc/shadowsocks.json -d start
```
### 4.终端内使用，需要安装proxychains

ubuntu:

```
sudo apt-get install proxychains
```

编辑 `/etc/proxychains.conf`

修改最后一行

```
socks5 127.0.0.1 1080
```

接着我们就可以直接 用 `proxychains` + 命令的方式使用代理，例如

```
proxychains curl xxxx
proxychains wget xxxx
sudo proxychains apt-get xxxx
```

### 5.关闭Shadowsocks

两种方式任选其一来关闭Shadowsocks

1)在终端内输入

```
lsof –i:1080
```

kill 相应的 pid 即可，kill命令为

```
kill -9 PID
```

2)通过sslocal命令关闭

```
sudo /home/imimo/anaconda3/bin/sslocal -c /etc/shadowsocks.json -d stop
```

