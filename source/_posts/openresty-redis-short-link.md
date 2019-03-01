---
title: 使用openresty和redis实现一个简单的短连接服务
date: 2019-03-01 17:16:56
tags:
 - nginx
 - openresty
 - redis
 - ubuntu
---

### WHAT
基于openresty和redis的一个短连接服务

### WHY
项目中需要一个短连接服务，用来给用户发短信时缩短短信中的连接

<!-- more -->

### HOW

#### 需要的环境

- ubuntu/macOS
- [openresty](http://openresty.org/cn/)（一款基于 NGINX 和 LuaJIT 的 Web 平台，可以理解为直接在nginx上执行lua脚本）
- redis

#### 安装redis
```shell
# ubuntu安装redis
apt-get install redis-server
```
mac的安装方法，由于电脑上已经装了redis，所以没有重新折腾，具体安装方法详见[谷歌](https://www.google.com.hk/search?q=mac%E5%AE%89%E8%A3%85redis)

##### 启动redis

```shell
# 最后的&是为了让redis在后台运行
redis-server &
```

#### 安装openresty

##### mac[命令安装](https://openresty.org/cn/installation.html)

当然你也可以选择下载源码手动编译

```shell
brew install openresty/brew/openresty
```

##### Ubuntu[命令安装](https://openresty.org/cn/linux-packages.html)

```shell
# 导入openresty的 GPG 密钥：
wget -qO - https://openresty.org/package/pubkey.gpg | sudo apt-key add -

# 添加openresty官方 official APT 仓库：
sudo add-apt-repository -y "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main"

# 更新 APT 索引：
sudo apt-get update

# 安装openresty
sudo apt-get install openresty
```

##### 启动openresty

和启动nginx操作一样，不过命令由nginx变为了openresty

```shell
# 启动openresty
openresty

# 测试openresty配置文件(可以通过这个找到配置文件位置)
openresty -t
```

#### 重定向实现

找到openresty配置文件里的：

```nginx
location / {
    root   html;
    index  index.html index.htm;
}
```

改为：

```nginx
location / {
    content_by_lua_block {
        local redis = require "resty.redis"
        local red = redis:new()
        local request_uri = string.match(ngx.var.request_uri, ".*", 2)
        red:set_timeout(1000)
        local ok, err = red:connect("127.0.0.1", 6379)
        local res, err = red:get(request_uri)
        return ngx.redirect(res, 301)
    }
}
```

此处去掉了多余的异常检测等内容，详细内容请看[lua-nginx-module官方文档](https://github.com/openresty/lua-nginx-module)

重新加载配置文件

```shell
# 测试配置文件是否有问题
openresty -t
# 重新加载
openresty -s reload
```

此时访问[http://localhost](http://localhost)(服务器ip)，就会看到

![openresty_err](/images/openresty_err.png)

在redis中设置一个key-value

```
redis-cli -h localhost
localhost:6379> set dwz https://www.baidu.com
```

然后请求[http://localhost/dwz](http://localhost/dwz)，就会自动跳转到百度

### END

此处只实现了请求nginx后，在nginx层直接通过code（上面的`dwz`）在redis中找到跳转地址，然后301重定向到对应网址（也可使用302临时重定向）

短码code生成规则，可以自行选择方法，实现算法可以参考[短网址(short URL)系统的原理及其实现](https://hufangyun.com/2017/short-url/)

至此，大功告成

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>