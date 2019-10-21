---
title: php debug 工具 sdebug 安装
tags:
  - php
  - debug
  - sdebug
categories:
  - php
---


为 phpstorm 安装 debug 工具



> ps：不能 debug 的 IDE 那还叫 IDE 么

<!-- more -->

先下载并安装 sdebug



[https://github.com/mabu233/sdebug](https://github.com/mabu233/sdebug)

在 php.ini 下添加如下配置



```bash
zend_extension="xdebug.so"

xdebug.remote_autostart=1
xdebug.remote_enable=1
xdebug.remote_connect_back=0
;xdebug.cli_color=0
;xdebug.profiler_enable=0
xdebug.remote_handler=dbgp
xdebug.remote_mode=req

xdebug.remote_port=9999
xdebug.remote_host=127.0.0.1
xdebug.idekey=PHPSTORM
```



确保 php 被正确添加进 phpstorm，如下图：



![](http://img.i6miao.cn/phpstorm.png)



在按照如下设置，即可使用 debug 功能。

![](http://img.i6miao.cn/phpstormdebug.png)
