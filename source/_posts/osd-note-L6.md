---
title: OSD_L6_note
date: 2021-04-06 21:25:34
tags: OSDI
categories: 資工
description: process management
---

# L6 process management


**system call** ![](https://i.imgur.com/iGAGQqP.png)

> system call就是當program使用system內的library，怎麼呼叫的流程吧

* exception是synchronous ㄉ，就會去執行完exception才會回原本的process（或跟本不回去了）

![](https://i.imgur.com/UoulA3a.png)
* 收到interrupt之後去vector table看該編號的動作
* vector table用來jump到procedure(routine)的位置 ex. divided by zero的procedure......

![](https://i.imgur.com/Vh9SttO.png)
![](https://i.imgur.com/vbmAjYv.png)

> The numbers of the system calls will be put in register eax. The other values are put into the remaining registers before calling the software interrupt int 0x80. After each syscall, an integer is returned in eax.

![](https://i.imgur.com/71tyE0m.png)
先exception，再用system call


![](https://i.imgur.com/OyJ1mc7.png)
* process context的概念
* context沒有明確的定義，但大概就是跟這個process有關的東西，例如register state，memory內容之類的總總
* user space跟kernel space的stack是分開的，為了安全因素
* 在同個process，從user space切到kernel space(due to system call)不算context switch

![](https://i.imgur.com/zctRLaQ.png)
ELF program(stored in disk)被load 到memory中就變成 process了

![](https://i.imgur.com/jpmYmaU.png)
* fork和exec的一些detail

![](https://i.imgur.com/MzXdB75.png)
整塊就是全部的memory吧

![](https://i.imgur.com/aU8Ff8h.png)


* parent create process descriptor for child.

## discussion
* mode的指令是有限制的，不同mode之下可以用的register也可能不同


* kernel space vs user space
    * 切換mode的時候，同時space會跳到RAM上的特定位置，代表kernel space
    * 但是在同個context之下（不會做context switch）
        * Advantage of context switch: 有完整空間可以用
        * Disadvantage of context switch: overhead，slow

* kernel program切kernel program context會比user program切user program context快
    * 因為kernel context不需要virtual memory

* thread vs process
    * every process都被記載在PCB內(process control block)
    * 但如果某個程式被開了100次，那就有100 processes，而且每次schedule都要做context switch，但context幾乎一樣，這樣做context switch就很浪費時間
    * icache可以reuse，這樣就有機會可以用到之前別人用過的instruction

* linux沒有process, 只有thread，PCB不叫座PCB，叫做task structure
    * 但在linux內，thread level的context switch也是有時間的落差
    * 相同的program切很快，不同的就會切得慢

* 從前從前，在一個program之下開了很多個threads，其實概念會像是那個program開了一個daemon，用來管理旗下的threads，而kernel不知道。==user thread的感覺==
    * for example，program A開了3個threads，program B開了5個threads，但這兩個program分到的CPU time是equal的
* 現在的ptrhead應該會讓kernel知道他有開threads了 ==kernel thread==

* hyper threading, core, SMP
    * SMP(Symmetric multiprocessing): 多個CPU放在同個硬體設備之上
    * hyper threading: 傳統上，CPU time很多時間花在register load/write，而不是ALU之上，所以就是多一套register給你用，這樣就可以有比較好的utilization
