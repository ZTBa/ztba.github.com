---
layout: post
title: "红米1s运行lineageOS音乐播放问题修正"
description: "在插入耳机线的情况下熄屏播放无故中断"
description: "安装笔记"
category: lessons
tags: [tutorial]
---
{% include JB/setup %}

##  问题概述

  红米1s运行LineageOS已经有一段时间了，这样一个搭配完美满足所有工作需求。唯一让我不满的问题出现在娱乐需求方面：在播放内置音乐文件的时候（我使用的是海贝播放器），如果插入耳机线，并且熄屏，会出现随机时长后声音中断的现象，点亮屏幕会发现依然处于播放状态，但耳机里就是没声音；不定时长后（无论熄屏亮屏）声音会重新恢复，乐曲内容连贯，没有跳过一段的感觉。这一点有别于单纯的系统静音。  
  但同样用海贝播放器，如果使用手机喇叭外放，则完全没有中断的情况，蓝牙耳机也播放正常。
  
##  问题分析

  开始以为是省电模式把后台进程干掉了，专门到开发者模式调高了后台进程许可数量。无效。再到论坛上溜达，发现不少人提到LOS与谷歌热词探测（hotword detection）服务冲突。偏偏我装得全家桶是极简版（pico version），根本没有关闭这项服务的入口。只好遵循[这篇](http://www.lineageosdownloads.com/get-google-assistant-lineage-os/)教程刷了遍机，然后到服务参数中关闭语音检测（顺便提一句，ok google这个功能真的挺带感的）。  
  然而，问题依旧。
  
##  解决方案

  真正奏效的方案是在[这个网页](https://forum.xda-developers.com/mi-5/help/lineageos-14-1-problem-headphones-sound-t3633885)上找到的，据说是LOS与小米系统之间的一个配合bug，解决方案简单说来就是
  
* `进TWRP`
* `wipe dalvik + cache`
* `安装miui（我是从小米官网下的miui8）`
* `wipe dalvik + cache`
* `安装LOS`
* `wipe dalvik + cache`
* `重启`  
  亲测有效。