---
title: 配置Ubuntu允许远程SSH连接
images: /images/abstract_pic/
copyright: true
permalink: 1
date: 2020-01-09 14:09:40
tags: [Ubuntu,ssh]
categories:
password:
---

# 配置Ubuntu允许远程SSH连接
```
Sudo vim /etc/ssh/sshd_config
```
* PermitRootLogin yes
* PasswordAuthentication yes

```
Sudo -i service sshd restart
```
* ```passwd```改密码

* ```ssh root@ip```
* 先```sudo -i``` 进入root权限  
  
```wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh```

```wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh```

```chmod +x shadowsocks-all.sh```
```./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log```
* 选择go

```
/etc/init.d/shadowsocks-go restart
Service sshd restart
Service ssh restart
```
### 完成
