---
layout: post
title: "同时显示两个Excel文件内容"
description: "老问题"
category: 技巧
tags: [Excel, skill]
---
{% include JB/setup %}

## 奇怪的历史问题
  微软Office的其他组件,word, powerpoint等, 基本上都是每打开一个文件就创建一个实例, 占据一个窗口. 唯独excel是多个文件共享一个窗口. 也许能够节省资源, 但如果要双文件对比操作就挠头了

  偏偏这个问题早就存在, 十几年过去了微软还是这个德性.

## 解决方法
  微软早就贴出了[解决方案](https://support.microsoft.com/en-us/kb/2636670), 具体地说就是到注册表里把非常规的 **/dde** 参数改成大伙喜闻乐见的 **"%1"** , 注意引号不能省.

  regedit=>Coputer\HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open\command
改 (Default) 和 command 命令末尾, 同时重命名 Coputer\HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open\ddeexec键. 

  保险起见对Excel.Sheet.8做同样的修改, 虽然我只改了12就已经生效了.
  
  此技巧适用于office 2003/2010.