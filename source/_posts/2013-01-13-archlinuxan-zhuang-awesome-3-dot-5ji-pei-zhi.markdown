---
layout: post
title: "Archlinux基本配置."
date: 2013-01-13 16:42
comments: true
categories: 
---

##安装图形界面

安装X

    pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils

安装mesa获得3D支持

    pacman -S mesa

安装显卡驱动

    pacman -S nvidia

笔记本安装触屏驱动

    pacman -S xf86-input-synaptics

##测试X

安装默认的测试环境

    pacman -S xorg-twm xorg-xclock xterm
    rm ~/.xinitrc
    startx

删除了.xinitrc, startx会启动默认的测试环境

##安装awesome

    pacman -S awesome

启动X的时候默认启动awesome

    cp /etc/skel/.xinitrc ~/
    echo "exec awesome" >> ~/.xinitrc

如果想登录后自动启动awesome

    [[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && exec startx >> .bash_profile [or .zprofile]

##声音配置

默认不需要多余的配置，只需要解除默认的静音状态即可

    pacman -S alsa-utils
    alsamixer

按照提示，解除master的静音状态
