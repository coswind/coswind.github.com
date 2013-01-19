---
layout: post
title: "Archlinux-Font 配置"
date: 2013-01-19 18:48
comments: true
categories: 
---

默认的配置，无论是英文还是中文，显示效果都不太满意。网上例子大都解释不清楚，不敢随意尝试，怕越改越乱。

##系统配置

首先要了解的是系统是怎样选择字体的？

系统的配置文件位于`/etc/fonts/conf.d`下, 通过阅读`README`, 了解这些配置前面数字的意义以及系统读取这些配置文件的`顺序`。

这些配置文件其实我们只需要了解两个: `49-sansserif.conf` & `65-nonlatin.conf`。

``` xml 49-sansserif.conf
<match target="pattern">
    <test qual="all" name="family" compare="not_eq">
        <string>sans-serif</string>
    </test>
    <test qual="all" name="family" compare="not_eq">
        <string>serif</string>
    </test>
    <test qual="all" name="family" compare="not_eq">
        <string>monospace</string>
    </test>
    <edit name="family" mode="append_last">
        <string>sans-serif</string>
    </edit>
</match>
```
