---
title: 使用阿里云，部署HEXO博客
date: 2018-11-07 11:37:43
tags:
  - 阿里云
  - 博客
categories:
  - 运维
---

## 使用阿里云，部署HEXO博客

> hexo初始化请看这里

在阿里云上如果要使用 hexo -d生成静态文件，git仓库是最合适的，在触发git hook，最后再执行bash命令将文件拷贝到博客网站目录。

<!-- more -->

### 创建git仓库

下载git并安装不必多说

1. 云服务器上创建用户 git：

`adduser git`

2. 在git家目录/home/git/*下创建目录.ssh 并创建authorized_keys文件：

``` bash
mkdir /home/git/.ssh/
vim /home/git/.ssh/authorized_keys
```

3. 修改目录属主和权限：

``` bash
chown -R git:git /home/git/.ssh/
chmod 700 /home/git/.ssh
chmod 600 /home/git/.ssh/authorized_keys
```

4. 在/srv/ 下创建裸仓 sample.git :

``` bash
 mkdir /var/git/sample.git
 chown -R git:git /srv/sample.git
 git init --bare sample.git //本条命令在 /srv 下输入
 ```

 5. 禁止git 的 shell 登陆：

文件 /etc/passwd 下有类似一行

`git:x:1001:1001:,,,:/home/git:/bin/bash`

修改为：

`git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`

执行su git ,若报如下错，

``` bash
fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
```

则进行这步，

将目录/usr/share/doc/git/contrib/git-shell-commands拷贝到git用户下/home/git/

`修改所有者chown -R git.git git-shell-commands `

如果该目录下的help和list没有执行权，那么再给它加执行权

``` bash
chmod +x /home/git/git-shell-commands/help
chmod +x /home/git/git-shell-commands/list
```

### 创建仓库

在阿里云服务器上创建git仓库，注意不要漏掉--bare参数。

``` bash
mkdir blog.git && cd blog.git
git init --bare
```

### Hexo配置
修改本地博客目录下的_config.yml配置，其中xx.xxx.xx.xxx是你的服务器ip地址，/www/blog.git是你上一步创建的git仓库路径，master是分支。

``` bash
deploy:
  type: git
  message: update
  repo: git@xx.xxx.xx.xxx:/var/git/blog.git,master
```

### 插件安装

此插件的作用是执行deploy时，将hexo生成的静态文件提交到_config.yml配置中的deploy.repo地址，即 git@xx.xxx.xx.xxx:/www/blog.git,master。

`npm install hexo-deployer-git --save`

### 自动部署

本地的deploy命令只是把静态文件提交到git仓库，既然有git hooks，那么我们可以在有文件提交上来时，再将文件拷贝到博客网站目录。
进入到git仓库hooks目录，并创建钩子post-receive。

``` bash
cd /www/blog.git/hooks
touch post-receive
vim post-receive
```

然后输入下面脚本：

``` bash
#!/bin/bash -l
GIT_REPO=/var/git/blog.git
TMP_GIT_CLONE=/www/tmp/blog
PUBLIC_WWW=/www/blog
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```
其中/var/git/blog.git为仓库路径，/www/blog为你的博客网站路径，/www/tmp/blog是临时目录，git会先将文件拉到临时目录，然后再将所有文件拷贝到博客网站目录/www/blog。

更改目录权限：

``` bash
chmod +x post-receive
chmod 777 -R /www/blog
```
上面涉及到的文件夹都要改权限。
