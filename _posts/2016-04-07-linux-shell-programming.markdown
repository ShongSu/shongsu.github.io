---
layout: post
title: Linux Shell Programming / Linux Shell编程
date: 2016-04-07 22:38
post-link:
---

## My First Shell

The first line must be start with `#!`

    #!/bin/bash
    # print hello,world
    echo "hello,world!"

脚本的执行需要一定的权限：

1. `chmod +x 脚本名字`  ##所有用户均对该脚本有执行权限。

2. `chmod 755 脚本名字`  ##用户所有者对该脚本具有完全的权限，其他人对该脚本拥有执行权限。

运行脚本：

1. 在当前目录下使用 `./脚本名` 来运行。

2. 或者以 `/bin/sh 脚本名` 来运行。


## Variables & IO

Shell中的变量名可以由字母、数字、下划线组成，但数字不能作为变量的第一个字符。

使用`=`来定义一个变量的值，例如：

1. `myname='pengyu'`  # 字符串类型，不解析任何字符

2. `courses="abcdef"` # 双引号内部会解析$和反斜杠特殊字符。

3. ``now_date=`date` `` # 反引号执行系统命令

Shell 的变量类型只有字符串类型。

变量的引用，使用`$`加变量名。

变量名外面加花括号是可选的，是为了帮助解释器识别变量的边界，比如：

    #!/bin/bash
    for skill in php java linux db2
      do
        echo "I am good at ${skill}Script"
      done

单引号，双引号和反引号的使用方法：

    sh-3.2$ testvar=100
    sh-3.2$ echo 'The testvalue is $testvar'

输出 The testvalue is $testvar

    sh-3.2$ echo "The testvalue is $testvar"

输出 The testvalue is 100

    sh-3.2$ now_date=`date`
    sh-3.2$ echo $now_date

输出 2016年 4月 7日 星期四 23时26分46秒 EDT


环境变量：
    PATH:系统路径
    HOME:当前用户Home目录
    HISTSIZE:保存历史命令记录的条数
    LOGNAME:当前用户登录名
    HOATNAME:主机名称
    SHELL:当前用户是哪种shell
    LANG/LANGUGE:和语言相关的环境变量，使用多语言的用户可以修改此环境变量
    MAIL:当前用户的邮件存放目录

查看环境变量：

    env: 显示所有环境变量
    echo $PATH :显示某一个环境变量的值

局部变量：

作用域从被定义的地方开始，到shell结束为止，其作用域为本脚本，离开本脚本变量无效。

预定义变量：

Shell一开始就定义了的变量，用户只能使用它们而不能重新定义。所有预定义变量都由￥符和另一个符号组成，常用的Shell预定义变量有：

    $# 位置参数的数量
    $* 所有位置参数的内容
    $? 命令执行后返回的状态，0表示成功，非0失败
    $$ 当前进程的进程号
    $! 后台运行的最后一个进程号
    $0 当前执行的进程名

  
