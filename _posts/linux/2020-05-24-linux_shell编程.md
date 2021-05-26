---
layout:     post
title:      "linux shell 编程"
subtitle:   "🐧"
date:       2020-05-24 12:00:00
author:     "Hiz"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - linux
---

shell是一个命令行解释器，他为用户提供了一个向Linux内核发送请求以便运行程序的界面系统级程序，用户可以用shell来启动、挂起、停止甚至编写一些程序。

# 脚本格式要求

* 脚本以 #!/bin/bash 开头
* 脚本需要有可执行权限

# shell变量

* linux shell 变量分为系统变量和用户自定义变量
* 系统变量：$HOME、$PWD、$SHELL、$USER等
* 显示当前shell中所有变量： set

## shell 变量自定义

* 定义变量： 变量名=值 （中间不要加空格）
* 撤销变量： unset变量
* 声明静态变量： readonly 变量，注意：不能unset # readonly a=2

## 变量规则

* 变量名称可以由字母、数字、下划线组成，但是不能以数字开头
* 等号两侧不能有空格
* 变量名称一般习惯为大写

## 将命令的返回值赋值给变量

```shell
A=`date`

A=$(date)
```

# 设置环境变量

```shell
# 基本语法
export 变量名=变量值 # 将shell变量输出为环境变量/全局变量
source 配置文件 # 修改后的配置信息立即生效
echo $变量名
```

# 位置参数变量

当执行一个shel脚本时，如果希望获取到命令行的参数信息，就可以使用到位置参数变量，

## 基本语法

* $n （n为数字，$0代表命令本身，$1-$9代表第一到第九个参数，十以上的参数需要用大括号包含${10}）
* $* （代表命令行中所有参数，$*把所有参数看成一个整体）
* $@ （命令行中所有参数，会把每个参数区分对待）
* $# （命令行中所有参数的个数）

# 预定义变量

shell事先定义好的变量，可以再shell脚本中直接使用

## 基本语法

* $$ (当前进程的进程号（pid）)
* $! (后台运行的最后一个进程的进程号（pid）)
* $? (最后一次执行的命令的返回状态，如果这个变量值为0，证明上一个命令正确执行，否则证明上一个命令执行不正确)

```shell
#!/bin/bash

echo "挡墙执行的进程id=$$"
# 以后台的方式运行一个脚本并获取她的进程号
/root/home/test.sh & 
echo "最后一个后台方式运行的进程id=$!"
echo "执行结果是=$?"
```

# shell 运算符

## 基本语法

* ”$((运算式))“ 或”$[运算式]“（常用） 或者 expr m + n
* 如果使用expr，运算符中间要有空格。 expr m - n
* expr \*, /, % 乘 除 取余

```shell
#!/bin/bash

# 1、计算（2+3）x 4 的值

# 第一种写法
RES1=$(((2+3)*4))
echo "RES1=$RES1"

# 第二种写法
RES2=$[(2+3)*4]
echo "RES2=$RES2"

# 第三种写法 expr
TEMP=`expr 2 + 3`
RES3=`expr $TEMP \* 4`
echo "RES3=$RES3"

# 2、请求命令行两个参数的和
SUM=$[$1+$2]
echo "SUM=$SUM"
```

# shell 条件判断

## 基本语法
[ condition ] (condition前后都有空格)，非空返回true,可以用$?验证 （0为true, >1为false）

## 常用判断条件

* = 字符串比较
* -lt 小于
* -le 小于等于
* -eq 等于
* -gt 大于
* -ge 大于等于
* -ne 不等于
* -r 有读的权限
* -w 有写的权限
* -x 有执行的权限
* -f 文件存在，并且是一个常规的文件
* -e 文件存在
* -d 文件存在，且是目录

```shell
#!/bin/bash
# 判断ok = ok
if [ "ok" = "ok" ]
then
  echo "equal"
fi

# 判断12是否大于13
if [ 12 -ge 13 ]
then
  echo "-ge"
fi

# /root/home/a.txt 是否存在
if [ -f /root/home/a.txt ]
then
  echo "存在"
fi
```

# shell 流程控制

```shell
if [ $1 -ge 60 ]
then
  echo "及格"
elif [ $1 -lt 60 ]
then
  echo "不及格"
fi

# switch case 
case $1 in
"1")
echo "周一"
;;
"2")
echo "周二"
;;
*)
echo "other..."
;;
esac
```

## for 循环

```shell
#1!/bin/bash
# 计算1加到100的值

SUM=0
for(( i=1; i<=100; i++))
do
  SUM=$[$SUM+$i]
done
echo "总和：$SUM"
```

## while 循环

```shell
#1!/bin/bash
# 从命令行输入一个数n,统计1+...+n的值

SUM=0
i=0
while [$i -le $1]
do
  SUM=$[$SUM+$i]
  i=$[$i+1]
done
echo "总和：$SUM"
```

# read 读取控制台输入

* read(选项)(参数)
* 选项 -p: 指定读取值得提示符
* 选项 -t: 指定读取值时等待的时间（秒），如果没有在指定时间内输入，就不等待了
* 参数： 变量 指定读取值的变量名

```shell
#!/bin/bash
# 读取控制台输入一个NUM1值
read -p "请输入一个数NUM1=" NUM1
echo "你输入的NUM1=$NUM1"
# 读取控制台输入一个num值，在10秒内输入

read -p -t 10 "请输入一个数num" num
echo "你输入的num=$num"
```

# 函数

## 系统函数

```shell
# basename 基本语法
# basename [pathname] [suffix]
# 删除所有的前缀包括最后的一个（'/'）字符，然后将字符串显示出来
# suffix 为后缀，然后将字符串显示出来
basename /home/aaa/a.txt # 返回 a.txt

basename /home/aaa/a.txt txt # 返回 a

# 返回路径
dirname /home/aaa/a.txt # /home/aaa
```

## 自定义函数

```shell
# 自定义函数
function getSum() {
  SUM=$[$n1+$n2]
  echo "和是$SUM"
}

# 输入两个值
read -p "请输入一个数n1=" n1
read -p "请输入一个数n2=" n2

# 调用函数
getSum $n1 $n2
```

# exp

```shell
# 定时备份数据库
#!/bin/bash
# 1、每天凌晨2：30备份数据库user到/data/backup/db
# 2、备份开始和备份结束能够给出相应的提示信息
# 3、备份后的文件要求已备份时间为文件名，并打包成.tar.gz的形式，比如2021-05-24_230201.tar.gz
# 4、备份的同时，检查时候有10天备份的数据库文件，如果有就将其删除
# 备份目录
BACKUP=/data/backup/db
# 当前时间
DATETIME=$(date +%y-%m-%d_%H%M%S)
echo $DATETIME
# 数据库地址
HOST=localhost
# 数据库用户名
DB_USER=root
# 数据库密码
DB_PW=root
# 备份的数据库名
DATABASE=user

echo "开始备份"
# 创建备份目录，
[ ! -d "$BACKUP/$DATETIME"] && mkdir -p "$BACKUP/$DATETIME"

# 备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --datebases ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/$DATETIME.sql.gz

# 将文件处理成tar.gz
cd ${BACKUP}
tar -zcvf ${BACKUP}/${DATETIME}

# 删除对应的备份目录
rm -rf ${BACKUP}/${DATETIME}

# 删除10天前的备份文件
find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "备份结束"
```