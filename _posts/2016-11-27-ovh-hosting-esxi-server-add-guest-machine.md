---
layout: post
title: "OVH托管ESXi服务器环境下如何安装guest machine"
description: ""
category: 
tags: [linux,debian,tutorial]
---
{% include JB/setup %}


## 先决条件
**MAC + IP failover**  
 托管环境下是没有dhcp服务的，所以必须首先确定guest系统的ip，否则没有网络连接。
 系统镜像上传到datastore。这里选择Ubuntu v16 LTS x64。


## 安装步骤
 启动vShere Client， 配置guest， 修改参数，找到网络适配器，手动修改MAC地址。  
 之后启动guest，一路安装，会告知无法侦测dhcp，手动配置ip  
 填写IP failover作为地址和gatewey，mask为24，dns选取OVH统一地址：  
  **213.186.33.99**  
  安装结束后重启，发现依然无法上网，需要继续修改/etc/network/interface文件

		#The loopback network interface  
		auto lo  
		iface lo inet loopback  
		#The primary network interface  
		auto eth0  
		iface eth0 inet static  
			address XX  
			netmask 255.255.255.255  
			broadcast XX  
			post-up route add YY dev eth0  
			post-up route add default gw YY  
			pre-down route del YY  
			pre-down route del default gw YY  
			dns-nameservers 213.186.33.99  
			dns-search ovh.net  



 注释：  
  *XX	:guest获取的IP failover  
  *YY		:ESXi的gatewey  

 关于后者的获取，需要到vSphere Client，Host的设置标签页，DNS与路由选项，对应VMKernel 的地址


 期间一个小插曲，在interface文件中我发现历来是eth0的网卡名被设定成ens160，让人感觉很不爽， 于是查询真实属性  
 
	dmesg | grep -i eth

发现被重命名了，
于是进入到grub文件  
	
	sudo vi /etc/default/grub
	
修改GRUB_CMDLINE_LINUX行  

	GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
	
然后重新生成引导文件  

	sudo grub-mkconfig -o /boot/grub/grub.cfg


重启，世界美好了。
