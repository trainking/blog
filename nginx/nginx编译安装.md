# nginx编译安装

- [nginx编译安装](#nginx编译安装)
  - [准备工作](#准备工作)
    - [1. 检查系统版本](#1-检查系统版本)
    - [2. 安装必备](#2-安装必备)
    - [3. 编译](#3-编译)
    - [4. 安装](#4-安装)

## 准备工作

### 1. 检查系统版本

```
[vagrant@localhost nginx]$ uname -a
Linux localhost.localdomain 3.10.0-229.el7.x86_64 #1 SMP Fri Mar 6 11:36:42 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

内核版本大于`2.6`就可以安装

### 2. 安装必备

```
yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

### 3. 编译

```
./configure --prefix=/usr/local/nginx
```

### 4. 安装

```
make && make install
```

热部署仅需要make，然后再把二进制文件替换