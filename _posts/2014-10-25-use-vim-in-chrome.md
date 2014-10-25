---
layout: post
title: "vimer们福利，一款能在chrome上面使用vim快捷键的插件"
tagline: "vim chrome"
description: "能在chrome上面使用vim快捷键的插件"
tags: [vim, chrome]
---
{% include JB/setup %}

今日推荐一款在chrome上面使用vim的插件Vichrome[下载地址][1], 这款插件非常实用，应该说是非常合各个vimer的胃口，基本的快捷操作都延续了vim的风格，并且支持自定义，相当棒。  

##### 默认快捷键简介  

说明：下面命令中的C均指的是ctrl(windows, linux, mac ox), 例如<C-f>就是同时按下ctrl+f  

1.  __Normal Mode Mapping (nmap)__  

    j : ScrollDown  
    <C-e> : ScrollDown  
    k : ScrollUp  
    <C-y> : ScrollUp  
    h : ScrollLeft  
    l : ScrollRight  
    <C-f> : PageDown  
    <C-b> : PageUp  
    <C-d> : PageHalfDown  
    <C-u> : PageHalfUp  
    gg : GoTop  返回到当前页面顶部  
    G : GoBottom  返回到当前页面底部 
    t : TabOpenNew  新建一个标签页  
    x : TabCloseCurrent  关闭当前标签页  
    X : TabCloseCurrent --focusprev  关闭当前标签页，并将其之前一个标签页作为活动标签页  
    n : NextSearch  搜索关键词的下一个出现的位置  
    N : PrevSearch  搜索关键词的上一个出现的位置  
    gt : TabFocusNext  当前标签页的下一个标签  
    gT : TabFocusPrev  当前标签页的上一个标签  
    <C-l> : TabFocusNext  同gt  
    <C-h> : TabFocusPrev  同gT  
    r : TabReload  重新加载当前页面  
    H : BackHist  在当前页面后退  
    L : ForwardHist  在当前页面前进  
    : : GoCommandMode  打开命令模式  
    / : GoSearchModeForward  进入搜索模式--从页面当前位置开始到页面底部  
    ? : GoSearchModeBackward  进入搜索模式--从页面当前位置开始到页面顶部  
    a : GoLinkTextSearchMode  
    f : GoFMode  进入选择模式  
    F : GoFMode --newtab  进入选择模式，但在新标签页打开所选链接  
    ; : GoExtFMode  搜索  
    i : FocusOnFirstInput  将光标指向页面的第一个输入位置  
    u : RestoreTab  恢复之前关闭的标签  
    gp : WinOpenNew --pop  在新窗口中打开当前标签页  
    gs : TabOpenNew --next view-source:%url  查看当前页面的源码  
    yy : copyurl  复制当前页面的url到剪切板  
    p : Open %clipboard  在当前标签页中打开剪切板中的链接  
    P : TabOpenNew %clipboard  在当前标签页中打开剪切板中的链接  
    o : Open -i  在当前标签页中打开在命令行中输入的链接地址  
    O : TabOpenNew -i  在新标签页中打开在命令行中输入的链接地址  
    s : Open -i g  在当前标签页打开在命令行中输入关键字的搜索结果(默认是google)  
    S : TabOpenNew -i g  在新标签页打开在命令行中输入关键字的搜索结果(默认是google)  
    b : Open -b  在当前标签页中打开在命令行中输入的书签地址  
    B : TabOpenNew -b  在新标签页中打开在命令行中输入的书签地址  
    '' : BackToPageMark  
    <C-^> : TabSwitchLast  转到当前窗口的最后一个标签页  
    <C-ESC> : GoEmergencyMode  
    <ESC> : Escape  回到普通模式,并清除之前的所有操作记录  
    <C-[> : Escape  同上  
    ,z : ToggleImageSize  
    <SPACE>tw : TabOpenNew http://www.twitter.com  快速在新标签页打开twitter  
    <SPACE>gr : TabOpenNew http://www.google.com/reader  快速在新标签页打开google reader(现已停用)  
    <SPACE>m : TabOpenNew https://mail.google.com/mail/#inbox  快速在新标签页打开gmail  

2. __Insert Mode Mapping (imap)__  

<ESC> : Escape
<C-[> : Escape

3. __Search/Command Mode Mapping (cmap)__  

<TAB> : FocusNextCandidate
<S-TAB> : FocusPrevCandidate
<DOWN> : FocusNextCandidate
<UP> : FocusPrevCandidate
<ESC> : Escape
<C-[> : Escape

4. __Emergency Mode Mapping (emap)__  

<ESC> : Escape

5. __Command Aliases (alias)__  

    o : Open  
    ot : TabOpenNew  
    opt : OpenOptionPage  
    help : TabOpenNew http://github.com/k2nr/ViChrome/wiki/Vichrome-User-Manual  
    map : NMap  
    tabe : TabOpenNew  
    tabnew : TabOpenNew  
    tabn : TabFocusNext  
    tabp : TabFocusPrev  
    tabN : TabFocusPrev  
    tabr : TabFocusFirst  
    tabl : TabFocusLast  
    tabc : TabCloseCurrent  
    tabo : TabCloseAll --only  
    tabs : TabList  
    q : TabCloseAll  
    copyurl : Copy %url  
    copytitle : Copy %title  
    viewsource : TabOpenNew --next view-source:%url  
    OpenNewTab : TabOpenNew  
    MoveToNextTab : TabFocusNext  
    MoveToPrevTab : TabFocusPrev  
    MoveToFirstTab : TabFocusFirst  
    MoveToLastTab : TabFocusLast  
    CloseCurTab : TabCloseCurrent  
    CloseAllTabs : TabCloseAll  
    ShowTabList : TabList  
    ReloadTab : TabReload  
    OpenNewWindow : WinOpenNew  
    ext : TabOpenNew chrome://extensions/  
    option : TabOpenNew chrome://settings/browser  
    downloads : TabOpenNew chrome://downloads  
    history : TabOpenNew chrome://history  
    
##### 后话  

    人生的经历仿佛就是一段一段的解决问题的过程，在每段经历中肯定会遇到问题，接下来想办法，查资料，提方案，实验，结果没达到预期，继续改进，直到达到预期，达到预期心中肯定会有溢于言表的欣喜乃至狂欢，然后在这之后我们会接下来去解决下一个问题，...，这些问题或大或小，有些是生活中的有些是工作中的。分析、解决问题的这些过程就构成了我们的经历。各个阶段的经历拼成了人生。   

-------------------------------------------------------  
我开通了微信公众号--__分享技术之美__，我会不定期的分享一些我学习的东西.  
你可以搜索公众号：__swalge__ 或者扫描下方二维码关注我  
![关注][photo]  

[photo]:http://imagle.github.io/static/img/photo.jpg
[1]: https://chrome.google.com/webstore/detail/vichrome/gghkfhpblkcmlkmpcpgaajbbiikbhpdi
