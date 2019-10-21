---
title: Mac 下 php hyperf 安装记录
tags:
  - php
  - hyperf
categories:
  - php
---

## Mac 下 php hyperf 安装记录



> 学习 php，记录安装遇到的问题



### 通过 brew 安装 php

`  brew install php`

这里没有遇到什么问题

<!-- more -->

### 安装 swoole > 4.0.4



`pecl install swoole`



这里会出现 `error "Enable openssl support, require openssl library.`



或者 openssl library not found,nghttp2 library not found



![img](https://image-static.segmentfault.com/236/409/2364092446-5d725702780af)



https://segmentfault.com/a/1190000020314381

具体改动为

安装 openssl 在后指定 openssl 目录

`--with-openssl-dir=/usr/local/Cellar/openssl/1.0.2s/`  即可解决



![img](https://image-static.segmentfault.com/510/148/510148272-5d72569d8c501)





安装完成后，报 warning：



```
Warning: mkdir(): File exists in /usr/local/Cellar/php/7.3.5/share/php/pear/System.php on line 294
ERROR: failed to mkdir /usr/local/Cellar/php/7.3.5/pecl/20180731`
```



是因为没有这个目标目录导致这个问题，手动创建目录解决问题。



`mkdir -p /usr/local/lib/php/pecl`



自此，安装完成。
