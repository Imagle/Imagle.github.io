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
1\.__push_heap__  
首先来看看STL中的该算法的实现，该算法实现在stl_heap.h文件中，再此函数被调用的时候，新元素已经插入到底部容器的最尾端。  

    template <class RandomAccessIterator, class Compare>
    inline void push_heap(RandomAccessIterator first, RandomAccessIterator last){
        __push_heap_aux(first, last, distance_type(first), value_type(first));
    }
    template <class RandomAccessIterator, class Distance, class T>
    inline void __push_heap_aux(RandomAccessIterator first, RandomAccessIterator last, Distance*, T*) {
        __push_heap(first, Distance((last - first) - 1), Distance(0), T(*(last - 1)));
    }
    template <class RandomAccessIterator, class Distance, class T>
    void __push_heap(RandomAccessIterator first, Distance holeIndex, Distance topIndex, T value) {
        Distance parent = (holeIndex - 1) / 2;
        while (holeIndex > topIndex && *(first + parent) < value) {
            *(first + holeIndex) = *(first + parent);
            holeIndex = parent;
            parent = (holeIndex - 1) / 2;
        }   
        *(first + holeIndex) = value;
    }  

可以看到push_heap算法最终调用的是__push_heap，算法内部都用的是迭代器，first是指向堆顶的迭代器，holeIndex指的新插入元素位置距离堆顶的距离，first+topIndex是该堆顶的位置，topIndex是距离， value是新插入的元素的值。  
可以看出__push_heap就是我们介绍的第一种堆算法----插入法。  

2\.____adjust_heap__  
在详细介绍pop_heap, sort_heap, make_heap三个算法前先来介绍另一个算法__adjust_heap，__adjust_heap函数定义如下:  

    template <class RandomAccessIterator, class Distance, class T>
    void __adjust_heap(RandomAccessIterator first, Distance holeIndex, Distance len, T value) {
        Distance topIndex = holeIndex;
        Distance secondChild = 2 * holeIndex + 2; //得到右孩子节点
        while (secondChild < len) {
            if (*(first + secondChild) < *(first + (secondChild - 1)))  //选择左右孩子中的较大节点
                secondChild--;
            *(first + holeIndex) = *(first + secondChild);
            holeIndex = secondChild;
            secondChild = 2 * (secondChild + 1);
        }
        if (secondChild == len) {
            *(first + holeIndex) = *(first + (secondChild - 1));
            holeIndex = secondChild - 1;
        }
        //此时没有满足整个堆的性质，需要再从下往上调整一次
        __push_heap(first, holeIndex, topIndex, value);
    }  

 通过将该算法和介绍的第二个算法相比，可知这个算法就是筛选法。  

3\.__pop_heap__  
pop_heap算法并不是将堆顶元素删除，而只是将其移到堆的最后，然后调整堆的时候按照len=len-1, 来进行调整，调整后之前最大的元素放在了容器的最后，可以用back()得到。  
函数定义如下  

    template <class RandomAccessIterator>
    inline void pop_heap(RandomAccessIterator first, RandomAccessIterator last) {
        __pop_heap_aux(first, last, value_type(first));
    }
    template <class RandomAccessIterator, class T>
    inline void __pop_heap_aux(RandomAccessIterator first, RandomAccessIterator last, T*) {
        __pop_heap(first, last - 1, last - 1, T(*(last - 1)), distance_type(first));
    }
    template <class RandomAccessIterator, class T, class Distance>
    inline void __pop_heap(RandomAccessIterator first, RandomAccessIterator last, RandomAccessIterator result, T value, Distance*) {
        *result = *first;
        __adjust_heap(first, Distance(0), Distance(last - first), value);
    }  

可以看出pop_heap最后调用的是__adjust_heap。  

4\.__sort_heap__  
sort_heap就是对堆进行排序。 从pop_heap得之，它每次可获得heap中键值最大的元素，如果持续对整个heap做pop_heap操作，每次将操作范围从后向前缩减一个元素，当整个程序执行完毕时，数组就是按照从大到小的顺序递增，下面是实际代码:  

    template <class RandomAccessIterator>
    void sort_heap(RandomAccessIterator first, RandomAccessIterator last) {
        while (last - first > 1) pop_heap(first, last--);
    }  

5\.__make_heap__  
make_heap用来将一段现有的数据转化为一个heap，下面是具体实现：  


[1]:http://imagle.github.io/static/img/eclipse-linux-01.png 
[2]:http://imagle.github.io/static/img/eclipse-linux-02.png
[3]:http://imagle.github.io/static/img/eclipse-linux-03.png
[4]:http://imagle.github.io/static/img/eclipse-linux-04.png
