---
layout:     post
title:      "linux rpm yum"
subtitle:   "🐧"
date:       2020-05-23 12:00:00
author:     "Hiz"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - linux
---

# rpm

```shell
# 查询已安装的rpm列表，
rpm -qa | grep firefox

# rpm 相关指令
rpm -qa # 查询所安装的所有rpm软件包
rpm -qa | more

rpm -q firefox # 查询软件包是否安装

rpm -qi firefox # 查询软件包信息

rpm -ql firefox # 查询软件包中的文件

rpm -qf /etc/passwd # 查询文件所属软件包
rpm -qf /root/install.log

rpm -e firefox # 卸载rpm包
rpm -e --nodeps firefox # 强制卸载rpm包


# 安装rpm包
# 需要先下载，在安装
rpm -ivh rpm包全路径名称
# i 安装 v 提示 h 进度条

```

# yum

yum是一个shell前端软件包管理器，基于rpm,能够从指定的服务器自动下载rpm包并安装

```shell
# yum 基本指令
yum list | grep firefox # 查询

yum install firefox # 安装
```