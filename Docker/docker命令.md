---
title: docker运行的流程
date: 2022-12-14 19:36:36
tags: "docker"
categories: "docker"
updated: 2022-12-14
description: "docker容器和镜像命令"
comments: true
swiper_index: 9
---
## docker运行的流程

run的流程

![image-20221214163405653](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20221214163405653.png)

docker和vm比较

![image-20221214163609415](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20221214163609415.png)

1. docker有着比虚拟机更少的抽象层
2. docker利用的是宿主机的内核，vm需要的是Guest OS
3. 所以说，新建一个容器的时候，docker不需要想虚拟机一样重新加载一个操作系统内核，避免引导。虚拟机是加载Guest OS，分钟级别的，而docker是利用宿主机的操作系统，省略了这个复杂的过程，秒级!

## 镜像的命令

帮助命令

```shell
docker version  #版本信息
docker info   #系统信息
docker 命令 --help    #帮助文档
```

帮助文档的地址

```shell
docker images  #查看镜像

[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        latest    83a5aeccc5e0   36 hours ago    117MB
tomcat       latest    1dad8c5497f9   4 days ago      474MB
nginx        latest    ac8efec875ce   8 days ago      142MB
mysql        5.6       dd3b2a5dcb48   11 months ago   303MB

# 解释
REPOSITORY 镜像的仓库源
TAG   # 标签
IMAGE ID  # id
REATED #时间
SIZE #大小
```

```shell
[root@localhost ~]# docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        latest    83a5aeccc5e0   36 hours ago    117MB
tomcat       latest    1dad8c5497f9   4 days ago      474MB
nginx        latest    ac8efec875ce   8 days ago      142MB
mysql        5.6       dd3b2a5dcb48   11 months ago   303MB
[root@localhost ~]# docker images -q
83a5aeccc5e0
1dad8c5497f9
ac8efec875ce
dd3b2a5dcb48

```

```
docker seach 搜索

--filter 过滤

docker pull 镜像 #下载镜像
默认最新
也可以指定镜像版本

docker rmi -f id # 删除镜像

docker rmi -f $(docker images -aq)
递归删除---骚操作 删除全部
```

## 容器命令

我们有了镜像才可以创建容器,下载一个centos来测试学习

```
docker pull centos
```

新建并且启动

```shell
docker run 可选参数 image

# 参数说明
--name = "Name" 容器名字 用来区分容器
-d  后台方式运行，jar nohup
-it 使用交互方式进行，进入容器查看内容
-p  指定容器的端口 -p 8080:8080
	-p 主机端口：容器端口
-P   随机指定端口

```

```shell
[root@CEHNYI signal]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   14 months ago   13.3kB
centos        latest    5d0da3dc9764   15 months ago   231MB
[root@CEHNYI signal]# docker run -it centos /bin/bash
[root@446ca1885d2d /]# ls
bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
```

- doker ps ：当前正在运行的容器
- docker ps -a ：显示最近创建的容器

小结：

>run 是运行镜像 ，如果本地没有就会去dockerhub中拉取最新的镜像，通过docker ps可以查看正在运行的容器，docker ps -a查看最近运行的容器，可以通过docker start 容器id/容器名 获取开启容器

```shell
停止容器:
docker stop hello1
启动已经停止容器:
docker start hello1
进入容器:
docker exec -it hello1 /bin/bash
删除容器:
docker rm -f hello1
帮助命令:
docker --help
```

**执行过程：**

>1.拉取一个镜像直接用pull
>
>2.run 镜像：如果本地有镜像就不用到dockerhub上下载镜像。直接启动本地的镜像即可
>
>3.doker ps 查看当前已经运行的容器  
>
>4.docker ps -a 查看之前运行过的所有容器
>
>5.通过 **docker run  - - name = 所取的容器名字   -it  centos /bin/bash**创建交互式容器,可以指定name开创建多个交互性容器，就是说一个centos镜像中可以创建很多个容器来运行
>
>```shell
># 创建并且运行 cy01
>[root@localhost ~]# docker run --name=cy01 -it centos /bin/bash
>[root@2c5d283f9ef6 /]# ls
>bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
>dev  home  lib64  media       opt  root  sbin  sys  usr
>[root@2c5d283f9ef6 /]# exit
>exit
># 开启cy01
>[root@localhost ~]# docker start cy01
>cy01
># 进入cy01
>[root@localhost ~]# docker exec -it cy01 bash
>[root@2c5d283f9ef6 /]# ls
>bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
>dev  home  lib64  media       opt  root  sbin  sys  usr
>[root@2c5d283f9ef6 /]# exit
>exit
># 创建并且运行 cy02
>[root@localhost ~]# docker run --name=cy02 -it centos /bin/bash
>[root@e3513bc87bfe /]# ls 
>bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
>dev  home  lib64  media       opt  root  sbin  sys  usr
>[root@e3513bc87bfe /]# exit
>exit
># 开启cy02
>[root@localhost ~]# docker start cy02
>cy02
># 进入cy01
>[root@localhost ~]# docker exec -it cy01 bash
>[root@2c5d283f9ef6 /]# ls
>bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
>dev  home  lib64  media       opt  root  sbin  sys  usr
>[root@2c5d283f9ef6 /]# exit
>exit
>
>```
>
>![image-20221214174936483](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20221214174936483.png)
>
>6.exit 退出运行状态  后台不在运行
>
>7.通过  docker start  容器id或者容器名（注意：不是镜像名 是自己起的容器名） 开启容器 
>
>8.docker ps 可以看到运行状态新鲜
>
>9.通过 docker exec -it 容器名 /bin/bash 进入容器  同样exit 退出 但是后台仍在运行容器
>
>10.通过docker stop 容器名或id 关闭容器 后台就不在运行了
>
>11.若不用该容器或者镜像 用 docker rmi -f 镜像id或名   强制删除该镜像   docker rm -f 删除该容器
>
>docker ps -a | xargs docker rm

