---
layout: post
title: "论C++的STL源码中关于堆算法的那些事"
tagline: "Something about heap in C++ STL"
description: "Something about heap in C++ STL"
tags: [堆, C++, STL源码]
---
{% include JB/setup %}

关于堆，我们肯定熟知的就是它排序的时间复杂度在几个排序算法里面算是比较靠上的O(nlogn)经常会拿来和快速排序和归并排序讨论，而且它还有个优点是它的空间复杂度为O(1), 但是STL中没有给我们提供像vector, deque, stack, queue之类的数据结构供我们使用，但在C++STL中却提供了一些列的算法，让我们依旧可以使用堆，比如make_heap(), push_heap(), pop_heap(), sort_heap()。今天就来论论这几个算法，在介绍上述算法之前先引入两个关于堆的算法，上述四个算法本质上都使用引入的两个算法。 

#### 一、关于堆的两个算法  

1\.__堆的插入算法__  
下面先看个例子，一个数组a[]={1,9,5,13},建一个最大堆，下图为过程  
![picture:build max heap][1]  
从图中可以看出每插入一个元素，调整后都是一个最大堆，而且是插入到最后一个上(假设数组是可以动态增长的)，这样是从下往上进行调整(percolate up)，这样的调整算法叫插入法, 下面来看看插入法的代码实现：  

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

2\. __堆的筛选法__  
还是先看个例子  
例：a[]={1, 9, 5, 13}, 排序过程如下图:  
![picture:siftheap][2]  
像这样一次自上而下的调整过程，叫做筛选法， 算法实现:  

          void sift_heap(int a[], int i){
              // j 是 i 的左孩子
              int j=2*i+1;
              int n = strlen(a);
              while(j<n){
                  if(j<n-1 && a[j]<a[j+1]) ++j;
                      if(a[i]<a[j]){
                           swap(a[i],a[j]);
                           i=j;
                           j=2*i+1;
                      }
                      else
                         break;
                  }
               }
          void build_heap(int a[], int n){
              int i=n/2;
              //循环完后，就建立了一个最大堆
              for(; i>=0; ++i){
                  sift_heap(a, i);
              }
          }

#### 二、STL算法中的xxxx_heap算法  

stl中的关于堆的算法都是在vector上实现的，并且实现的最大堆，且最大的元素位于数组的第一个位置。  
1\._push_heap_  
首先来看看STL中的该算法的实现，该算法实现在stl_heap.h文件中，再此函数被调用的时候，新元素已经插入到底部容器的最尾端。  

[1]:http://imagle.github.io/static/img/eclipse-linux-01.png 
[2]:http://imagle.github.io/static/img/eclipse-linux-02.png
[3]:http://imagle.github.io/static/img/eclipse-linux-03.png
[4]:http://imagle.github.io/static/img/eclipse-linux-04.png
