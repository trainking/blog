title: 使用Docker搭建Laravel环境
tags:
    - '译文'
-----

现在有很多关于Docker的争吵，这是正常的。因为Docker带来了一个重大飞跃，app的集装
箱化活用，将搭建例如google和facebook那样强大的数据中心的能力带到了普通的开发者
或系统管理员的手中。

Docker使用源生Linux内核集装成一个进程，类似于将它们放入一个独立的小仓库里面，使
得它们不会影响到系统的其他进程。Docker常被用于部署，管理，和自动化构建容器。集装
箱化，带来和虚拟机相似等级的隔离，却只耗费运行虚拟机管理程序的10%-15%的性能。看
下面这部Docker创始人兼Docker的CTO的访谈，可以帮助你快速了解Docker。（油管链接
，我就不贴了）。如果你想要了解更多背景，你可以访问Dokcer的官网。

我开发的大多数应用程序都是基于Laravel，所以我想看看使用Docker搭建一个本地开发环
境，作为我的生产环境的一个镜像。当你仅仅部署应用程序本身和它的依赖（像Nginx），与部署在
传统的虚拟机上相比，安全和稳定的风险将大大降低。而你仍然可以节省从开发环境到生产环境的时
间.

网上很少文章是结合Larvel和Docker来讲的.所有我们阅读到的要使用Vagrant的概念,都可以直接
使用Dokcer,本文中所有的进程将运行在一个容器中.因为一些原因,这意味着你将错过了一些Dokcer
的真正优点.我们想要一个进程运行在一个容器中,我们将每个容器(即进程)链接一个数据容器上,所有
的应用程序文件都保存在这个数据容器上.让我们开始吧.

## 准备你的开发环境
~~Docker使用集装箱化技术专用于Linux, 所以如果你使用OS X或者Windows,则需要虚拟机.非linux的Docker包被称之为Boot2Docker.~~
(注:Docker已经宣布支持OS X和Windows了)

