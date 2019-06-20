---
title: 批量删除jenkins的构建历史
date: 2019-06-20 17:01:18
tags:
 - jenkins
 - shell
---

### WHAT

通过shell脚本，批量删除jenkins的构建历史。

### WHY

jenkins每构建一个任务，就会生成一个构建历史的文件夹，里面包含每次构建的日志。所以jenkins跑的时间久了，定时任务的构建历史就会有一大堆文件。文件倒是不大，但是服务器的inode空间是有限的，结果就会导致inode空间不足。所以就写了个shell脚本删除jenkins的构建历史。

<!-- more -->

### HOW

##### 首先，找到jenkins的系统配置：

jenkins中的任意视图比如([All](http://localhost:8080/)) -> 左侧的[系统管理](http://localhost:8080/manage) -> [系统设置](http://localhost:8080/configure)

然后就可以看到主目录：`/var/lib/jenkins`

##### 然后，根据[这个链接](https://www.cnblogs.com/kaituorensheng/archive/2012/12/19/2825376.html)找到了循环文件的方法：

```shell
# 循环当前目录下所有文件，并输出
for dir in ./*
do
  echo $dir
done
```

##### 最后，到jenkins的主目录下的jobs中，循环每个job的builds文件夹，删掉所有内容

```shell
cd /var/lib/jenkins/jobs/
for dir in ./*
do
  cd /var/lib/jenkins/jobs/
  # "$dir" 加双引号 防止路径中有空格导致失效
  cd "$dir"
  cd builds
  echo $(pwd)
  rm -rf *
done
```

然后静静的等待就好了（文件较多会删的比较慢）

### END

方法很简单，主要难点在于shell的循环语法，还有`cd "$dir"`，防止目录名存在空格。还有写脚本过程中，最好先把循环中最终cd到的目录打印下来，然后执行一遍，确认目录无误后，再执行`rm -rf *`，防止误删。

至此，大功告成

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>