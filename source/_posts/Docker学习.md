---
title: Docker学习
date: 2018-05-27 18:01:27
tags:
---

#### 在您的 Docker 主机的命令行界面，首先您需要完成登录，输入您的 DaoCloud ID 和密码即可完成
```
sudo docker login daocloud.io
Username:<DaoCloud 用户名>
Password:<DaoCloud 密码>
Email:<注册时用的电子邮件地址>
Login Successed
```

#### 我不用官方的dockhub了，转而使用国内的仓库daocloud
查看加速器地址: https://www.daocloud.io/mirror.html#accelerator-doc
```
$ echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://5001b0f0.m.daocloud.io\"" | sudo tee -a /etc/default/docker
$ sudo service docker restart


# Docker 版本在 1.12 或更高

# 创建或修改 /etc/docker/daemon.json 文件，
# 修改为如下形式 （请将 加速地址 替换为在加速器页面获取的专属地址）

{
    "registry-mirrors": [
        "加速地址"
    ],
    "insecure-registries": []
}
```




#### 打包
>docker build -f "Dockerfile"  -t static_web . 最后有一个点


sudo docker run -i -t anapsix/alpine-java /bin/bash

#### alpine 安装openssh
```
ole@T:~$ docker run -it --rm alpine /bin/ash
    / # apk update
    fetch http://dl-4.alpinelinux.org/alpine/v3.3/main/x86_64/APKINDEX.tar.gz
    fetch http://dl-4.alpinelinux.org/alpine/v3.3/community/x86_64/APKINDEX.tar.gz
    v3.3.1-97-g109077d [http://dl-4.alpinelinux.org/alpine/v3.3/main]
    v3.3.1-59-g48b0368 [http://dl-4.alpinelinux.org/alpine/v3.3/community]
    OK: 5853 distinct packages available
    / # apk add openssh
    (1/3) Installing openssh-client (7.1_p2-r0)
    (2/3) Installing openssh-sftp-server (7.1_p2-r0)
    (3/3) Installing openssh (7.1_p2-r0)
    Executing busybox-1.24.1-r7.trigger
    OK: 8 MiB in 14 packages
```


#### 运行Docker容器并使用SSH管理

```
docker run -i -t centos /bin/bash  #启动一个Docker容器，并进入到bash中

yum -y install openssh-server      #安装sshd

ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key

ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key 

ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ""

/usr/sbin/sshd -D               #后台启动sshd(whereis、which 查找执行命令)

 vi /etc/ssh/sshd_config  #将UsePAM yes改成no,否则ssh登陆到容器时会马上退出

yum -y install passwd              #安装passwd 命令

passwd root                        #修改root用户密码，用于ssh登录名

 docker commit containerid imagename   #containerid:容器的id,imagename:提交时候镜像的名称

docker run -d -p 10022:22 imagename /usr/sbin/sshd -D #参数-d表示后台运行，-p表示docker到主机的端口的映射

ssh root@localhost -p 10022

使用SecureCRT 用10222端口登陆容器，就可以管理了。
```