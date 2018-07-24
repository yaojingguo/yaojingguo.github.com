---
layout: post
title: "GCE Shadowsocks Setup"
description: ""
category:
tags: []
---
{% include JB/setup %}

## GCE
VM settings:
- OS: Ubuntu 16.04.4 LTS
- zone: asia-northeast1-b
- internal IP: use static IP
- external IP: use static IP
- copy my own .ssh/id_rsa.pub to `SSH Keys` section

In `VPC Network->Firewall rules`, add `shadowsocks` rule:
- IP ranges: `0.0.0.0/0`
- Protocols and ports: `tcp:9999`

After the setup, using ssh in a terminal should be able to connect to the VMN.

Use [How do I fix my locale issue?](https://askubuntu.com/a/913263) to remove
the following warning:

```
WARNING! Your environment specifies an invalid locale.
 The unknown environment variables are:
   LC_CTYPE=UTF-8 LC_ALL=
 This can affect your user experience significantly, including the
 ability to manage packages. You may install the locales by running:

   sudo apt-get install language-pack-UTF-8
     or
   sudo locale-gen UTF-8
```

## Shadowsocks
### Installation
Install shadowsocks-python:

```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
sudo ./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

### BBR
Add the following content to `sysctl -p`:
```
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
```

Enable the configuration:
```
$ sudo sysctl -p
```

Verify that BBR is working:
```
$ lsmod | grep bbr
tcp_bbr                20480  0
```

- [一键安装最新内核并开启 BBR 脚本](https://teddysun.com/489.html)
- [轻松在 VPS 搭建 Shadowsocks 翻墙 ($5/月 支付宝)](https://www.diycode.cc/topics/738)
- [Shadowsocks 一键安装脚本（四合一）](https://teddysun.com/486.html)
- [用Google Cloud Platform搭建Shadowsocks服务教程](http://godjose.com/2017/06/14/new-article/)
