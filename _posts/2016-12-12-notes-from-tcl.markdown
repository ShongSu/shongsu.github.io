---
layout: post
title: Notes from Tcl and the Tk Toolkit / Tcl/Tk入门经典
date: 2016-12-12 22:42
categories: [blog ]
tags: [Tcl, Tk]
description:
---


## Part I. Tcl语言

### Ch1 概述

进入Tcl：`tclsh`

算数运算：`expr 表达式`

退出Tcl：`exit`

创立图形界面：`wish`

使用wish的`hello world`程序：

		button .b -text "Hello, World!" -command exit
		grid .b

设置变量值：`set a 44`

使用变量：`expr $a * 4`

命令替换：`set b [expr $a*4]`

转义字符：`\$ -> $`, `\n`

**阶乘过程：**

		proc factorial {val} {
		  set result 1
		  while {$val>0} {
		    set result [expr $result*$val]
		    incr val -1
		  }
		  return $result
		}

如果没有return命令，一个过程的返回值就是该过程块中所执行的最后一条命令的返回值

**UI界面：**

#! /user/local/bin/wish

		proc factorial {val} {
		  set result 1
		  while {$val>0} {
		    set result [expr $result*$val]
		    incr val -1
		  }
		  return $result
		}

		entry .value -width 6 -relief sunken -textvariable value
		label .description -text "factorial is"
		label .result -textvariable result
		button .calculate -text "Calculatr" -command {set result [factorial $value]}
		bind .value <Motion> {
		    .calculate flash
		    .calculate invoke
		}
		grid .value .description .result -padx 1m -pady 1m
		grid .calculate - - -padx 1m -pady 1m

**交互式修改组件：**

		source fileName
		.value configure -background yellow
