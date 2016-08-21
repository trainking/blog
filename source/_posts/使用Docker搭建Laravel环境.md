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
境，作为我的生产环境的一个镜像。当你仅仅部署应用程序本身和它的依赖（像Nginx），与部署在传统的虚拟机上相比，安全和稳定的风险将大大降低。



原文地址：[http://dylanlindgren.com/docker-for-the-laravel-framework/](http://dylanlindgren.com/docker-for-the-laravel-framework/)
