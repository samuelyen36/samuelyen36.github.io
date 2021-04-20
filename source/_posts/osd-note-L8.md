---
title: OSD_L8_note
date: 2021-04-20 21:27:55
tags: OSDI
categories: 資工
description: memory management 
---

# L8 memory management I

* benefits of virtual memory
    * isolate processes from each other
    * Large/continue per process address space

* disadvantage of virtual memory
    * expensive and slow

* MMU is used for memory translation
* virtual memory是連續的，但對應的physical memory不一定需要連續
![](https://i.imgur.com/YTNOOyA.png)

* 在code之中，如果要access某個address(例如jump，或去看global variable)，那個address也會被compiler編成virtual memory
* 在user space基本上無法查virtual memory對應的physical memory

## segmentation memory management
* MMU: memory management unit
* MEU: memory protection unit

* 有segmentation和virtual address的狀況(x86)
![](https://i.imgur.com/SbN4qAv.png)
* linear address也是virtual address
* TLB can be regarded as a cache of memory translation.
    * TLB will be flushed when context switch happens
    * Logical cache也要flushㄛ，但physical cache可以不用flush

* logical cache vs physical cache (是先cache再MMU還是先過MMU再cache)
![](https://i.imgur.com/aStYlFR.png)

![](https://i.imgur.com/Eo1htnA.png)
設計上不能接受近kernel就要context switch，因為太慢了

## discussion
* segment和page的意義
    * create a larger(and also continuous) memory space than physical memory

* 可不可以load一個超過physical memory and disk大小的程式
    * 可以load，可能這個程式執行的時候其實只需要一點點memory
    * 阿發現真的炸開的話就abort

* 目前allocate memory都只跟定址空間有關，和memory大小以及swapfile大小無關

* swaping space的大小要多大
    * 根據要使用的程式的寫法，可能有87程式吃一堆資源這樣

* page fault的時候，要怎麼知道在硬碟上的實際上的哪個位置？
    * VMA會有記載資訊，OS把資訊做各種處理之後issue給disk driver去處理

* 早期的memory design:
    * virtual memory都是4GB, 1GB for kernel and 3GB for other application
    * 早期kernel不能超過1GB，超過要做partition
    * Nowadays, 大概有128MB的大小用page來對，為了就是超過1GB的限制
        * 所以現在存取kernel space的時候，效率不一定都相同