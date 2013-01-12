---
layout: post
title: "Archlinux-2012.12 安装"
date: 2013-01-12 21:25
comments: true
categories: 
---

##检查网络

####有线-无路由器

    pppoe-setup
    pppoe-start

####WIFI

    wifi-menu

####note

    systemctl disalbe dhcpcd.service | 8.8.8.8 > nameserver

##分区

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

####note

    wifi  --> MUST INSTALL dialog.
    pppoe --> MUST INSTALL rp-pppoe

    多系统 --> MUST INSTLL os-prober

##生成fstab

    genfstab -p /mnt >> /mnt/etc/fstab

##配置新系统

    arch-chroot /mnt

####Locale

    /etc/locale.gen
    
    locale-gen
    
    /etc/locale.conf
    
    LANG=

####Time

    timedatectl

    硬件时钟不正确 --> ntpdate "asia.pool.ntp.org" && hwclock -w

####设置主机名

    hostnamectl

####网络设置

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




