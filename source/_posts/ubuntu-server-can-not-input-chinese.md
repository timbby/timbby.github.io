---
title: ubuntu 服务器终端无法输入中文
date: 2018-11-23 10:38:06
tags:
 - ubuntu
 - 中文
---


### WHAT
新建的ubuntu服务器，ssh进去后无法在终端输入中文

### WHY
新建了一台ubuntu16.04服务器，需要添加git用户，但是输入中文的时候发现无法输入

<!-- more -->

### HOW
#### 参考
[ubuntu server显示并输入中文](https://blog.csdn.net/vivian_ll/article/details/80417661)

#### 修改/etc/default/locale为：
```shell
LANG=”zh_CN.UTF8” 
LANGUAGE=”zh_CN:zh”
```

#### 安装中文包支持
```shell
apt-get update && apt-get install language-pack-zh-hans 
```

#### 退出ssh后重新进入
```shell
exit
ssh root@xxx.xxx.xxx.xxx
```

#### 输入中文
```
git config user.name 啦啦啦
```

### END
大功告成！

