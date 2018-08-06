---
title: ubuntu 安装pip3
date: 2018-08-06 14:44:36
tags:
 - python
 - python3
---

### WHAT
在ubuntu上安装pip3

### WHY
在ubuntu上通过apt-get install python3-pip安装pip3报错，所以找了找如何在ubuntu上安装pip3

<!-- more -->

### HOW

#### 安装setuptools

```shell
wget --no-check-certificate  https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26

tar -zxvf setuptools-19.6.tar.gz

cd setuptools-19.6.tar.gz

python3 setup.py build

python3 setup.py install
```

#### 安装pip

```shell
wget --no-check-certificate  https://pypi.python.org/packages/source/p/pip/pip-8.0.2.tar.gz#md5=3a73c4188f8dbad6a1e6f6d44d117eeb

tar -zxvf pip-8.0.2.tar.gz

cd pip-8.0.2

python3 setup.py build

python3 setup.py install
```

### END

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>