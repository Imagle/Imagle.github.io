---
layout: post
title: "论C++的STL源码中关于堆算法的那些事"
tagline: "Something about heap in C++ STL"
description: "Something about heap in C++ STL"
tags: [堆, C++, STL源码]
---
{% include JB/setup %}

关于堆，我们肯定熟知的就是它排序的时间复杂度在几个排序算法里面算是比较靠上的O(nlogn)经常会拿来和快速排序和归并排序讨论，而且它还有个优点是它的空间复杂度为O(1), 但是STL中没有给我们提供像vector, deque, stack, queue之类的数据结构供我们使用，但在C++STL中却提供了一些列的算法，让我们依旧可以使用堆，比如make_heap(), push_heap(), pop_heap(), sort_heap()。今天就来论论这几个算法，在介绍上述算法之前先引入两个关于堆的算法，上述四个算法本质上都使用引入的两个算法。 

###### 一、关于堆的两个算法  

    1\.__堆的插入算法__  
    下面先看个例子，一个数组a[]={1,9,5,13},建一个最大堆，下图为过程  
    ![picture:build max heap][1]  
    从图中可以看出每插入一个元素，调整后都是一个最大堆，而且是插入到最后一个上(假设数组是可以动态增长的)，这样是从下往上进行调整(percolate up)，这样的调整算法叫插入法, 下面来看看插入法的代码实现：  
    <pre>
            void insert_heap(int a[], int i){
                while(i>0){
                    int j=(i-1)/2;
                    if(a[i] < a[j]){
                         swap(a[i], a[j]);
                         i=j;
                    }
                    else break;
                }
            }
    </pre>  

    2\. __堆的筛选法__  
    还是先看个例子

[1]:http://imagle.github.io/static/img/eclipse-linux-01.png 
[2]:http://imagle.github.io/static/img/eclipse-linux-02.png
[3]:http://imagle.github.io/static/img/eclipse-linux-03.png
[4]:http://imagle.github.io/static/img/eclipse-linux-04.png
