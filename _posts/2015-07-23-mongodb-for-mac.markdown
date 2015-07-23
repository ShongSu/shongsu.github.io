---
layout: post
title: Install and Run MongoDB on Mac OS X
date: 2015-07-23 15:30
post-link: 
---


**English (英文)**



**中文 (Chinese)**


Mac下安装启动MongoDB很简单，在Terminal下运行以下指令就可以了


1. 通过[Homebrew][hb]安装Mongodb

    $ brew update 

    $ brew install mongodb

如果没有Homebrew还是先装一个吧，程序员必备。


2. Mongodb 数据默认存在/data/db下，所以需要创建这个文件夹

    $ sudo mkdir -p /data/db

    $ sudo chown xxx /data/db

请把xxx替换为自己当前的用户名，如果不确定可以先run $ whoami


3. 把mongodb/bin加入$PATH

    $ touch .base_profile

    $ vim .base_profile

加入以下地址以后重启terminal

	export MONGO_PATH=/usr/local/mongodb  
	export PATH=$PATH:$MONGO_PATH/bin  


4. 启动mongodb
   
    $ mongod

5. query database

在另一个terminal窗口运行

    $ mongo

然后可以开始各种数据库指令，比如

    $show dbs    //显示已经存在的数据库

    $use somedbname    //创建（使用）某个数据库

6. 退出

    $exit



[hb]:http://brew.sh/index.html