关于不同系统版本的安装方式,参见Dokcer的官方文档.***[Docker documentation](https://docs.docker.com/engine/installation/#installation)***

## 概述
获取和启动运行Lavavel应用程序,我们不仅仅需要一个可以运行PHP的web服务器,我们也
需要能够运行PHP命令行应用程序的**composer**和**artisan**.也许还有更好运行环境
的方式(例如浏览器).但是这是一个好的基础整合Dokcer和Laravel.每个进程都有一个自
己的容器.

下面列出了我们要使用Docker镜像:

+ [dylanlindgren/docker-laravel-data](https://github.com/dylanlindgren/doc
ker-laravel-data) - 这个镜像是用来创建我们的数据容器的.这个容器将提供我们的应
用程序文件给其他容器访问.

+ [dylanlindgren/docker-laravel-composer](https://github.com/dylanlindgren/docker-laravel-composer) - 这个镜像将创建一个允许我们使用`composer`命令行的
容器.

+ [dylanlindgren/docker-laravel-artisan](https://github.com/dylanlindgren/docker-laravel-artisan) - 这个镜像将创建一个允许我们使用`artisan`命令行的容器.

+ [dylanlindgren/docker-laravel-phpfpm](https://github.com/dylanlindgren/docker-laravel-phpfpm) - PHP-FPM处理PHP文件.

+ [dylanlindgren/docker-laravel-nginx](https://github.com/dylanlindgren/docker-laravel-nginx) - 一个Nginx服务器.这个容器将链接到我们的PHP-FPM容器上.

+ [dylanlindgren/docker-laravel-bower](https://github.com/dylanlindgren/docker-laravel-bower) - 浏览器.(可有可不有)

有分离的`composer`和`artisan`容器对我们来说真是一大优势,我们可以只选择推送
`docker-laravel-data`,`docker-laravel-nginx`和`docker-laravel-phpfpm`容器
到生产环境.

我画了一副流程图可视化帮助理解,沿着获取数据方向理解如何整合这些容器.

![流程图](http://7xpgwr.com1.z0.glb.clouddn.com/900b8f8c-4397-11e4-943a-2a723cff17fe.png)

你可以看到这些容器都将他们的`/data`定位到了`docker-laravel-data`容器.又可以
看到是从主目录的`~/myapp`目录定向到了`/data`.在`~/myapp`中我们有两个目录
`www`和`logs`.

+ `www` - 包含我们的应用程序文件(例如:`public/index.php`)
+ `logs` - 存放Nginx的错误日志文件.

从Docker Hub上拉取这些镜像非常容易,只需要在命令行(或boot2docker虚拟机)中运行
下面的命令:

```
$ docker pull dylanlindgren/docker-laravel-data && \
  docker pull dylanlindgren/docker-laravel-composer && \
  docker pull dylanlindgren/docker-laravel-artisan && \
  docker pull dylanlindgren/docker-laravel-phpfpm && \
  docker pull dylanlindgren/docker-laravel-nginx && \
  docker pull dylanlindgren/docker-laravel-bower
```

这些镜像也可以通过获取上面GitHub上的Dockerfile,使用`docker build`命令来构建.
不过那已经是本教程之外的内容了.

## Docker&Laravel实践
我使用2013年的MackBook Pro开发,因此下面说明都是针对`OS X`环境的.应该很容易改
变一些路径,就可以用于了`Linux`和`Windows`了.

### 创建数据容器
创建两个目录在你的系统中`~/myapp/www`和`~/myapp/logs`.这个`~/myapp/`目录映射
到数据容器中,提供给其他容器访问应用程序文件.

如果你已经有了一Laravel app,将它的所有文件放入`~/myapp/www`目录中.否则我们将
创建一个.

让我们来创建我们的数据容器,并且将`~/myapp/`映射到这个容器的`/data`目录上.

```
$ docker run --name myapp-data -v /Users/dylan/myapp:/data:rw dylanlindgren/docker-laravel-data
```

### 运行composer命令行
通过如下的命令执行`composer`命令行:

```
$ docker run --privileged=true --volumes-from myapp-data --rm dylanlindgren/docker-laravel-composer
*your composer commands here*  
```

哇!这真是一个好长的命令.谁想输入这么长的命令只是为了简单的运行`composer dump-autoload`?可以灵活的使用Bash的aliases使命令简短.在`.bashrc`文件中添加下面的代码.

```
alias myapp-composer="docker run --privileged=true --volumes-from myapp-data --rm dylanlindgren/docker-laravel-composer"
```

重新启动终端的会话.你就可以通过`myapp-composer`命令行来运行`composer`了.

如果你想要创建一个新的Laravel app,执行下面的命令,使用`composer`下载Laravel和它的依赖:

```
myapp-composer create-project laravel/laravel /data/www --prefer-dist
```

不要忘记适当的给`app/storage`目录权限,不然你可能会看到运行错误.

### 运行artisan命令行
`artisan`命令行的运行方式和运行`composer`命令行的方法相似:

```
$ docker run --privileged=true --volumes-from myapp-data --rm dylanlindgren/docker-laravel-artisan
*your artisan commands here*
```

同样的,让我们增加另一条配置到`.bashrc`中:

```
alias myapp-artisan="docker run --privileged=true --volumes-from myapp-data --rm dylanlindgren/docker-laravel-artisan"
```

重启终端,你就可以通过`myapp-artisan`来运行`artisan`了.

### Laravel服务
`Nginx`和`PHP-FPM`是分开的两个进程,因此,我们将分开它们不同容器的原因在这之前解释清楚.

首先,我们先创建`PHP-FPM`的容器,注意这里我们使用`-d`选项,说明进程将在后台运行.`PHP-FPM`不是运行不成功,以为是像`composer`和`artisan`那样退出了,其实它只是在保持运行.我们要把它当做守护进程那样运行,这样我们就可以做其他的事情,例如加载Nginx的容器.

如下运行PHP-FPM:

```
$ docker run --privileged=true --name myapp-php --volumes-from myapp-data -d dylanlindgren/docker-laravel-phpfpm  
```

我们使用`--link`选项,当我们创建Nginx容器的时候,链接到这个PHP-FPM容器,通过它们的ip来沟通数据.(准确来说是9000端口)

让我们运行Nginx:

```
$ docker run --privileged=true --name myapp-web --volumes-from myapp-data -p 80:80 --link myapp-php:fpm -d dylanlindgren/docker-laravel-nginx  
```

最后通过浏览器访问`http://localhost`就可以看见Laravel的欢迎界面.

![欢迎界面](http://7xpgwr.com1.z0.glb.clouddn.com/594f430c-43a7-11e4-8e77-0f2f773ae813.png)

## 总结

希望通过运行本教程的案例,不仅仅只看到了Dokcer,更应该明白Laravel开发是一个巨大的游戏规则.

原文地址：[http://dylanlindgren.com/docker-for-the-laravel-framework/](http://dylanlindgren.com/docker-for-the-laravel-framework/)
