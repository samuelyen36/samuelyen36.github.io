---
title: Modern C++ - Ch2.1 Constant
date: 2021-06-17 20:22:54
tags: Modern_C++
categories: 資工
description: modern C++ Ch2.1, constexpr和nullptr
---

## 前言
C和C++是資工人求學歷程的良伴
不過學校教導的大多都是稍微舊一些的版本和語法
而語言本身其實也是不斷在演進變化的
有時候去Github看其他人開源的專案，才發現明明都是使用C++，對方的code卻是充滿了許多黑魔法黑科技
於是起了想要鑽研近代C++的念頭

我所看的reference是[Modern C++](https://github.com/changkun/modern-cpp-tutorial)，其內容可以在網路上免費瀏覽(under Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License)
目前希望在行有餘力之時可以整理一些筆記至部落格上

## Ch2.1 Constants
### nullptr
Traditionally, null and 0 are treated as same thing. Some compiler treat null as ((void *)0)

``` C
int foo(char *);
int foo(int);
```
* 在做function overload時，假如function call是 foo(NULL)，會call到下面的function(parameter是 integer的)，because NULL is considered as 0 in most compiler

為了解決這個問題，C++11 新增了 `nullptr` keyword，其目的是用來分別null pointer，使其不要和0混淆

### constexpr
const expression: 就是像3+5, 3/7之類的東西。這些東西的值是不會因其他因素改變的(在runtime和compile time都會保持一致)
所以如果可以在compile time的時候就先算好，可以增進program在執行時的performance

``` C++
constexpr int len=1*2*3;
int arr[len];    //It's legal.
```
C++11 provides constexpr to let the user explicitly declare that the function or object constructor will become a constant expression at compile time.

constexpr也可以套用在遞回函式

``` C
constexpr int len_foo_constexpr() {
    return 5;
}
constexpr int fibonacci(const int n) {
    return n == 1 || n == 2 ? 1 : fibonacci(n-1) + fibonacci(n-2);
}
``` 
