---
title: OSD_L7_note
date: 2021-04-13 21:27:21
tags: OSDI
categories: 資工
description: process management (II)
---

# L7 process management II
This course focus on schedule and context.

* schedule: find the next process to run
* context switch: store the context of current process, restore the next process.

* schedule也是一個program

* user interactive的program，其overhead應該短（context switch頻繁）

* (tick kernel)內部會有timer interrupt，固定時間會回kernel，去跑schedule看有沒有需要做context switch

* kernel preemption
    * Linux kernel is possible to preempt a task at any point, so long as the kernel doesn't hold a clock.

![](https://i.imgur.com/g3p8rfH.png)

* nice value: 越低越被系統重視，可以得到的time slice比較大


## discussion
* 某個在kernel的proc能不能被中斷，取決於用到的resource會不會被其他人用到。沒有用到的話就可以被中斷
* kernel can be interrupted, also scheduler.

* kernel interrupt vs kernel preemption
    * 執行kernel code時，能不能接受被interrupt，代表kernel preemption
    * 先有kernel interrupt才有kernel preemption

* CPU time分配
    * Linux: 對所有proc發放配額，依照"過去使用時間"和priority來發放
    * 之前用掉越少的人拿越多
    * 越重要的拿越多
    * priority高的比較有機會可以拿到CPU使用
    * 重新發配額的時間：所有人用完或是有人用完的時候
    * 設計原因:
        * IO bound在這樣的狀況下更有可能可以使用CPU，因為他佔用的CPU time比較少

* call scheduler的時機
    * sleep, exit...
    * 拿不到resource....
    * 發現timer用完了
