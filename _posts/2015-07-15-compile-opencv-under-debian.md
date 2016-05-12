---
layout: post
title: "Compile OpenCV under Debian"
tagline: "Debian wheezy x64环境下编译OpenCV"
description: "折腾3.0.0版本未遂的全过程"
category: 
tags: []
---
{% include JB/setup %}

# 依赖环境的安装

## FFMPEG相关模块

首先添加deb-multimedia提供的库文件支持到本地库列表中

    su -
    echo "deb http://www.deb-multimedia.org wheezy main non-free" >> /etc/apt/sources.list.d/deb-multimedia.list
    echo "deb-src http://www.deb-multimedia.org wheezy main non-free" >> /etc/apt/sources.list.d/deb-multimedia.list
    apt-get update
    
之后安装公匙

    apt-get install deb-multimedia-keyring
    apt-get update

最后安装依赖文件

    sudo apt-get install build-essential checkinstall git cmake libfaac-dev libjack-jackd2-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libsdl1.2-dev libtheora-dev libva-dev libvdpau-dev libvorbis-dev libx11-dev libxfixes-dev libxvidcore-dev texi2html yasm zlib1g-dev libsdl1.2-dev libvpx-dev

此处曾经出现过libfaac-dev找不到的情况, 后来发现是因为没有apt-get update的原因


## 安装其他库文件

Gstreamer:

    sudo apt-get install libgstreamer0.10-0 libgstreamer0.10-dev gstreamer0.10-tools gstreamer0.10-plugins-base libgstreamer-plugins-base0.10-dev gstreamer0.10-plugins-good gstreamer0.10-plugins-ugly gstreamer0.10-plugins-bad gstreamer0.10-ffmpeg

libgtk2:

    sudo apt-get install libgtk2.0-0 libgtk2.0-dev  

libjepg:

    sudo apt-get install libjpeg8 libjpeg8-dev 

## 编译库文件

    mkdir ~/source  
    mkdir ~/source/x264  
    mkdir ~/source/v4l  
    mkdir ~/source/opencv  
    mkdir ~/source/ffmpeg  

### Compile X264

    cd ~/source/x264  
    wget ftp://ftp.videolan.org/pub/videolan/x264/snapshots/x264-snapshot-20120528-2245-stable.tar.bz2  
    tar xvf x264-snapshot-20120528-2245-stable.tar.bz2  
    cd x264-snapshot-20120528-2245-stable  
    ./configure --enable-shared --enable-pic
    make  
    sudo make install  

### Compile ffmpeg
    cd ~/source/ffmpeg  
    wget http://ffmpeg.org/releases/ffmpeg-0.11.1.tar.bz2  
    tar xvf ffmpeg-0.11.1.tar.bz2  
    cd ffmpeg-0.11.1  
    ./configure --enable-gpl --enable-libfaac --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libtheora --enable-libvorbis --enable-libx264 --enable-libxvid --enable-nonfree --enable-postproc --enable-version3 --enable-x11grab --enable-shared --enable-libvpx --enable-pic
    make  
    sudo make install  

### Compile v4l
    cd ~/source/v4l  
    wget http://www.linuxtv.org/downloads/v4l-utils/v4l-utils-0.8.8.tar.bz2  
    tar xvf v4l-utils-0.8.8.tar.bz2  
    cd v4l-utils-0.8.8  
    make  
    sudo make install  

# 终于可以编译OpenCV本体了
此处一个小插曲, 下载了 OpenCV3.0.0正式版试图编译, 结果在24%进度报错, 原因是cap_ffmpeg.cpp文件在45行处使用了一个未声明enum类型变量AVCodecID. 尝试所有方案,未遂. 重新回到2.4.9版本,一次编译就过了.
还有改进空间啊OpenCV Team.

    cd ~/source/opencv
    wget http://kent.dl.sourceforge.net/project/opencvlibrary/opencv-unix/2.4.9/opencv-2.4.9.zip  
    unzip opencv-2.4.9.zip  
    cd opencv-2.4.9  
    mkdir release  
    cd release  
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_NEW_PYTHON_SUPPORT=ON ..
    make
    sudo make install
