---
layout: post
title: "Longest Palindromic Substring-----最长回文子串"
tagline: "Longest Palindromic Substring-----最长回文子串"
description: "Longest Palindromic Substring-----最长回文子串"
tags: [算法, C++, 最长回文子串，Palindromic]
---
{% include JB/setup %}

首先讲讲什么是回文, 看看Wiki是怎么说的：回文，亦称回环，是正读反读都能读通的句子，亦有将文字排列成圆圈者，是一种修辞方式和文字游戏。回环运用得当，可以表现两种事物或现象相互依靠或排斥的关系， 比如madam，abba，这样正反都一样的串就是回文串。

今天要写的问题了就是在一个字符串中找出最长的回文字串，比如串："abcdedabakml"，它的最长回文字串就是"abcdedaba"。一般的方法有暴力法，动态规划法，今天来写一个时间复杂度为O(n)的算法。  

回文匹配，一般情况会分奇数和偶数来分开进行设计算法来统计，今天介绍的算法重新构造了一个字符串，这个新字符串消除了之前的奇偶差别，使得只用设计一种算法就可以。 

##### 1. 首先，构造新的字符串T
构造方法：在原字符串S的首尾和每个字符之间加入一个特殊字符'#', 例如S: abba， 则构造后为T: #a#b#b#a#(注：在程序中为防止越界会在首尾再加两个字符，即^#a#b#b#a#$);  

##### 2. 关键算法：  

a[ ]: a[i]代表以位置 i 为中心向左向右可扩展的长度  
mid: 当前取得最大回文的中心位置(下面例子用 m 表示)  
R： 以 mid 为中心，向右扩展 a[mid] 位的最右位置  
j:  与 i 关于 mid 对称的位置  

i | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9  
T | ^ | # | a | # | b | # | b | # | a | # | $  
a | 0 | 0 | 1 | 0 | 1 | 4 | 1 | 0 | 1 | 0  
  |   |   |   |   |   | m |   |   |   | R |   |

此算法的关键之处在于: 当 i<R时，a[i] 有如下简便计算公式` a[i] = min(R-i, a[j])`:  
              
- A. 当 R - i > a[j] 的时候，以T[j]为中心的回文子串包含在以T[mid]为中心的回文子串中，由于 i 和 j 对称，以T[i]为中心的回文子串必然包含在以T[mid]为中心的回文子串中，所以必有 a[i] = a[j], 如下图：此时a[i] = a[j] = 1;  
![picture:palindrome][1]  

- B. 当 R - i < a[j] 的时候，以T[j]为中心的回文子串不一定完全包含于以T[mid]为中心的回文子串中，但是基于对称性可知，下图中两个绿框所包围的部分是相同的，也就是说以S[i]为中心的回文子串，其向右至少会扩张到 R 的位置，也就是说 a[i] >= R-i。至于R之后的部分是否对称，就只能老老实实去匹配了。  
![picture:palindrome]2]  

##### 3. 得到所求字符串  

找出 p[ ] 数组中最大的值及其下标，记为max和index。  
所求字符串为string(s, (index-1-max)/2, max)  


##### 4, 算法实现  

    // Transform S into T.
    // For example, S = "abba", T = "^#a#b#b#a#$".
    // ^ and $ signs are sentinels appended to each end to avoid bounds checking
    string preProcess(string s) {
        int n = s.length();
        if (n == 0) return "^$";
        string ret = "^";
        for (int i = 0; i < n; i++)
            ret += "#" + s.substr(i, 1);
        ret += "#$";
        return ret;
    }

    string longestPalindrome(string s) {
        string T = preProcess(s);
        int n = T.length();
        int *a = new int[n];
        int mid = 0, R = 0;
        for (int i = 1; i < n-1; i++) {
            int j = 2*mid-i; // 找到i关于mid对称的位置    
            a[i] = (R > i) ? min(R-i, a[j]) : 0;
            // Attempt to expand palindrome centered at i
            while (T[i + 1 + a[i]] == T[i - 1 - a[i]])
                a[i]++;
            // If palindrome centered at i expand past R,
            // adjust center based on expanded palindrome.
            if (i + a[i] > R) {
                mid = i;
                R = i + a[i];
            }
        }
        // Find the maximum element in a.
        int maxLen = 0;
        int centerIndex = 0;
        for (int i = 1; i < n-1; i++) {
            if (a[i] > maxLen) {
                maxLen = a[i];
                centerIndex = i;
            }
        }
        delete[] a;  
        return s.substr((centerIndex - 1 - maxLen)/2, maxLen);
    }

__参考文章__:  
    http://leetcode.com/2011/11/longest-palindromic-substring-part-ii.html


---------------------------------------------------------------------------------------
我开通了微信公众号--__分享技术之美__，我会不定期的分享一些我学习的东西.
你可以搜索公众号：__swalge__ 或者扫描下方二维码关注我  
![关注][photo]  


[1]:http://imagle.github.io/static/img/palindromicsubstring-01.png 
[2]:http://imagle.github.io/static/img/palindromicsubstring-02.png
[photo]:http://imagle.github.io/static/img/photo.jpg
