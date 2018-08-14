---
title: 通过charles代理获取手机app请求数据
date: 2017-11-10 19:32:15
tags:
 - charles
 - http
 - https
 - crawler
---

### WHAT
发现了神奇的抓包工具charles，可以抓取手机app的api调用，然后抓取相关数据；

### WHY
准备抓取app数据，然后找到了charles，在此记录以便以后查询；

<!-- more -->

### HOW
#### 安装charles
[windows、mac、linux版都有](https://www.charlesproxy.com/download/)

#### 获取网友技术支持
[技术支持](https://www.zzzmode.com/mytools/charles/)

随便输入一个RegisterName,选择版本,生成文件，然后下载下来替换charles目录中的charles.jar
```
mac: /Applications/Charles.app/Contents/Java/charles.jar
windows: C:\Program Files\Charles\lib\charles.jar
linux: {charles_path}/lib/charles.jar
```

#### 使用方法

	手机和电脑需要在同一个局域网下

##### 手机安装配置chales代理
- 点击“设置->无线局域网->#{当前链接的wifi右侧的<i>}”
- 滑动到最下方点击“配置代理”
- 选择手动， 服务器填电脑的局域网ip，端口填8888
- 手机配置好代理后，电脑上charles会有个弹窗，提示是否允许来自xxx.xxx.xxx.xxx请求，点“Allow”

至此，已经可以获取手机上的http请求了。但是，https请求的内容还是一片乱码，需要继续配置

##### 手机安装证书
- 点击 Help -> SSL Proxying -> Install Charles Root Certificate on a Mobile Device
- 从弹窗获取手机安装SSL证书的地址 chls.pro/ssl
- 在手机Safari浏览器输入地址 chls.pro/ssl，出现证书安装页面，点击安装
- IOS 需要在 “设置→通用→关于本机→证书信任设置” 里面启用完全信任Charles证书

##### 3. charles配置https代理
- 点击 Proxy -> SSL Proxying Settings...
- 点击 下方“add”，添加一个域名，允许使用通配符(如：*.baidu.com)，port处填443
- 点击 “ok”

至此，已经可以获取手机上的https请求了。

### END

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>