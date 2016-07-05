---
layout: post
title: "黑爵AJAZZ机械键盘AK33S上手体验"
description: ""
category: 感慨
tags: [keyboard,mechanical,comment]
---
{% include JB/setup %}


## 背景

现役两把机械键盘都是国外品牌， 一把cherry原厂清轴clear switch， 一把corsair红轴red switch。手感各有千秋。 后来看到国产机械键盘几乎是前者折半以下的价格销售，不由得开始长草。  
鉴于现有两个键盘一个是full sized （104 keys)， 一个是compact size ( 88 keys )，为避免重复投资，目标转向mini size （82 keys) 。  
这样一来Vortex Poker布局的键盘成为了首选。  
然后发现了国产品牌黑爵AJazz推出的AK33S。  

## 卖点

对背光其实完全零需求，那个东西太幼稚。 真正吸引我的还是布局--这个尺寸和键盘布局实在太好了，几乎不可以再小了，而基本功能又都照顾到了：尤其值得点赞的是右侧的一竖列Home, PgUp,PgDn,End，比poker还贴心－用拼音输入法的人应当了解其重要性。
  
## 体验

上手打字后有几个体验：  
一是国产轴blue switch声音好尖锐啊，相比之下clear switch 和 red switch音量也许差不多，但音调更低沉，因而扰邻现象缓和得多（red switch基本与薄膜键盘持平），这种哒哒哒的声音大概只有当初敲机械打字机Typewriter时听到过。  
二是黑爵这款产品的品控和大厂还有差距，例如Fn键和字母键录入声音都有差别，例如脚撑太松在包里就脱落了，例如塑料基座与顶层铝板接缝处的切削面明显有豁口。  
三是软件系统有待提升，从官网上下载的驱动在win8.1 x64系统下安装后竟然识别不出已经连接的键盘，所以这个驱动有什么功能不得而知。
  
## Azerty改造

买回来后一直没有当作生产力设备，最大的原因在于悲催的缺键！这是当初购买时完全没有料到的问题。  
因为面对中国市场，键盘采用英美Qwerty排列。以前一直没有发觉，在这样的排列下，左手shift键是长长一条；而在法文键盘Azerty或德文键盘Qwerty DE排列中，左手shift键是短键帽，其右侧是<键。作为一个程序员，这个符号实在不能少。  
解决方法一是沿用Qwerty排列，但手速立刻跌得离谱--没办法，这么多年已经习惯Azerty了。  
解决方法二是修改系统KeyMap。具体的说就是在Qwerty键盘上找一个替换按键，将之重新定义为<键。  

这里介绍一下方法二的实现.    
我选择了数字行最左侧的œ键。  

### linux系统

* 找到keycode，在Terminal下启用xev程序监听键盘输入，查知<键的keycode是94，而œ键的keycode是49。  
* 使用
```
    xmodmap -pke
``` 
 命令，查阅机器当前key maping  
* 创建一个对掉表文件，内容是  
    keycode 49 = less greater less greater bar brokenbar bar    
    keycode 94 = oe OE oe OE leftdoublequotemark rightdoublequotemark leftdoublequotemark   
* 加载这个映射表
```
    xmodmap ~/.Xmodmap
```

### windows系统

windows下， 同样以œ键为目标。奇葩的是在法语键盘布局下这个键被识别为²，我感觉整个儿人都方了

* 下载微软自家的Microsoft Keyboard Layout Creator， 安装，启动   
* 加载法语布局File=> Load Existing Keyboard =>Français   
* 创建自定义布局：单击左上角那个键，在弹出窗口点All按钮，之后\<Key\> 输入
```
U+003c
```
,shift+\<Key\>输入 
```
U+003e
```      
* 编译，生成一个适应32/64位系统的安装包，懒人可以一直接从
  [这里](http://ztba.github.io/img/fr_nv.7z)
下载  
* 安装新布局，在输入法中选定刚才编译出来的Français - Switch布局，之后重启（必需的）  

然后就可以愉快的输入<符号了（防呆提示，欲输入>符号需按shift键）。

## 结论

黑爵这款键盘手感上强于普通薄膜键盘，但和原厂轴比较依然有差距（做工，噪音），压力数介于清轴与红轴之间（我只用过这两个），造型很酷，布局贴心，性价比无敌。