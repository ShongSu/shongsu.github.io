---
layout: post
title: Internet Protocol - SMTP and POP3 / 网络协议系列 - SMTP和POP3协议
date: 2016-06-13 20:43
categories: [blog ]
tags: [Internet Protocol, ]
description:
---

## 1 SMTP协议

[SMTP][smtp]（Simple Mail Transfer Protocol），即简单邮件传送协议。同`HTTP`等多数应用层协议一样，它工作在C/S模式下，用来实现因特网上的邮件传送。

SMTP是用来将客户机上的邮件传送到服务器上。这里的客户机是指某次连接中的发送方，服务器是指相应的接收方。邮件服务器向其它邮件服务器转发邮件也是采用SMTP协议。

### 邮件的收发过程

一般情况下，一封邮件的发送和接收过程如下:

1)  发信人在用户代理里编辑邮件，包括填写发信人邮箱、收信人邮箱和邮件标题等等。

2)  用户代理提取发信人编辑的信息，生成一封符合邮件格式标准的邮件。

3)  用户代理用SMTP将邮件发送到发送端邮件服务器（即发信人邮箱所对应的邮件服务器）。

4)  发送端邮件服务器用SMTP将邮件发送到接收端邮件服务器（即收信人邮箱所对应的邮件服务器）。

5)  收信人调用用户代理。用户代理用POP3协议从接收端邮件服务器取回邮件。

6)  用户代理解析收到的邮件，以适当的形式呈现在收信人面前。


### SMTP通信过程

一个具体的SMTP通信（如发送端邮件服务器与接收端服务器的通信）的过程如下:

1)  发送端邮件服务器（以下简称客户端）与接收端邮件服务器（以下简称服务器）的`25`号端口建立TCP连接。

2)  客户端向服务器发送各种命令，来请求各种服务（如认证、指定发送人和接收人）。

3)  服务器解析用户的命令，做出相应动作并返回给客户端一个响应。

4)  2)和3)交替进行，直到所有邮件都发送完或两者的连接被意外中断。


### SMTP命令格式

SMTP的命令不多（14个），它的一般形式是：

`COMMAND  [Parameter] <CRLF>`

其中`COMMAND`是ASCII形式的命令名，`Parameter`是相应的命令参数，`<CRLF>`是回车换行符(`0DH`, `0AH`)。

SMTP的响应也不复杂，它的一般形式是：

`XXX  Readable Illustration`

`XXX`是三位十进制数；`Readable Illustration`是可读的解释说明，用来表明命令是否成功等。`XXX`具有如下的规律：以`2`开头的表示成功，以`4`和`5`开头的表示失败，以`3`开头的表示未完成（进行中）。


### SMTP的缺点

1)  命令过于简单，没提供认证等功能。

2)  只传送7位的ASCII码，不能传送二进制文件。

针对缺点1)，标准化组织制定了扩充的SMTP（即[ESMTP][esmtp]）。ESMTP最显著的地方是添加了用户认证功能。如果用户想使用ESMTP提供的新命令，则在初次与服务器交互时，发送的命令应该是`EHLO`而不是`HELO`。

针对缺点2)，标准化组织在兼容SMTP的前提下，提出了传送非7位ASCII码的方法：对邮件首部的扩充和对邮件正文的扩充（即[MIME][mime]）。

### 浏览器发送邮件

这个过程是这样的：
1)  `xxxx@gmail.com`在Gmail提供的邮件页面上填写的相应信息（如发信人邮箱、收信人邮箱等）；
2)  通过`HTTP`协议被提交给gmail服务器；
3)  gmail服务器根据这些信息组装一封符合邮件规范的邮件（就像用户代理一样）；
4)  然后`smtp.gmail.com`通过SMTP协议将这封邮件发送到接收端邮件服务器。


## 1 POP3协议

[POP3][pop3]，（Post Office Protocol version3），即邮局协议第3版。它被用户代理用来邮件服务器取得邮件。POP3采用的也是C/S通信模型。

### POP3通信过程

用户从邮件服务器上接收邮件的典型通信过程如下：

1)  用户运行用户代理（如Foxmail, Outlook Express）。

2)  用户代理（以下简称客户端）与邮件服务器（以下简称服务器端）的`110`端口建立TCP连接。

3)  客户端向服务器端发出各种命令，来请求各种服务（如查询邮箱信息，下载某封邮件等）。

4)  服务端解析用户的命令，做出相应动作并返回给客户端一个响应。

5)  3）和4)交替进行，直到接收完所有邮件转到步骤6)，或两者的连接被意外中断而直接退出。

6)  用户代理解析从服务器端获得的邮件，以适当地形式（如可读）的形式呈现给用户。

其中2)、3）和4）用POP3协议通信

### POP3命令格式

POP3的命令不多，它的一般形式是：

`COMMAND  [Parameter] <CRLF>`

其中`COMMAND`是ASCII形式的命令名，`Parameter`是相应的命令参数，`<CRLF>`是回车换行符(`0DH`, `0AH`)。

服务器响应是由一个单独的命令行组成，或多个命令行组成，响应第一行`+OK`或`-ERR`开头，然后再加上一些ASCII文本。`+OK`和`-ERR`分别指出相应的操作状态是成功的还是失败的。

POP3相对于因特网报文存取协议[IMAP][imap]（Internet Message Access Protocol）的最大的不足是：它只是一个脱机协议，客户与服务器的交互性不是特别好。例如不能直接在邮箱中创建文件夹，不太好选择性地下载邮件的某部分。


[smtp]:https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol
[pop3]:https://en.wikipedia.org/wiki/Post_Office_Protocol
[esmtp]:https://en.wikipedia.org/wiki/Extended_SMTP
[mimie]:https://en.wikipedia.org/wiki/MIME
[imap]:https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol
