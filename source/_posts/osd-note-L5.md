---
title: OSD_L5_note
date: 2021-03-20 21:24:10
tags: OSDI
categories: 資工
description: Interrupt, I/O (II)
---

# L5 Interrupt and interrupt handling II
* CPU在執行main program的時候有可能context switch到interrupt handling的code
![](https://i.imgur.com/ti6tYIn.png)

![](https://i.imgur.com/Gumpk00.png)
* PIC: programmable interrupt controller

* Exception通常是synchornous的，Interrupt通常是asynchornous

* Exception frame
![](https://i.imgur.com/fPnSWbd.png)

* ![](https://i.imgur.com/NwNTKPT.png)
* Interrupt latency: Interrupt發生到開始執行第一行的Interrupt handler的時間
* RTOS通常會在意interrupt latency

![](https://i.imgur.com/EaFyKEn.png)
* software operations

* Interrupt handling可以分前半後半
    * 前半: diable interrupt，處理比較critical的東西
    * 後半: enable interrupt，跟interrupt相關的task

* In interrupt handler, many APIs can't be called. For example, sleep.
* Driver內部寫的function call需要跟kernel註冊
![](https://i.imgur.com/mYSiGpw.png)

![](https://i.imgur.com/Gi1fvfV.png)
by default寫tasklet，除非是time critical就寫softirq

## Discussion
* 做top half, bottom half的原因
    * 做多工，寧運整個interrupt變久，也要分成兩塊來做
    * 希望重要的工作快做完(top half)，比較沒那麼重要的等之後再做

* Interrupt
    * WorkQueue
        * Top half做完後，bottom half放在queue
        * 固定一個daemon看著處理
        * 優點：簡單
        * 缺點：Interrupt不知道拿時可以做完
        * 適合：不用急的事情（ex. printer）
    * Tasklet
    * SoftIRQ
        * top half做完，bottom half放在queue裡面
        * 每次做完top half就去看queue，看有沒有人在queue排隊，就去那邊執行
        * 優點： 很快有效率
        * 缺點： 要依照特定格式來寫，很複雜
