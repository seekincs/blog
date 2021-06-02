---
author: "trayvonpan"
title: "C++ 11/14/17/20 关键特性"
date: 2020-10-23T20:56:54+08:00
draft: false
description: ""
tags: ["C++"]
toc: true
---

都说 C++ 是 C 语言的超集，其实不然。C++ 的许多语言特性设计还是与 C 语言有很大差别的，特别是现代 C++，已经涌现了诸多崭新的语法，C++ 语言也不断进化着。要认识到 C++ 语言是复杂的，掌握基本的语法可能很简单，但要写出具体的应用还得结合操作系统、计算机网络等技术。

注：本文参考了欧长坤著的[<b><i>《现代 C++ 教程：高速上手 C++ 11/14/17/20》</i></b>](https://changkun.de/modern-cpp/)，部分文字、代码版权归原作者所有。

<!--more-->

 ## 关键字

 ### nullptr

传统 C++ 会把 NULL 和 0 视为同一种东西。有些编译器将其 NULL 定义为 ((void*)0) ，有些则直接定义为 0 。这在函数重载的时候就可能会出现错误。C++11 引入了 nullptr 关键字解决了这个问题，用 nullptr 来区分空指针和 0 。

### constexpr

这是 C++11 开始提出的关键字，**用来声明一些编译时的常量或者常量函数**。之所以做这个，是因为如果 C++ 编译器在编译时期知道常量表达式的结果，那么编译器就会对其进行优化。如果编程者能确定一个函数在编译时期是常量，可以加 constexpr 修饰，以告诉编译器：请在编译时期优化这个常量表达式。
```cpp
constexpr int get_five() {
  return 5;
}
// Create an array of 12 integers. Valid C++11
int some_value[get_five() + 7]; 
```

这里容易和 const 混淆。 const 其实是具有双重语义的，它只是说一个量是 readonly ，或者说是一个运行时的常量，但是在程序中（编译时期）可能是个变量。

```cpp
void do_something(const& string t) {
  // 不允许对字符串t进行更改，但是const修饰的参数t是个变量，
  // 运行时才能确定，编译器保证运行时期t不会发生变化
}
```

 ### auto

这个关键字其实在 C++98 已经引入了，不过当时是用来声明变量是一个自动变量，自动变量具有自由的生命周期，与其对应的是 register 关键字。这基本是多余的，很少场景能用得上。C++11 对 auto 赋予了新的语义，用以在定义变量的时候根据变量的初始值为变量匹配正确的类型，不需要编程者显式指出。比如在使用迭代器遍历容器的时候就很方便。

```cpp
vector<string, map<string, int>> v;
// 传统方式
vector<string, map<string, int>>::iterator it = v.begin();
for (; it != v.end(); ++it) {
  // do something
}
// 使用auto关键字
for (auto it = v.begin(); it != v.end(); ++it) {
  // do something
}
```

 ### decltype

这个关键字也可以进行类型推断，和 auto 的区别是， auto 只能对变量的类型进行推断，而 decltype 能对一个表达式的类型进行推断。例如：

```cpp
auto x = 1;
auto y = 2;
decltype(x+y) z;
```

## 语言特性

 ### 增强 for 循环

这个有时候也叫**区间迭代**。不少语言如 Python 、 Java 等都提供了这种迭代方式。使用起来也非常简单。比如遍历容器的时候还可以这么写：

```cpp
vector<string, unordered_map<string, int>>::iterator it = v.begin();
for(auto it : v) {
	// do something
}
```  

 ### 初始化列表

 C++11 使用初始化列表统一了变量的初始化方式。这样，普通变量、结构体、对象，都可以使用 {} 进行初始化。

```cpp
class Foo {
  private:
    vector<int> v;
  public:
    Foo(initializer_list<int> init_v) {
        for (const auto &it : v) {
            v.push_back(it);
        }
    }
};

struct Node {
    int val;
    Node *next;
};

int main(int argc, char const *argv[]) {
    // 统一的初始化方式
    int a{3};
    double b{3.14};
    int arr[]{1, 2, 3, 4};
    Node n{99, nullptr};
    Foo foo{1};
    return 0;
}
```

 ### 结构化绑定

总所周知， C++ 的函数只能返回一个值。（当然，将多个值封装成结构体或者类这个另当别论）。 C++11 可以使用 tuple （就是所谓的元祖）比较方便地实现返回多个值，但是遗憾的是， C++11/14 都没有提供相关的方法来解析返回的元祖中的数据。 C++17 解除了这个限制，提出了一种结构化绑定的方式，将一个初始化的容器（或者说对象）里的子对象按照名字取出来。这个类似于 JavaScript 里面对象的[解构赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)。

```cpp
#include <iostream>
#include <tuple>

using namespace std;

tuple<int, double, string> getValue() { 
    return make_tuple(1, 3.14, "nick"); 
}

int main(int argc, char const *argv[]) {
  	// 将元祖中的三个值分别绑定到三个变量行x，y，z
    auto [x,y,z] = getValue();
    cout << x << " " << y << " " << z << endl;
    return 0;
}
```

### 类型别名

通常，我们可以使用 typedef 来为一个类型制定别名，但是有时候这种写法会稍微有点绕。如果使用 using （ C++11 引入），代码会更加简洁。

```cpp
typedef unsigned int ui;
// int_arr相当于int[16]
// typedef int int_arr[16];
using int_arr = int[16];
```

 ### 尖括号合法化

传统的 C++ ，如果存在嵌套模板，那么尖括号必须使用空格分开，否则一律当作右移运算符处理。但是在 C++11 连续的右尖括号是合法的。

```cpp
// 旧版本的C++需要将连续的右尖括号分开
vector<vector<int> > v1;
// 新版本的这样写也可以编译通过
vector<map<int, vector<int>>> v2;
```

### 变长参数模板

C++11  加入了新的表示方法， 允许任意个数、任意类别的模板参数，同时也不需要在定义时将参数的个数固定。另外，可以使用  sizeof... 来计算参数的个数。

```cpp
template<typename... Ts>
void magic(Ts... args) {
    std::cout << sizeof...(args) << std::endl;
}
```

 ### 显式虚函数重载

 C++11 引入 final ，override 这两个关键字，用来避免虚函数重载可能发生的一些隐秘问题。

- 当重载虚函数时，引入  override  关键字将显式的告知编译器进行重载。

- final  则是为了防止类被继续继承以及终止虚函数继续重载引入的。


 ### 显式禁用默认函数

C++11 提供了一种方案，允许编译器生成的默认构造函数与自定义的构造函数共存。

```cpp
class Magic {
    public:
    // 显式声明使用编译器生成的构造
    Magic() = default; 
    // 显式声明拒绝编译器生成构造
    Magic& operator=(const Magic&) = delete; 
    Magic(int magic_number);
}
```

 ### Lambda 表达式

这是一种匿名函数机制，其实很多语言都有提供。Lambda 表达式具有如下形式。

```cpp
[捕获列表](参数列表) mutable(可选) 异常属性 -> 返回类型 {
  // 函数体
}
// 值捕获
auto copy_value = [value] {
        return value;
};
// 引用捕获
auto copy_value = [&value] {
        return value;
};
```

捕获列表分为两种：
- 值捕获（[=]），提供变量的拷贝，对原变量无影响
- 引用捕获（[&]），可能会修改原变量

