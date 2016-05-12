---
layout: post
title: "How to config http proxy in Debian xfce4"
description: ""
category: skills
tags: [linux,debian,skill]
---
{% include JB/setup %}

## Why http proxy
Many entreprises do not allow their employee visit web site for private reason. A config http proxy is obligate. 
Unlike Unity on Ubuntu, xfce4 on Debian have no option `Network Proxy Setting`, so we have to set it in different way.

## How To
There are two utilisation : `apt-get` type and general web browser.

### apt-get

	sudo sh -c 'echo "Acquire::http::proxy \"http://XXX.XXX.XXX.XXX:YYYY/\";" >> /etc/apt/apt.conf'
	
In here XXX.XXX.XXX.XXX is proxy server IP and YYYY is proxy server port.

### general web browser

	sudo sh -c 'echo "export http_proxy=\"http://XXX.XXX.XXX.XXX:YYYY\"\nexport proxy=\"http://XXX.XXX.XXX.XXX:YYYY\"" >> /etc/profile.d/proxy.sh'
	source /etc/profile.d/proxy.sh
	
### about SSL
on little surprise with `https` connexion, navigator refuse de show the content of web page. A trick is add correct dns IP at `/etc/network/interfaces` then restart networking service.

Done.