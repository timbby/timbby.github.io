---
title: 安装nvm，灵活管理node版本
date: 2018-11-23 11:48:33
tags:
 - ubuntu
 - nvm
 - node
---


### WHAT
在ubuntu上安装nvm，领过的管理node版本

### WHY
新建服务器，需要安装node环境，搭建node服务

<!-- more -->

### HOW

#### 获取到当前的nvm
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

#### 执行：
```
export NVM_DIR="$HOME/.nvm"
```


#### 将下面配置配置到 /etc/profile
```
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

#### 安装相关版本的node
```
nvm install v9.7.1
```
#### 查看node版本
```
root@xxx:~# node -v
v0.10.32
```

### END

大功告成！
