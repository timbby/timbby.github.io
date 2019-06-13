---
title: git操作远程分支(git pull,git push等)时提示enter passphrase for key '~/.ssh/id_rsa'
date: 2019-06-13 10:36:57
tags:
 - git
 - passphrase
---

### WHAT

git操作远程分支(**git pull**,**git push**等)时提示`enter passphrase for key '~/.ssh/id_rsa'`

### WHY

之前有同事操作git时，拉代码需要输入密码，然后用ide拉代码就没有问题，当时就没太在意。但是后来又遇到了同样的问题，这次是在服务器上配置的id_rsa，出现了同样的问题，每次都要输入密码，于是就查了一下解决办法。

<!-- more -->

### HOW

原因应该是生成key的时候设置了密码，导致每次操作git都需要输入密码。然而很多小伙伴第一次生成key的时候都处于懵懵懂懂的状态（比如我），面对陌生的命令，莫名其妙就设置了密码，然后每次使用时都需要输入密码这个繁杂的操作。

解决办法很简单，就是输入以下命令：

```shell
$ ssh-keygen -p [-P old_passphrase] [-N new_passphrase] [-f keyfile]
```

举个例子：

```shell
$ ssh-keygen -p -P 123456 -N '' -f ~/.ssh/id_rsa
```

这样就把最开始无知的我们设置的'123456'密码改为了万能的''密码，然后就可以无痛使用git pull等命令，再也不用输入密码了。

### END

解决后发现，看起来很简单的问题，都来源于最初对工具的不熟悉。自己挖的坑还是要自己填~

解决办法来自于[stackoverflow](https://stackoverflow.com/questions/21095054/ssh-key-still-asking-for-password-and-passphrase#answer-34800766)

至此，大功告成

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>