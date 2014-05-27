---
layout: post
title: "ubuntu12.04中eclipse提示框背景为黑色的解决方法"
tagline: "How to solve eclipse's Black background of Prompt box in Ubuntu 12.04"
description: "Eclipse's Black background of Prompt box in Ubuntu 12.04"
tags: [eclipse, Ubuntu12.04, background]
---
{% include JB/setup %}

前段时间和一个朋友聊天，由于他要在Ubuntu下面用eclipse进行编码，但是遇到一个问题---eclipse的提示框的背景是黑色的导致可视性非常的差, 我就给他说我当时也遇到过这个问题，我也费了很大的劲最后找到解决方法怎么解决的。相信会有很多朋友会遇到这样的问题，这就样本文就诞生了。

**问题描述:**  
当我们在ubuntu下面使用eclipse编程的时候，需要查看相应的代码的结构等信息，但是当我们把鼠标放到我们想看的代码上面的时候，出现了一个大黑框，然后鼠标点击相应的块才能看见相应的文字，其他的全是黑色无法看见，导致心里很不爽，如下图：
![picture:eclipse-linux-01][1]  

**问题分析:**  
出现这个问题的原因主要是由于Ubuntu（12.04）中默认的主题为Ambiance，这个主题的tooltip默认设置前景(foreground)色为黑色(#ffffff)，背景(background)色为白色(#000000)，所以才出现这个问题。当然一般我们解决这个问题的方法是更换主题就好了，但是这样做不能根本的解决问题，下面我们介绍一种能够不更换主题，手工修改的解决方法。 


**解决方法：**   

*   1\. 进入系统用户主题目录
    : `$ cd /usr/share/themes/`

*   2\. 选择使用的主题，进入该主题目录，以Ambiance为例
    : `$ cd Ambiance`

*   3\. 修改主题配置gtk-2.0/gtkrc
    : `$ sudo vi gtk-2.0/gtkrc`
    ![picture:eclipse-linux-02][2]

*   4\. 修改tooltip的前景色(tooptip_fg_color)和背景色(tooptip_bg_color)  
    `tooltip_fg_color:#000000`  
    `tooltip_bg_color:#c5e0b3` (该颜色模式RGB为：R:197, G:224, B:179; 若想改为windows下面的淡黄色则为#f2edbc)

    注：若上述gitrc文件中没有tooltip的配置，则可以自己添加上述配置。
    ![picture:eclipse-linux-03][3]


*   5\. 保存退出，并且在系统设置--->外观--->主题的中切换下主题，然后在切换回来，就Okay了，如下图：
    ![picture:eclipse-linux-04][4]

[1]:http://imagle.github.io/static/img/eclipse-linux-01.png 
[2]:http://imagle.github.io/static/img/eclipse-linux-02.png
[3]:http://imagle.github.io/static/img/eclipse-linux-03.png
[4]:http://imagle.github.io/static/img/eclipse-linux-04.png
