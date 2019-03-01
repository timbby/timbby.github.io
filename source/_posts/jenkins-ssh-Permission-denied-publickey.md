---
title: jenkins 执行 ssh 命令报错Permission denied (publickey).
date: 2019-02-14 17:41:24
tags:
 - jenkins
 - ssh
 - ubuntu
---

### WHAT
jenkins执行带有ssh命令的任务，报错“Permission denied (publickey).”；

### WHY
手动迁移几个jenkins任务，结果有几个任务命令中带ssh，一直报错；

<!-- more -->

### HOW
#### 现象
- 两个不同环境的服务器小A和小B，都启动了jenkins服务，上面有一个同样的任务是“ssh登录到服务器小C上执行一个命令”；
- 原本在小A的jenkins上的任务可以正常执行；
- 但是小B上的任务一直报`Permission denied (publickey).`；
- 确认了下小A和小B的`~/.ssh/id_rsa.pub`都是一样的；
- 同时在小B服务器上直接`ssh root@小C "echo 'hello world'"`也是ok的
- google关键字[“jenkins ssh Permission denied (publickey).”](https://www.google.com/search?q=jenkins+ssh+Permission+denied+publickey.&oq=jenkins+ssh+Permission+denied+publickey.)找到了[这个问题](https://stackoverflow.com/questions/18130759/jenkins-cant-ssh-to-remote-server-key-permission-denied-but-works-from-cl#answer-18193633)中的这句话“I would suggest to use the -v argument to compare the 2 outputs:

”
- 于是将原本的命令`ssh root@小C "echo 'hello world'"`改成`ssh -v root@小C "echo 'hello world'`，将ssh的详细信息打印出来
- 最后几行输出为
```
	debug1: Next authentication method: publickey
	debug1: Trying private key: /var/lib/jenkins/.ssh/id_rsa
	debug1: Trying private key: /var/lib/jenkins/.ssh/id_dsa
	debug1: Trying private key: /var/lib/jenkins/.ssh/id_ecdsa
	debug1: Trying private key: /var/lib/jenkins/.ssh/id_ed25519
	debug1: No more authentication methods to try.
	Permission denied (publickey).
```
- 找到了最关键的目录`/var/lib/jenkins/.ssh/`
- 于是进去看了一眼，发现小A的目录下安安静静的躺着三个文件“authorized_keys  id_rsa  id_rsa.pub”
- 而小B的目录下空空如也
- 于是把小B的`~/.ssh/`复制到`/var/lib/jenkins/.ssh/`下
- 果然，jenkins任务成功执行了

至此，大功告成

### END

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>