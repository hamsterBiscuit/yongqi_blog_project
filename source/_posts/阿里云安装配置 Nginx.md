---
title: 阿里云安装配置 Nginx
date: 2018-11-07 11:37:43
tags:
  - 阿里云 nginx
categories:
  - 运维
---

> 转载自 [如何在阿里云Centos下安装Nginx](https://www.aliyun.com/jiaocheng/152926.html)

Nginx("engine x")是一款轻量级的HTTP和反向代理服务器。相比于Apache、lighttpd等,它具有占有内存少、并发能力强、稳定性高等优势。它最常见的用途就是提供反向代理服务。

在Linux下我们需要下载Nginx的源代码包并且手动编译,而不是用包管理工具,例如Yum、Aptitude来安装。因为我们需要在编译时对Nginx进行配置,不得不手动编译,这样也就会依赖一些工具和库文件。

首先,需要安装C语言的编译环境,因为Nginx是C语言编写的。通常大多数Linux都会默认安装GCC,如果没有的话,可以如下安装。

<!-- more -->

**安装make:**

` yum -y install gcc automake autoconf libtool make `

**安装g++:**

` yum install gcc gcc-c++ `

**PCRE库:**

Nginx 需要 PCRE(Perl Compatible Regular Expression),因为 Nginx 的 Rewrite 模块和 Http 核心模块都会使用到 PCRE 正则表达式语法。其下载地址为 http://www.pcre.org/, 我们也可以通过yum来安装。

` yum install pcre pcre-devel `

**zlib库:**

zlib库提供了压缩算法,Nginx很多地方都会用到gzip算法。其下载地址为 http://www.zlib.net/, 也可以通过yum安装。

` yum install zlib zlib-devel `

**OpenSSL:**

Nginx中如果服务器提供安全页面,就需要用到 OpenSSL 库。其下载地址为 http://www.openssl.org/, 也可以通过yum安装。

` yum install openssl openssl-devel `

**下载Nginx:**

Nginx源代码包可以从官方网站下载http://nginx.org/en/download.html,目前最新稳定版本为1.10.1,还有开发版本可供选择。相关命令如下:

`wget https://nginx.org/download/nginx-1.10.1.tar.gz`

`tar zxf nginx-1.10.1.tar.gz`

`cd nginx-1.10.1/`

**安装Nginx:**

在安装之前需要进行配置,这也是linux下安装软件的常见步骤。初次安装可以直接使用configure脚本,如果有需要可以设置开关选项开启需要的功能模块,这里就不展开了。相关命令如下:

`./configure`

`make`

`make install`

**运行Nginx:**

Nginx会默认安装在/usr/local/nginx目录,我们cd到/usr/local/nginx/sbin/目录,存在一个Nginx二进制可执行文件。直接运行就可以启动Nginx。运行成功后打开浏览器访问此机器的IP.

`./nginx`

如果是阿里云，要去ECS实例一下打开端口

步骤如下：

- 进入阿里云控制台，并找到ECS实例。
- 点击实例后边的“更多”
- 点击“网络和安全组” ，再点击“安全组配置”
- 右上角添加“安全组配置”
- 进行80端口的设置，具体设置如图就好

Nginx相关命令:

`nginx -h` -------------------------> 帮助命令

`nginx -s stop `-------------------------> 立即停止守护进程(TERM信号)

`nginx -s quit` -------------------------> 温和的停止守护进程(QUIT信号)

`nginx -s reopen `-------------------------> 重新打开日志文件

`nginx -s reload` -------------------------> 重新载入配置文件

`nginx -t` -------------------------> 测试配置文件是否合法

`killall nginx` -------------------------> 强行终止Nginx进程

由于任何nginx命令都是检查配置文件是否合法,如果配置文件不合法,命令不会执行,killall命令可以避免无法停止Nginx服务。

Nginx配置文件有自己独特的语法,在这里就不展开了。
