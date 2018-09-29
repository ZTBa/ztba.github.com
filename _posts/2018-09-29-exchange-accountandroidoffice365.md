---
layout: post
title: "如何在没有Exchange Account选项的Android下添加Office365帐号"
description: ""
category: lessons
tags: [tutorial]
---
{% include JB/setup %}

  选择Personal IMAP选项  
邮箱地址填入完整含域名的帐号firstname.lastname@company.com 以及密码

  IMAP服务器  
服务器地址写入outlook.office365.com  
port写入993  
Security Type选SSL/TLS  

  SMTP服务器  
服务器地址写入smtp.office365.com  
port写入587  
Security Type选TLS  
完工！