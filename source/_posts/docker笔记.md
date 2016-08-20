title: docker笔记
targs:
    - docker
    - 虚拟化
---

`docker`是一个开源的应用引擎容器.可以看做一个沙箱,在里面的运行的程序与外界隔绝.并且可以
在你指定的配置环境中运行.听起来像是一个虚拟机,我一开始对他的概念也是一个轻量级的虚拟机.
虽然好像也没什么错,但是,`docker`有虚拟机程序`VMare`,`VirutalBox`之类的,最重要的就是它
不仅仅是虚拟化,还有一整套类似于`git`的管理模式.使得`docker`让人眼前一亮,轻巧,灵活,可以
从开发环境无缝部署到正式环境,拯救了不少运维的生命啊.:)

## 0x00 引言
`docker`已经宣布支持`windows`和`mac`,不过,`linux`才是最早支持的环境.而且各种资料也比
较多了,所以我是在一台`Linux Mint 18`的机器上,和一台`Centos 7`机器上搭建了`docker`.因
为这两个系统都已经在安装源上内置了`docker`.

二者安装有一些差别的, Mint:
```
sudo apt-get install docker.io
```
而CentOS上:
```
sudo yum install docker
```

安装之后,可以通过`docker version`查看安装信息:
```
fry@fry-linux-mint ~ $ docker version
Client:
 Version:      1.11.2
 API version:  1.23
 Go version:   go1.6.2
 Git commit:   b9f10c9
 Built:        Thu, 16 Jun 2016 21:17:51 +1200
 OS/Arch:      linux/amd64
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

最后一句提示无法连接到docker daemon,这是docker的守护进程,也就是docker的后台服务.docker
程序分为客户端和服务端.通常这二者是在同一台机器上,而启动daemon的方式也很简单:
```
sudo service docker start
```

## 0x01 镜像

**镜像** 是docker的可读层,可以看做是一个虚拟机的系统.docker的Docker Hub

获取镜像:
```
sudo docker pull REPOSITORY[:TAG]
```

搜索镜像:
```
sudo docker search REPOSITORY[:TAG]
```

查看镜像:
```
sudo docker images
```
输出:
```
fry@fry-linux-mint blog $ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test                latest              c96ffd3bfa54        14 minutes ago      126.4 MB
ubuntu              latest              f8d79ba03c00        8 days ago          126.4 MB
```
**IMAGE ID** 是一个镜像的唯一标识,**REPOSITORY** 是来源仓库,例如上图中的第二,是从ubuntu
仓库中pull下来的,第一个则是我自己根据`ununtu`新建的,所以你看到二者的 **SIZE** 是一样大
小的. **TAG** 是镜像的标签,通过 `REPOSITORY[:TAG]` 的形式,也可定位一个指定的镜像.但是,
这种形式与镜像是一个多对一的关系,如下:
```
ubuntu              14.04               f8d79ba03c00        8 days ago          126.4 MB
ubuntu              latest              f8d79ba03c00        8 days ago          126.4 MB
```
我将`ubuntu:latest`添加了一个标签`14.04`,这两个标签的ID是一样的,说明他们的指向的是同一
个镜像.我们可以通过下面的命令,做到这点:
```
sudo docker tag ubuntu:latest ubuntu:14.04
```

创建镜像,创建镜像有三种方式:
1. 基于已有镜像的容器创建
```
docker commit -m "提交信息" -a "创建者信息" 镜像id REPOSITORY[:TAG]
```
2. 基于本地模板导入
```
docker import
```
3. 基于Dockerfile创建
这个最灵活最受欢迎的方式.

## 0x02 容器
