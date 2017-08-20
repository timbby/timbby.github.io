---
title: 批量设置redis-key的过期时间
date: 2017-08-20 12:22:17
tags:
 - redis
 - ubuntu
---

### WHAT
  批量的给redis中***同样前缀***的key设置失效时间。

### WHY
  由于之前没有考虑到数据量的问题，在redis中建了大量永久的key，早上发现redis内存满了，一对定时任务挂掉了，于是打算给这些key设置失效时间.

<!-- more -->

### HOW（使用xargs）
  之前有批量删除key的方法：
  ```shell
  redis-cli -h 127.0.0.1 keys "pre:key_*" | xargs redis-cli -h 127.0.0.1 del
  ```
  打算直接用上述命令，结果发现xargs是把"|"前的标准输出接到后面的标准输入后面然后执行的，而redis的expire需要把在key后面加上时间参数。

  于是便去找xargs的使用方法，找到了"-i"命令，
  ```shell
  -i, --replace[=R]   replace R in INITIAL-ARGS with names read from standard input; if R is unspecified, assume {}
  ```
  加上"-i"参数，可以用"{}"代替"|"前面的标准输出，
  ```shell
  timbby@timbby:~$ echo "world" | xargs -i echo "hello "{}", end."
  hello world, end.
  ```
  因此最终解决办法就是：
  ```shell
  redis-cli -h 127.0.0.1 keys "pre:key_*" | xargs -i redis-cli -h 127.0.0.1 expire {} 86400
  ```
### END
  命令执行后会有满屏的"(integer) 1"，→_→ ...
