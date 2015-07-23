---
layout: post
title: Install and Run MongoDB on Mac OS X / 在Mac下安装并使用MongoDB
date: 2015-07-23 15:30
post-link: 
---


<center>**------English (英文)------**</center>

<b>1> Install MongoDB by [Homebrew][hb]</b>

	$ brew update
	$ brew install mongodb

if you didn't have Homebrew yet, install it first. (programmer's must-have)


<b>2> MongoDB defaults data store under `/data/db`, so we need to create this folder</b>

	$ sudo mkdir -p /data/db
	$ sudo chown xxx /data/db

replace `xxx` by your current user name. If you not sure, you may run `$ whoami` first.


<b>3> Add `mongodb/bin` to $PATH</b>

	$ touch .base_profile
	$ vim .base_profile

add the following codes and then restart Terminal

	export MONGO_PATH=/usr/local/mongodb  
	export PATH=$PATH:$MONGO_PATH/bin  


<b>4> Run MongoDB</b>
   
	 $ mongod


<b>5> Query Database</b>

Open another Terminal window

	$ mongo

then you can write database command, for example,

	$ show dbs          // display existing databases
	$ use somedbname    // create (or use) a database

<b>6> Exit MangoDB</b>

	$ exit


Is this very easy?

<hr />

<center>**------中文 (Chinese)------**</center>


Mac下安装启动MongoDB很简单，在Terminal下运行以下指令就可以了


<b>1> 通过[Homebrew][hb]安装MongoDB</b>

	$ brew update 
	$ brew install mongodb

如果没有Homebrew还是先装一个吧，程序员必备。


<b>2> MongoDB 数据默认存在`/data/db`下，所以需要创建这个文件夹</b>

	$ sudo mkdir -p /data/db
	$ sudo chown xxx /data/db

请把xxx替换为自己当前的用户名，如果不确定可以先运行一下`$ whoami`


<b>3> 把`mongodb/bin`加入$PATH</b>

	$ touch .base_profile
	$ vim .base_profile

加入以下地址以后重启Terminal

	export MONGO_PATH=/usr/local/mongodb  
	export PATH=$PATH:$MONGO_PATH/bin  


<b>4> 启动MongoDB</b>
   
	 $ mongod

<b>5> 数据库查询</b>

在另一个Terminal窗口运行

	$ mongo

然后可以开始各种数据库指令，比如

	$ show dbs    //显示已经存在的数据库
	$ use somedbname    //创建（使用）某个数据库

<b>6> 退出</b>

	$ exit




[hb]:http://brew.sh/index.html


