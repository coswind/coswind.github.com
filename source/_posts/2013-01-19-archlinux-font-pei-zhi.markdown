---
layout: post
title: "Archlinux-Font 配置"
date: 2013-01-19 18:48
comments: true
categories: archlinux font
---

默认的配置，无论是英文还是中文，显示效果都不太满意。网上例子大都解释不清楚，不敢随意尝试，怕越改越乱。

##系统配置 [/etc/fonts/*]

首先要了解的是系统将字体分为那几大类？

> 主要分为三大类，`衬型`，`非衬型`和`等宽`，具体介绍和效果可以见[这里](http://wenq.org/cloud/fcdesigner_local.html)

首先要了解的是系统是怎样选择字体的？

> 系统的配置文件位于`/etc/fonts/conf.d`下, 通过阅读`README`, 了解这些配置前面数字的意义以及系统读取这些配置文件的`顺序`。

这些配置文件其实我们只需要了解两个: `49-sansserif.conf` & `65-nonlatin.conf`。
``` 
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
上面的配置简单明了，将所有的字体归为了三大类，而不属于这三大类的字体，默认归为`sans-serif`。

```
<alias>
    <family>serif</family>
    <prefer>
        <family>Artsounk</family> <!-- armenian -->
        ...
        <family>SimSun</family> <!-- han (zh-cn,zh-tw) -->
        <family>PMingLiu</family> <!-- han (zh-tw) -->
        <family>WenQuanYi Zen Hei</family> <!-- han (zh-cn,zh-tw) -->
        <family>WenQuanYi Bitmap Song</family> <!-- han (zh-cn,zh-tw) -->
        ...
    </prefer>
</alias>

<alias>
    <family>sans-serif</family>
    <prefer>
        <family>Nachlieli</family> <!-- hebrew -->
        ...
        <family>SimSun</family> <!-- han (zh-cn,zh-tw) -->
        <family>PMingLiu</family> <!-- han (zh-tw) -->
        <family>WenQuanYi Zen Hei</family> <!-- han (zh-cn,zh-tw) -->
        <family>WenQuanYi Bitmap Song</family> <!-- han (zh-cn,zh-tw) -->
        ...
    </prefer>
</alias>

<alias>
    <family>monospace</family>
    <prefer>
        <family>Miriam Mono</family> <!-- hebrew -->
        ...
        <family>NSimSun</family> <!-- han (zh-cn,zh-tw) -->
        <family>MingLiu</family> <!-- han (zh-tw) -->
        <family>AR PL ShanHeiSun Uni</family> <!-- han (ja,zh-cn,zh-tw) -->
        <family>AR PL New Sung Mono</family> <!-- han (zh-cn,zh-tw) -->
        <family>HanyiSong</family> <!-- han (zh-cn) -->
        ...
    </prefer>
</alias>
```
上面的配置文件则是各类字体在分别的类别中优先级，越靠前的字体，优先级越高。

从上面两个配置文件我们可以基本上了解为什么Linux系统装完`wenquanyi zen hei`字体后，系统的中文就"默认"成了该字体，然而该字体在linux下，稍显模糊。

相比较下，我更喜欢`wenquanyi zen hei sharp`, 十分适合终端显示，而`wenquanyi micro hei`则十分适合浏览器显示。

无图无真相，下面是系统的字体配置效果。
![urxvt-zh-screenshot.png](/images/urxvt-zh-screenshot.png)
![chromium-zh-screenshot.png](/images/chromium-zh-screenshot.png)

##用户配置 [~/.config/fontconfig/fonts.conf]

SO?如何达成上面截图中所示的中文现实效果，直接贴配置:
```
<match>
    <test name="family"><string>Terminus</string></test>
    <edit name="family" mode="append" binding="strong">
        <string>WenQuanYi Zen Hei Sharp</string>
    </edit>
</match>
<match>
    <test name="family"><string>serif</string></test>
    <edit name="family" mode="append" binding="strong">
        <string>WenQuanYi Zen Hei Sharp</string>
    </edit>
</match>
<match>
    <test name="family"><string>sans-serif</string></test>
    <edit name="family" mode="append" binding="strong">
        <string>WenQuanYi Micro Hei</string>
    </edit>
</match>
<match>
    <test name="family"><string>monospace</string></test>
    <edit name="family" mode="append" binding="strong">
        <string>WenQuanYi Zen Hei Sharp</string>
    </edit>
</match>
```
很简单吧，就像前面介绍的，我们只要把自己喜欢的中文字体放在各类别的前面就可以了。

这里再稍微解释一下为什么会有第一个`match`的配置，原因也很简单，`Terminus`字体属于不常见的字体，不在系统默认的字体分类里面。

根据前面的介绍，不在三大分类的字体会被默认分为`sans-serif`，因而中文显示会是`wenquanyi micro hei`。但是该字体在终端下表现不太好，因此我们需要给它加上相应中文字体的设定。

> 这里还要补充的一点是`mode`的设定，是`append`，而不是`prepend`，前者是将字体的优先级设定在匹配字体的后面，即在匹配不到的时候作为第一候选，而后者相反。
> 因为中文字型大都包含英文字体，所以使得不仅中文是设定字体，英文都强制的使用了中文字型里的英文字体，实在是太难看，而且所有的英文字体都是如此。
