---
layout: post
title: "使用powershell远程控制windows服务器"
description: "PSTools淘汰后的替代方案"
description: "笔记"
category: lessons
tags: [tutorial]
---
{% include JB/setup %}


##  前言

  在UAC还没有出现的年代，windows环境下远程命令调用大约只有PSTools一条路可走，但无可否认的是这条路顺畅无比，长久以来我们使用的自动化脚本都是以此为基石的。  
  然后美好时光结束了。
  
  最近要在一个非AD环境下实现远程命令调用，操作系统是Server 2008 R2和Windows 7。尽管下载了号称匹配Srv2008的PSTools （v2.45），我依然悲伤的发现童话都是骗人的。  
  于是Powershell成了唯一的选择。
  
##  准备工作

###  网络类型
  
  在非AD环境下，各机器处于WorkGroup环境。为了实现winrm的远程调用，需要将网络类型设定为`私有`。修改方法多样，可以直接点击修改，也可以不怕麻烦的到注册表下`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetwarkList\Profiles\XX`项目中，设定DWORD类型变量 Category =>1， CategoryType=>0。其中XX对应当前网络连接名称。
  
### Powershell的脚本执行许可

  默认的powershell脚本是不可执行的，需要以管理员权限打开powershell端口（这个设定适用于以后所有的powershell命令），键入命令  
  
	set-executionpolicy remotesigned  
  
### 防火墙开口子

  有时会发现远程测试命令`test-rsman  A的IP`  怎么都过不去，多数情况是被墙了。解决方案是执行如下命令  
  
	winrm quickconfig
	
  会添加一个例外规则到防火情上。

## 配置各机器  
  
  服务器设为A，操作端设为B。
  
### A方配置

  输入命令检测`winrm`服务的运行状态
  
	get-service winrm

  如果是running，继续键入
  
	enable-psremoting -force
	
  以许可远程控制本地程序。

### B方配置

  关键点在于添加远程机器到信任名单中，方法1是在具有Admin权限的cmd窗口录入`winrm set winrm/config/client @{TrustedHosts=”A的IP”}`，方法2是据说这个命令同样可以在具有admin权限的powershell中执行，方法3是powershell窗口录入`Set-Item WSMan:\localhost\Client\TrustedHosts -Value "A的IP" -Force`。 注意，为了可以许可所有机器，参数`A的IP`可以用通配符`*`代替。  
  检验方法是`Get-Item WSMan:\localhost\Client\TrustedHosts`

#### 0x80070005报错

  在设定信任名单时有可能会遇到这个报错，解决方法是到注册表下`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`项目中，设定DWORD类型变量LocalAccountTokenFilterPolicy =>1
  

## 脚本研究

  远程命令执行当然要用`invoke-command`指令，同时为了避免每次重新输入用户名密码，可以采用特殊格式预设定登陆信息  
  
	$Username = 'username'
	$Password = 'password'
	$pass = ConvertTo-SecureString -AsPlainText $Password -Force
	$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $Username,$pass
  
  然后用Credential参数调用  
  
	Invoke-Command -ComputerName A的IP -Credential $Cred -ScriptBlock{set-date "猴年马月"}
	
##  Windows环境下对于Linux脚本的远程调用

  这个技能在powershell出现之前就存在了，幸运的是powershell同样支持在脚本中混杂这个命令，一个ps1文件就可以全包括。  
  具体的说，需要通过putty内置的plink.exe调用ssh，呼叫linux下的预定script，完整的命令是：  
  
	PLINK.EXE -ssh  C的用户名@C的IP -pw C的密码 /home/YY/脚本.sh
	
  在Linux中，如果想用sudo级别的命令又不想费事输入密码的，可以使用如下方案：  
  
	echo -e "sudo的密码\n" | sudo service ZZ restart
	
##  脚本的视窗化

  为了方便传统windows用户（习惯双击鼠标左键），可以设定一个快捷方式到这个脚本；另外为方便检视运行结果，使用参数-NoExit保留powershell窗口。 完整命令如下：  
  
	powershell -NoExit "c:\脚本.ps1"