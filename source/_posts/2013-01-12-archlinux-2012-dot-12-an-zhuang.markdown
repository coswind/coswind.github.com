---
layout: post
title: "Archlinux-2012.12 安装"
date: 2013-01-12 21:25
comments: true
categories: archlinux
---

##检查网络

默认情况dhcp服务是enabled，如果使用的是有线，并且调制解调器和路由器工作良好，网络是正常可用的。

####有线-无路由器

    pppoe-setup
    pppoe-start

设置正确并启动成功，网络仍然无法连通:

    systemctl disable dhcpcd.service
    pppoe-stop
    pppoe-start

还无法连通，试着先ping`8.8.8.8`，如果能ping通，将`8.8.8.8`加入nameserver

####WIFI

    wifi-menu

情况和上面类似    

##分区

一般来说，分三个区，swap分区为82

    cfdisk: /, /home, swap
    
    mkfs.ext4 {/, /home}
    mkswap swap
    swapon swap
    swapon -s

    mount {/, /home} {/mnt, /mnt/home}

##安装基本系统

    /etc/pacman.d/mirrorlist 163
    
    pacstrap /mnt base{,-devel} 
    pacstrap /mnt grub-bios

注意在安装完基本的软件包后，针对pppoe和wifi的上网方式，需要分别安装对应的软件包

    wifi  --> MUST INSTALL dialog.
    pppoe --> MUST INSTALL rp-pppoe

    多系统 --> MUST INSTALL os-prober

##生成fstab

    genfstab -p /mnt >> /mnt/etc/fstab

##配置新系统

    arch-chroot /mnt

####Locale

    /etc/locale.gen
    
    locale-gen
    
    /etc/locale.conf
    
####Time

    timedatectl

    硬件时钟不正确 --> ntpdate "asia.pool.ntp.org" && hwclock -w

####设置主机名

    hostnamectl

####网络设置

    pacman -S wpa_actiond
    wireless --> systemctl enable net-auto-wireless.service

####创建初始ramdisk

    mkinitcpio -p linux

####设置root密码

    passwd

####添加新用户

    useradd -m coswind 
    passwd coswind
    usermod -a -G video,audio,storage,optical,users,lp,wheel,log,scanner,games,power coswind

##安装grub

    grub-install /dev/sda
    防止启动时的warning: cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

    grub-mkconfig -o /boot/grub/grub.cfg

##Reboot

    exit 
    umount -a
    reboot




