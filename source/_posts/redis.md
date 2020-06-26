---
title: redis
date: 2020-05-02 17:11:45
tags: redis 
---

# linux下的redis开机自动启动

## 一、安装redis

首先安装好redis，具体安装过程可以参考菜鸟教程中linux安装步骤

https://www.runoob.com/redis/redis-install.html

安装好之后，需要改动到redis安装目录下的redis.conf文件内容，使用vim redis.conf打开之后首先修改第69行的bind 127.0.0.1 为自己linux电脑的ip地址。然后将第90行的protected-mode yes 设置为no,还有将139行的daemonize no改为yes.如果需要修改端口号的话，将第94行的port 6379修改为要使用的端口号。

| bind               | 修改为自己的ip地址，方便外部访问，若不修改，只能本机访问。   |
| ------------------ | ------------------------------------------------------------ |
| **protected-mode** | **是否开启保护模式，开启后需要通过配置密码等保护操作。**     |
| **daemonize**      | **配置是否允许后台运行，这步必须改为yes,否则无法做到开机自启。** |
| **port**           | **配置端口号，有默认端口号，可以不改。**                     |

上述这些配置完成之后，可以先通过linux的客户端先启动 redis,看是否成功。启动和连接命令

redis-service  和 redis -cli,可以连接上就开始配置开机自动启动redis操作了。

## 二、配置开机自动启动redis

首先在redis的安装目录下找到utils文件夹，将里面的redis_init_script拷贝到/etc/init.d文件下。mv redis_init_script /etc/init.d/redis  移动过去并重新命名为redis文件。利用vim修改参数，在#! /bin/sh 下添加一条 #chkconfig: 2345 80 90然后根据自己的安装路径修改以下的实际路径

**PEDISPORT=6379  #端口**

**EXEC=/usr/local/java/redis/bin/redis-server  #启动服务的命令路径（我的真实安装路径为 /home/sell/redis-5.0.8/src/redis-server,将真实路径替换上去即可。）**

**CLIEXEC=/usr/local/java/redis/bin/redis-cli #客户端路径 <u>(它和redis-server安装位置通常都在一个地方)</u>**

**PIDFILE=/var/run/redis_${REDISPORT}.pid #记录进程id的文件路径(这个只要在redis.conf文件中没有单独配置过，这里就不用改)**

**CONF=/usr/lcoal/java/redis/bing/redis.conf #配置文件的路径(这个在redis安装目录下就有redis.conf文件，修改为自己的安装路径)**

加入服务输入chkconfig --add redis命令

启动redis 服务 service redis start ,可用ps -ef |grep redis  查看是否启功成功，最后通过重启查看是否实现开机自启。



