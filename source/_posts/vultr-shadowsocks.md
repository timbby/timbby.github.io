---
title: 【搭梯子】购买服务器并搭建ss服务
date: 2018-09-27 13:42:58
tags:
 - shadowsocks
 - vultr
 - ubuntu
---


### WHAT
在[Vultr](https://www.vultr.com/?ref=7404275)上购买VPS，搭建SS服务

### WHY
之前在服务器上搭了VPN，后来发现VPN极其不稳定，所以打算改用SS，所以记录一下配置过程

<!-- more -->

### HOW
#### 购买VPS
在[Vultr](https://www.vultr.com/?ref=7404275)上购买VPS，最低3.5$/月，1核，512M内存，20G SSD，500G流量。

#### 安装shadowsocks
```shell
apt-get install python-pip //安装pip
pip install shadowsocks // 通过pip安装shadowsocks
```

#### 配置shadowsocks
```shell
vi /etc/shadowsocks.json // shadowsocks配置文件
```

```json
//===========配置文件内容============
{
  "server": "0.0.0.0", // 服务ip，固定写0.0.0.0
  "server_port": 1123, // 服务端口，1024以上自选
  "password": "150423", // 客户端链接用的密码
  "method": "aes-256-cfb" // 加密协议
}
//=================================
```

#### 关闭防火墙，启动SS服务
```shell
ssserver -c /etc/shadowsocks.json -d start // 开启shadowsocks服务
systemctl stop firewalld.service // 关闭防火墙
```

#### 客户端下载
IOS在APP Store搜索 ```SUPERWINGY``` 3块钱
[MAC下载地址](https://github.com/shadowsocks/ShadowsocksX-NG/releases/download/v1.7.1/ShadowsocksX-NG.1.7.1.zip)

### END

至此就可以愉快的科学上网了