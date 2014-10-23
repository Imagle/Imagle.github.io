---
layout: post
title: "ssh 快速登录远程服务器"
tagline: "ssh password "
description: "只用一个命令就能快速登录远程服务器"
tags: [ssh, password]
---
{% include JB/setup %}

最近入手了一台MBA，新东西刚用着确实很不顺手，各种不习惯，于是乎在紧张的时间之余开启了另一段征程---征服这台电脑。 
拿到mac，对于非ios开发的程序猿来说最常用的非终端莫属。经过几天的工作磨合下来，发现了iTerm2这么个好东西。  
    有关iTerm的安装请移步[iTerm2安装][2]  
    iTerm安装好后，接下来终端的各种配置和vim的配置，这个请移步[iTerm2配置和tmux配置][3]  
    有关sz，rz的安装配置，请移步[ZMODEM配置][4]  
我这篇文章很简单的说下如何最快速的通过终端访问远程服务器。

##### 基本情况

 - 服务器1台：web01
 - 目标：在mac终端下输入一个命令就能快速的登录远程服务器web01 

##### 开始设置 

1. __设置.alias.zshrc__   
    .alias.zshrc文件主要存放可以在终端下运行的命令的缩写，比如'll'  
    在.alias.zshrc中添加 web01=username@web01's ip -p 端口
    上述端口默认为22.
    sz=source ~/.zshrc

2. __设置.zshrc__   
    在.zshrc中添加source .alias.zhsrc
    上述source后写.alias.zshrc的路径  
    保存后退出，并执行命令sz

3. __为mac生成ssh key__   
    cd ~/.ssh
    如果没有上述目录则自己建一个mkdir ~/.ssh && cd ~/.ssh  
    ssh-keygen -t rsa
    这里可能会让输入名称，使用默认的就行，执行完后会在~/.ssh下生成一个id_rsa.pub文件
    将id_rsa.pub传输到服务器上去
    scp ~/.ssh/id_rsa.pub username@web01's:~/.ssh/
    
4. __在服务器上添加授权__  
    登录web01
    cd ~/.ssh
    cat id_rsa.pub >>authorized_keys
    rm id_rsa.pub
    Okay，到此我们已经配置完成了，在mac上的终端中输入web01试试吧。
    
##### 后话  
  人生的经历仿佛就是一段一段的解决问题的过程，在每段经历中肯定会遇到问题，接下来想办法，查资料，提方案，实验，结果没达到预期，继续改进，直到达到预期，达到预期心中肯定会有溢于言表的欣喜乃至狂欢，然后在这之后我们会接下来去解决下一个问题，...，这些问题或大或小，有些是生活中的有些是工作中的。分析、解决问题的这些过程就构成了我们的经历。各个阶段的经历拼成了人生。   

-------------------------------------------------------  
我开通了微信公众号--__分享技术之美__，我会不定期的分享一些我学习的东西.  
你可以搜索公众号：__swalge__ 或者扫描下方二维码关注我  
![关注][photo]  

[photo]:http://imagle.github.io/static/img/photo.jpg
[2]: http://iterm2.com/downloads.html
[3]: https://github.com/zanshin/dotfiles
[4]:http://h5b.net/iterm2-mac-os-ssh-client/