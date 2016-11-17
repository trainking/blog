title: Centos下搭建nginx+php
tags:
    - '运维'
-----

# nginx

## 先下载pcre包

```
> wget ftp://ftp.csx.cam.ac.uk//pub/software/programming/pcre/pcre-8.38.tar.gz
> tar -zxvf pcre-8.38.tar.gz
```

## 编译nginx
```
> wget http://nginx.org/download/nginx-1.10.2.tar.gz
> cd nginx-1.10.2
> ./configure --prefix=/usr/local/nginx --with-pcre=../pcre-8.38/
> make
> sudo make install
```

## 启动nginx

```
> sudo ./nginx
> sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
> sudo /etc/rc.d/init.d/iptables save
> sudo /etc/init.d/iptables restart
```

# PHP

## 下载PHP包
```
> wget http://hk1.php.net/get/php-7.0.12.tar.gz/from/this/mirror
> tar -zxvf mirror
```

## 编译PHP并且使用fpm
```
> cd php-7.0.12/
> sudo yum install gcc gcc++ libxml2-devel
> ./configure --prefix=/usr/local/php7 --enable-fpm --enable-mbstring
> make
> sudo make install
```

> fpm不是默认开启的，所以要使用`--enable-fpm`开启。`--enable-mbstring`是开启多字节语言包的。

## 配置PHP-fpm
```
> cd /usr/local/php7/etc/
> sudo mv php-fpm.conf.default php-fpm.conf
> cd /usr/local/php7/etc/php.d
> sudo mv www.conf.default ww.conf
```

## 修改配置文件后，找不到php文件的问题

修改
```
fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
```
