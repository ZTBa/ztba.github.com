---
layout: post
title: "debian环境下手动安装google chrome浏览器"
description: "Debian: install google chrome"
category: skills
tags: [linux,debian,skill]
---
{% include JB/setup %}

虽然chromium与chrome同宗同源，但毕竟还有少许差别：例如google的版本更新的快，还内置flash播放器，
等等。总之有那么一种需求，希望能够在debian环境下安装使用chrome浏览器。

这就要手动安装了，步骤如下：

    su
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
    sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/googlechrome.list'
    apt-get update && apt-get install google-chrome-stable

这样就可以享受google亲儿子了
