---
title: OSD_L3_note
date: 2021-03-18 23:10:32
tags: OSDI
categories: 資工
description: Some acknowledge about booting process
---

# L3 boot process
Power on -> booting from permanent memory (or booting from and devices) -> loading OS -> loading processes......

## Why four loaders are necessary
* first loader (bootloader in permanent memory such as ROM/NOR flash/randomly access and byte addressable)
    * Process can run but knows nothing about the whole system
    * ==prepared by SoC vendors or motherboard vendors== to make sure there is no problem with whole system (CPU藉由program去了解周邊的環境跟裝置是啥) （很可能是出廠前就燒好的）
    > SoC: System on memory
    * ==check/initialize the hardware==
    * May provide basic services to other program (API之類的？？，讓你去access某些硬體之類的)
    * May provide shells to end-users for basic operations.
    * Load/jump to the secod loader (based on predefined procedures/configurations)

* Second loader (bootloader in any devices such as DISK/CD-ROM/NAND flash/remote server/...) (放在另外的裝置上)
    * As soon as first bootloader can access the secod loader (on any device), load it into memory, and can jump to the starting instruction
    * ==Prepare by OS vendors== or other third party vendors
        * Stored in the correct location/in correct format
        * Must know how to load OSs
    * May provide shell to end-user to select OSs(multi-booting) or set configurations 
    * ==Goal==: Loading OS into memory
    * Optional for emdedded processors
* third loader (OS loader that loads OS)
    * A program executes without OS serivces
    * Initial and prepare OS services
* Fourth loader (processes replies on OS to fork/exec other program)
    * OS is now ready and can provide services
    * Any process calls OS services to fork/execute other processes 


## First boot-loader functions
* initialize(reset) the hardware setting
    * Power on self-test in x86/OS
* Basic monitor and debugger
* Pass the control to 2nd bootloader

> 跳到memory執行second bootloader是因為2nd bootloader需要比較大的空間之類的

![](https://i.imgur.com/QUUrnFT.png)
![](https://i.imgur.com/dePMSGb.png)
> 應該是first bootloader
> * POST: power on self-test
* 一開始的時候就是直接到specific location去拿東西

![](https://i.imgur.com/nDqqz7g.jpg)


## discussion
* CPU本身沒有driver
* vector table -> 指向其他device的記憶體的東西
* 開機
    * 在完整開機之下，要從BIOS開始，從沒有作業系統開始開，要把OS從hardware搬進Memory
    * 睡眠的話，就是在已經有OS之下再開
