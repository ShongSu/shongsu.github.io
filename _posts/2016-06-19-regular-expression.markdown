---
layout: post
title: Summaries of Regular Expression / 正则表达式总结
date: 2016-06-19 21:04
categories: [blog ]
tags: [regular expression, ]
description:
---

## Regular Expression Basic


Syntax     | Description
-----------|-------------
`abc…`	   |Letters
`123…`     |Digits
`\d`	     |Any Digit
`\D`	     |Any Non-digit character
`.`	       |Any Character
`\.`	     |Period
`[abc]`	   |Only a, b, or c
`[^abc]`	 |Not a, b, nor c
`[a-z]`	   |Characters a to z
`[0-9]`	   |Numbers 0 to 9
`\w`	     |Any Alphanumeric character
`\W` 	     |Any Non-alphanumeric character
`{m}`	     |m Repetitions
`{m,n}`	   |m to n Repetitions
`*`	       |Zero or more repetitions
`+`	       |One or more repetitions
`?`	       |Optional character
`\s`	     |Any Whitespace
`\S`	     |Any Non-whitespace character
`^…$`	     |Starts and ends
`(…)`      |Capture Group
`(a(bc))`	 |Capture Sub-group
`(.*)`	   |Capture all
`(abc|def)`|Matches abc or def



## Some Common Regular Expressions


Description            |Regular Expression
-----------------------|-------------------
Canada Postal Code     |`[a-zA-Z]\d[a-zA-Z]\s?\d[a-zA-Z]\d`
US ZIP Code(5 Numbers) |`\d{5}`
URL                    |`[a-zA-z]+:\/\/[^\s]*`
IP Address             |`((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)`
Email Address          |`\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*`
HTML Tags              |`<(.*)(.*)>.*<\/\1>|<(.*) \/>`
Date(yy-mm-dd)         |`(\d{4}|\d{2})-((1[0-2])|(0?[1-9]))-(([12][0-9])|(3[01])|(0?[1-9]))`
Time(hh:mm)            |`((1|0?)[0-9]|2[0-3]):([0-5][0-9])`
Capture tag contents   |`>([\w\s]*)<`
Get attribute values   |`='([\w://.]*)'`

To Be Continued...
