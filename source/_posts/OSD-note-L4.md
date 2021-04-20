---
title: OSD_L4_note
date: 2021-03-18 23:12:00
tags: OSDI
categories: 資工
description: Interrupt, I/O
---
# L4 Interrupt & I/O
寫driver要知道那個硬體的spec
![](https://i.imgur.com/Va0WfSk.png)
會佔mem的值/不佔mem的值

* driver: 能跟IO裝置溝通的程式

![](https://i.imgur.com/pd6sGZd.png)


![](https://i.imgur.com/BNxToA4.png)
* NIC: 像是網卡
* Memory某個區段是map給IO的，就寫到那個位置就其實是在做IO
* 用網路收檔案舉例，我們希望是網卡把檔案收完檢查完，再interrupt CPU
* 給processor的interrupt是PIC給的
* ==IRQ-> interrupt controller==

![](https://i.imgur.com/Sol0v8L.png)
* mask: 蓋掉，ignore。阿就是mask
* interrupt會有priority之分，高的先做


![](https://i.imgur.com/IDMX1Uq.png)
* OS會對device driver分類
* 要去做open/read/write/close之類的動作
* device driver要做的
    1. 註冊(tty, usb......)
    2. implement API


* 寫driver要知道OS提供什麼框架還有硬體提供啥東西給你

## L4 discuss
* IO device 的response time.
    * 裝置有快有慢，都等慢的人的話就會被block住
    * 例如，現在只有GPU可以跟CPU接，mouse之類的其他的IO bus就不會直接和CPU接
    
* X86會分memory bus和IO bus是因為上古世紀，memory很珍貴，再拿一點去給IO很浪費。arm arthitecture就不用再額外開IO bus了

* 以前 IO device在bus上的位置都是固定的，現在就是dynamic的，就會有一個地方記錄他們所在的位置
    * 裝置裝上之後，就會去搶位置這樣

* software interrupt & hardware interrupt
    * Exception(software)
        * 是一道指令
        * 目前可以做到直接請CPU去vector table找特定的值，去執行程式（這之間沒有signal, 不像hardware interrupt）
        
    * Hardware interrupt
        * 是一個hardware訊號 
