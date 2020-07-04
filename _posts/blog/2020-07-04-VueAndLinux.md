---
layout: post
title: 如何将 Vue 项目发布到 Linux 系统运行
categories: [Vue]
description: Vue 通过 nginx 发布到 Linux运行
keywords: Vue，Linux
typora-root-url: ..\..
---

## linux 相关命令介绍

### 1.wget  相关

安装 yum -y install wget

使用  wget <http://place.your.url/here>

### 2.解压相关

 安装 yum install -y tar

  使用 tar  -zxvf  xxxx.tar.gz

  安装    yum install -y unzip

  使用解压到指定文件夹  unzip dist.zip  -d  /home/www

### 3. vi 相关命令

i 编辑状态

Esc 退出编辑状态

:q!  退出不保存

:qw 退出保存

### 4.基本操作命令

进入目录 cd www

给目录赋权  chmod -R 777 /home/www/

创建文件夹      mkdir  www

删除文件夹及其子目录  rm -rf  www

## node.js环境 搭建

### 安装 nvm 版本管理器

安装 nvm 管理 node

安装 nvm软件

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh|bash

关闭窗口再打开，生效

输入 nvm 验证是否安装成功

### 使用 nvm 安装 Node.js

#### 1) 安装指定版本的 Node.js

我们可以通过以下 nvm 命令在线安装指定版本的 Node.js

```
nvm install [nodeversion]
```

例如，需要安装 v6.9.1 版本的 Node.js，那可以通过以下命令完成。

```
nvm install v6.9.1
```

#### 2）指定当前使用的 Node.js 版本

通过 nvm 可以同时安装多个版本的 Node.js，我们可以指定某个版本的使用

```
nvm use [nodeversion]
```

例如，需要使用 v6.9.1 版本的 Node.js，那可以通过以下命令完成。

```
nvm use v6.9.1
```

#### 3) 查看当前安装的 Node.js 版本列表

```
nvm ls
```

#### 4) nvm 的其他命令

nvm 还提供一些命令，方便我们平时管理 Node.js 的版本。

nvm uninstall [nodeversion]: 表示删除指定版本的 Node.js，用法类似于 install 命令。

nvm current: 表示显示当前使用的 Node.js 版本。

nvm reinstall-packages [npmversion]: 表示在当前的 Node.js 版本下，安装指定版本的 npm 包管理器。

#### 5) 验证是否安装成功

```
node -v
```

##  CentOS 下 nginx 环境 搭建

####  第一步 安装 yum-utils

```
sudo yum install yum-utils
```

####  第二步 创建 配置文件并设置内容

创建

```
vi /etc/yum.repos.d/nginx.repo
```

内容

```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

#### 第三 执行安装命令

```
sudo yum install nginx
```

#### 第四 进入 nginx 配置文件查看 配置

```
cd /etc/nginx/conf.d

vi default.conf
```

#### 第五 修改后重置 nginx

```
nginx -s reload
```

#### 第六 nginx 相关命令介绍

```
1、查看nginx进程 

ps -ef|grep nginx

说明：nginx的进程由主进程和工作进程组成。

2、启动nginx

nginx

启动结果显示nginx的主线程和工作线程，工作线程的数量跟nginx.conf中的配置参数worker_processes有关。 

3、平滑启动nginx 

kill -HUP `cat /var/run/nginx.pid`  

或者 

nginx -s reload

其中进程文件路径在配置文件nginx.conf中可以找到。

平滑启动的意思是在不停止nginx的情况下，重启nginx，重新加载配置文件，启动新的工作线程，完美停止旧的工作线程。

4、完美停止nginx 

kill -QUIT `cat /var/run/nginx.pid`

5、快速停止nginx 

kill -TERM `cat /var/run/nginx.pid`

或者

kill -INT `cat /var/run/nginx.pid`

6、完美停止工作进程（主要用于平滑升级） 

kill -WINCH `cat /var/run/nginx.pid`

7、强制停止nginx 

pkill -9 nginx

8、检查对nginx.conf文件的修改是否正确 

nginx -t -c /etc/nginx/nginx.conf 或者 nginx -t

9、停止nginx的命令 

nginx -s stop

10、查看nginx的版本信息

nginx -v

11、查看完整的nginx的配置信息 

nginx -V
```

## vue 发布 linux 流程

#### I.生成发布档

```
npm run build
```

编译完成后产生一个 dist 文件夹

#### II.将打包上传到 Linux 服务器的 代码解压缩到指定文件夹

```
unzip dist.zip  -d  /home/www
```

#### III.编辑 nginx 配置文件，让其指向发布档

指向 80端口，目录地址为 /home/www/dist  默认页为 index.html

```
server {
    listen       80;
    server_name  localhost;
    location / {
        root   /home/www/dist;
        index  index.html ;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
    }

```

#### IV.重置 nginx

```
nginx -s reload
```

#### V.访问服务器地址

