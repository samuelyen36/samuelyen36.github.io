---
title: Modern C++ - Ch3.1 Lambda Expression
date: 2021-07-27 14:48:39
tags: Modern_C++
categories: 資工
description: modern C++ Ch3.1, Lambda Expression
---

# Ch3.1 Lambda Expression
[extra reference](https://blog.gtwang.org/programming/lambda-expression-in-c11/)

在C++11標準中，新增 Lambda Expression 這個功能。Lambda Expression 做的事情類似於一個匿名的函示(Anonymous function)，即不直接以 Function name 去 call 它，而是以一個類似於 inline 的形式去 implement。

## Basic Syntax
Lambda Expression 的 basic systax 如下
```C++
[capture list] (parameter list) mutable(optional) exception attribute -> return type {
// function body
}
```

* `[capture list]`: 可以去抓取這個scope中的 varialbe的數值，並決定將會以何種形式(call by value or call by reference)傳送
例如，若我們後續要 capture 兩個 variable 進入這個 Lambda Expression，分別是 `int x` and `int y`
    * `[]` (跟傳送兩個參數的範例無關): 這個 Lambda Expression不會capture 任何 variable
    * `[=]`: All the variables are captured by value.
    * `[&]`: All the variables are captured by reference.
    * `[x, &y]`: Variable x are captured by value, while variable y are captured by reference.
    * `[&x, y]`: Variable x are captured by reference, while variable y are captured by value.
    * `[=, &y]`: Variable y are captured by reference, while the other variables are captured by value
    * `[&, =x]`: Variable x are captured by value, while the other variables are captured by reference.
    
* `(Parameter list)`: 定義這個 Lambda Expression 的參數，跟一般的 Function declaration 中的 Parameter list 雷同
    * Lambda Expression 不強制要傳入參數，如果不傳入參數的話， Parameter list 包括小括號都可以省略

* `mutable`: 讓 Lambda Expression 可以直接修改外部利用 capture by value 進來的 variable

* `exception attribute`: exception specification，`throw`。
    * 若這個 Lambda Expression 沒有用到例外功能，可以直接整個省略
    * More information about exception: [w3school](https://www.w3schools.com/cpp/cpp_exceptions.asp)

* `return type`: Return type of Lambda Expression.


## Example
```C++
void lambda_value_capture() {
    int value = 1;
    auto copy_value = [value] {    //capture value
        return value;
    };        //declaration of lambda expression
    value = 100;
    auto stored_value = copy_value();
    std::cout << "stored_value = " << stored_value << std::endl;
    // At this moment, stored_value == 1, and value == 100.
    // Because copy_value has copied when its was created.
}
```


```C++
#include <iostream>
using namespace std;
int main() {
  auto lambda = []() { cout << "Hello, Lambda" << endl; };
  lambda();
}
```

```C++
#include <iostream>
using namespace std;
int main() {
    auto sum = [](int x, int y) -> int { return x+y;};
    cout<<sum(3,5)<<endl;
}
```


