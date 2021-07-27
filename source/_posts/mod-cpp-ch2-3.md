---
title: Modern C++ - Ch2.3 Type inference
date: 2021-06-17 20:22:54
tags: Modern_C++
categories: 資工
description: modern C++ Ch2.3, auto and decltype
---
# Ch2.3 Type inference
C language一直以來給人的印象都是strong type的語言
但是在C++11之後，其加入了`auto`和`decltype`去實現type derivation，可以使開發者更有彈性空間

## auto type
* 讓compiler可以自動去辨識變數所需要的type
``` C++
#include <cstdio>

int main(void){
    auto f=3.0;    //treated as double
    auto i=3;      //treated as integer
    return 0;
}
```
![](https://i.imgur.com/UUZjjda.png)

但auto 一定要給initialization value，且不能當作function的return type

## decltype
* 為了解決auto只能用在宣告變數的問題
* usage: `decltype(expression)`

``` C++
#include <cstdio>

int main(void){
    
    auto f=3.0; //treated as double
    auto i=3;   //treated as integer

    decltype(f+i) sum;  //treated as double
    sum = f+i;

    return 0;
}
``` 

## 補充 - How to extract the type of variables in C or C++
C 不像python，可以使用`type(expr)`去看某個expression 的型別
尤其在這章節是講述C++ 對於type 的處理，所以理解其運行模式是滿重要的
在自己跑範例code 的時候也遇到無法直接印出C++ variable 的type 的問題，所以就查了一下資料，目前覺得最方便的是下面兩種方法

1. GDB


GDB 是常見的文字介面的debug 工具
而其中的`ptype`可以印出該variable 的type

下面則是範例，檔名是auto.cpp，executable取名叫_auto，而使用的編譯器是g++

![](https://i.imgur.com/oYg1RTe.png)


* 編譯指令: `g++ -g auto.cpp -o _auto`
* 開啟GDB: `gdb _auto`

![](https://i.imgur.com/Wm7WYYF.png)
* 設定斷點(可以自己調整啦): `b 11`，即set a breakpoint at line 11
* 讓程式執行到斷點: `r`，就是run的意思
* 印出變數的型別: `ptype $variable_name`

如果要執行GDB，請記得在編譯的指令加上-g 的參數
因為若是想使用GDB，編譯器必須要記錄下編譯時的symbol table 以及source code 等等的資訊(如果沒有-g 就不會記錄這些額外的東西，這樣executable file 就是一坨assembly code 的binary，沒有辦法回推到原始碼對應的參數以及行數，換而言之就是普通的exectable 沒辦法提供足夠的debug 訊息)

2. std::is_same

*  `std::is_same<T, U>` is used to determine whether the two types `T` and `U` are equal.
* The following example will generate "type x == int" due to the fact that x is declared as an integer variable.

``` C++
void check_type(){
    int x;
    if (std::is_same<decltype(x), int>::value){
        std::cout << "type x == int" << std::endl;
    }
    if (std::is_same<decltype(x), float>::value){
        std::cout << "type x == float" << std::endl;
    }
    if (std::is_same<decltype(x), double>::value){
        std::cout << "type x == double" << std::endl;
    }
}
```
