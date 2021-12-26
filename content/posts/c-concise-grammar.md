---
author: "Trayvon Pan"
title: "C 语言回顾"
date: 2018-10-17T21:00:15+08:00
draft: false
description: ""
tags: ["C++"]
toc: true
---

作为常年霸占 [TIOBE](https://www.tiobe.com/tiobe-index/) 榜首的 C 语言，其地位已经不必多说。正如流传比较广泛的一句很贴切的话所说的：<i><b>“所有比 C 语言低级的语言（汇编语言、机器语言），都不足以抽象出一个完整的计算机系统；所有比 C 语言高级的语言（Java、Python、PHP），都可以使用 C 语言来实现。” </b></i>C 语言的语法并不难，难的是指针、内存管理和实际的应用。C 语言的应用场景非常广泛，操作系统、编译器、数据库、嵌入式系统等等，基本都是 C 的天下。

<!--more-->

## Hello, World!

### 介绍

`C`是一种通用的编程语言，广泛用于系统软件与应用软件的开发。于`1969`年至`1973`年间，为了移植与开发`UNIX`操作系统，由**丹尼斯·里奇**与**肯·汤普逊**，以`B`语言为基础，在贝尔实验室设计、开发出来的。

`C`语言具有高效、灵活、功能丰富、表达力强和较高的可移植性等特点，在程序设计中备受青睐，成为最近 25 年使用最为广泛的编程语言。当前，`C`语言编译器普遍存在于各种不同的操作系统中，例如`Microsoft Windows`、`macOS`、`Linux`、`Unix`等。`C`语言的设计影响了众多后来的编程语言，例如`C++`、`Objective-C`、`Java`、`C#`等。

二十世纪八十年代，为了避免各开发厂商用的`C`语言的语法产生差异，由美国国家标准局为`C`语言订定了一套完整的国际标准语法，称为`ANSI C`，作为`C`语言的标准。二十世纪八十年代至今的有关程序开发工具，一般都支持匹配`ANSI C`的语法。

### 第一个程序

首先安装`GCC`编译器。

```bash
$ sudo apt install gcc
```

创建`hello.c`文件。

```c
#include <stdio.h>
int main() {
  	printf("Hello, World!");
    return 0;
}
```

每个`C`程序都使用库，以提供执行必要函数的可能。比如，最基础的函数叫做`printf`(用来打印内容到屏幕)，就是定义在`stdio.h`头文件中的。

`main`函数提供了运行的入口。`int`关键字指明了`main`函数将返回一个简单的整数。`return`语句返回的数字将直接地指明我们的程序是否成功地工作。整数`0`说明运行正常，而非`0`的整数意味着程序可能出现了某种错误。

注意：`C`语言程序的每一行都必须以分号(`;`)结束。

编译，运行。

```bash
$ gcc hello.c && ./a.out
```

输出：

```bash
Hello, World!
```

## 变量和类型

### 数据类型

*注意：以下是典型的数据位长和范围。编译器可能使用不同的数据位长和范围。请参考具体的参考手册。*

在标准头文件`limits.h` 和 `float.h`中说明了基础数据的长度。`float，double和long double`的范围就是在[IEEE 754](https://zh.wikipedia.org/wiki/IEEE_754)标准中提及的典型数据。

|        关键字        |           位长(字节)           |                             范围                             |   格式化字符串   |
| :------------------: | :----------------------------: | :----------------------------------------------------------: | :--------------: |
|        `char`        |           `1 bytes`            |           `-128..127（或0..255，与体系结构相关）`            |       `%c`       |
|   `unsigned char`    |           `1 bytes`            |                           `0..255`                           |    `%c, %hhu`    |
|    `signed char`     |           `1 bytes`            |                         `-128..127`                          | `%c, %hhd, %hhi` |
|        `int`         | `2 bytes(16位系统) 或 4 bytes` |          `-32768..32767 或 -2147483648..2147483647`          |     `%i, %d`     |
|    `unsigned int`    |      `2 bytes 或 4 bytes`      |                 `0..65535 或 0..4294967295`                  |       `%u`       |
|     `signed int`     |      `2 bytes 或 4 bytes`      |          `-32768..32767 或 -2147483648..2147483647`          |     `%i, %d`     |
|     `short int`      |           `2 bytes`            |                       `-32768..32767`                        |    `%hi, %hd`    |
|   `unsigned short`   |           `2 bytes`            |                          `0..65535`                          |      `%hu`       |
|    `signed short`    |           `2 bytes`            |                       `-32768..32767`                        |    `%hi, %hd`    |
|      `long int`      |      `4 bytes 或 8 bytes`      | `-2147483648..2147483647 或 -9223372036854775808..9223372036854775807` |    `%li, %ld`    |
|   `unsigned long`    |      `4 bytes 或 8 bytes`      |          `0..4294967295 或 0..18446744073709551615`          |      `%lu`       |
|    `signed long`     |      `4 bytes或 8 bytes`       | `-2147483648..2147483647 或 -9223372036854775808..9223372036854775807` |    `%li, %ld`    |
|     `long long`      |           `8 bytes`            |         `-9223372036854775808..9223372036854775807`          |   `%lli, %lld`   |
| `unsigned long long` |           `8 bytes`            |                  `0..18446744073709551615`                   |      `%llu`      |
|       `float`        |           `4 bytes`            |              `2.939x10−38..3.403x10+38 (7 sf)`               |   `%f, %e, %g`   |
|       `double`       |           `8 bytes`            |             `5.563x10−309..1.798x10+308 (15 sf)`             |  `%lf, %e, %g`   |
|    `long double`     |     `10 bytes或 16 bytes`      |         `7.065x10-9865..1.415x109864 (18 sf或33 sf)`         | `%Lf, %Le, %Lg`  |

注意：`C`没有布尔类型。但是可以使用下面的方法定义：

```c
#define BOOL char
#define FALSE 0
#define TRUE 1
```

### 定义变量

对于整数，通常使用类型`int`，在计算机里面占据一个`word`的大小(`4 Bytes`)。

定义变量`foo`和`bar`，需要使用下列语法：

```c
int foo;
int bar = 1;
```

变量`foo`可以被使用，但没有初始化，它实际上存储的是什么并不确定。变量`bar`包含了整数`1`。

假设`a`，`b`，`c`，`d`和`e`是变量，在下面使用加减乘除操作，以及将一个新的值给`a`：

```c
int a = 0, b = 1, c = 2, d = 3, e = 4;
a = b - c / b + d * e;
printf("%d", a); /* will print 1-2/1+3*4 = 11 */
```

## 数组

数组是一种特殊的变量，可以包含超过一个**相同类型**的值，通过索引来访问。

```c
/* defines an array of 10 integers */
int numbers[10];
```

注意：`C`中的数组是以`0`为第一个索引的，这意味着如果定义了 一个大小为`10`的数组，数组的下标是`0`到`9`，`numbers[10]`不是一个实际的值。

```c
int numbers[10];

/* populate the array */
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
numbers[3] = 40;
numbers[4] = 50;
numbers[5] = 60;
numbers[6] = 70;

/* print the 7th number from the array, which has an index of 6 */
printf("The 7th number in the array is %d", numbers[6]);
```

## 多维数组

`C`可以创建和使用多维数组。

```c
type name[size1][size2]...[sizeN];
```

`type`：数组元素的类型

比如，这样：

```c
int foo[1][2][3];
```

或者，这样：

```c
char vowels[1][5] = {
    {'a', 'e', 'i', 'o', 'u'}
};
```

### 二维数组

多维数组最简单的形式是二维数组。

```c
type arrayName[x][y];
```

**`type`**可以是`C`的任何数据类型(`int,long,long long,double,etc`)，**`arrayName`**是一个合法的`C`标识符或者变量。二维数组也可以看成是`x`行`y`列的表格，一个3行4列的数组可以表示如下：

![](array_3_4.jpg)

在这种情况下，数组中的每一个元素都可以用`a[i][j]`来表示，其中`a`是数组名，`i,j`是该元素在数组中的唯一下标。

其实，并不一定要指定数组`a`的行数，可以像下面这样定义：

```c
char vowels[][5] = {
    {'A', 'E', 'I', 'O', 'U'},
    {'a', 'e', 'i', 'o', 'u'}
};
```

编译器已经知道这是一个二维数组，指定列数之后，行数也就确定下来了！，编译器可能是智能的，它知道有多少个整数、字符或者浮点数，无论定义多少维。

### 初始化二维数组

多维数组可以使用大括号(`{}`)来初始化每一行。下面是一个3行4列的数组。为了更加简化，可以不写行数把它留空，编译照样通过。

```c
int a[3][4] = {  
   {0, 1, 2, 3} ,   /*  initializers for row indexed by 0 */
   {4, 5, 6, 7} ,   /*  initializers for row indexed by 1 */
   {8, 9, 10, 11}   /*  initializers for row indexed by 2 */
};
```

内部指定每一行的大括号是可选的。下面的初始化与之前的等价。

```c
int a[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};
```

### 访问二维数组中的元素

二维数组中的元素是通过下标访问的，即数组的行标和列标。

```c
int val = a[2][3];
```

上面的语句将会从数组中取出第3行第4列的元素。

## 条件

### 作出决策

这里是一个C语言的判断结构。

```c
int target = 10;
if (target == 10) {
    printf("Target is equal to 10");
}
```

### if 语句

`if`语句检查表达式的真假(`true `or `false`)，然后根据结果执行不同的代码。

使用`==`来判断两个变量是否相等，就像第一个例子那样。

不等运算符也可以用在条件表达式中。比如：

```c
int foo = 1;
int bar = 2;

if (foo < bar) {
    printf("foo is smaller than bar.");
}

if (foo > bar) {
    printf("foo is greater than bar.");
}
```

当计算的条件表达式为`false`时，可以使用`else`关键字来执行其他代码。

```c
int foo = 1;
int bar = 2;

if (foo < bar) {
    printf("foo is smaller than bar.");
} else {
    printf("foo is greater than bar.");
}
```

有时会得到超过一个选择，这时，使用多个`if else`来完成。

```c
int foo = 1;
int bar = 2;

if (foo < bar) {
    printf("foo is smaller than bar.");
} else if (foo == bar) {
    printf("foo is equal to bar.");
} else {
    printf("foo is greater than bar.");
}
```

如果有必要，可以使用多个嵌套`if else`语句。

```c
int peanuts_eaten = 22;
int peanuts_in_jar = 100;
int max_peanut_limit = 50;

if (peanuts_in_jar > 80) {
    if (peanuts_eaten < max_peanut_limit) {
        printf("Take as many peanuts as you want!\n");
    }
} else {
    if (peanuts_eaten > peanuts_in_jar) {
        printf("You can't have anymore peanuts!\n");
    }
    else {
        printf("Alright, just one more peanut.\n");
    }
}
```

两个或者多个条件表达式也可以同时计算。判断两个表达式是否都是真(`true`)，使用`&`，判断它们是否至少有一个为真，使用`||`。

```c
int foo = 1;
int bar = 2;
int moo = 3;

if (foo < bar && moo > bar) {
    printf("foo is smaller than bar AND moo is larger than bar.");
}

if (foo < bar || moo > bar) {
    printf("foo is smaller than bar OR moo is larger than bar.");
}
```

非操作符(`&`)可以用来取反：

```c
int target = 9;
if (target != 10) {
    printf("Target is not equal to 10");
}
```

## 字符串

### 定义字符串

`C`中的`string`实际上是字符数组。可以使用指针来定义简单的字符串。

```c
char * name = "John Smith";
```

这种方法创建的`string`只能读。如果希望在字符串上操作，则需要将它定义为字符数组：

```c
char name[] = "John Smith";
```

空的中括号`[]`告诉编译器来动态地计算数组的大小。

```c
char name[] = "John Smith";
/* is the same as */
char name[11] = "John Smith";
```

即使字符串`John Smith`正好只有`10`个字符，但我们还必须在定义的时候加上`1`个。这是由字符串的表示法决定的。`C`中字符串的结尾有一个特殊字符`\0`表示字符串的结束，但这个字符不会纳入字符串长度的计算中，只是便于编译器处理罢了。

### 使用 printf 来格式化字符串

使用`printf`函数可以按照某种格式输出内容。

```c
char * name = "John Smith";
int age = 27;

/* prints out 'John Smith is 27 years old.' */
printf("%s is %d years old.\n", name, age);
```

注意：在打印字符串的时候，必须增加`\n`来换行，因为`printf`函数不是默认换行的。

### 字符串的长度

函数`strlen`传入字符串的地址，返回其长度。

```c
char * name = "Nikhil";
printf("%d\n",strlen(name));
```

### 字符串比较

函数`strncmp`比较两个字符串，参数是两个字符串，以及要比较的长度；如果相等则返回`0`，若不同则返回其他数字。也有一种不太安全的版本叫`strcmp`，但不推荐使用。

```c
char * name = "John";

if (strncmp(name, "John", 4) == 0) {
    printf("Hello, John!\n");
} else {
    printf("You are not John. Go away.\n");
}
```

### 字符串拼接

函数`strcat`在后面增加`n`个`src`中字符到`dest`中，其中，`n`是`min(n,length(src))`；参数是源字符串`src`和目的字符串`dest`，以及要添加的最大字符数量。

```c
char dest[20]="Hello";
char src[20]="World";
strncat(dest,src,3);
printf("%s\n",dest);
strncat(dest,src,20);
printf("%s\n",dest);
```

## For循环

`C`中的`for`循环是很直接的。当一段代码需要多次执行时使用循环来解决，`for`循环使用一个迭代变量，通常表示为`i`。

对`for`循环给出如下定义：

- 用初始值初始化迭代变量
- 检查条件，判断是否已经到达最后的值
- 改变迭代变量的值

比如说，如果希望迭代一段代码`10`次，可以这样写：

```c
int i;
for (i = 0; i < 10; i++) {
    printf("%d\n", i);
}
```

注意：在`for`循环头部声明迭代变量是`C++`的语法。

这个代码快将打印数字`0`到`9`。

`for`循环可以用来遍历数组。比如，若要计算数组内元素的总和，使用迭代变量`i`作为数组索引：

```c
int array[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
int sum = 0;
int i;

for (i = 0; i < 10; i++) {
    sum += array[i];
}

/* sum now contains a[0] + a[1] + ... + a[9] */
printf("Sum of the array is %d\n", sum);
```

## While 循环

`while`循环与`for`循环很像，但功能更少。当条件为`true`时，`while`循环会不断地执行内部的代码块。比如说，下面的代码将会正好执行`10`次。

```c
int n = 0;
while (n < 10) {
    n++;
}
```

`while`循环也可以无限地执行，如果条件永远都是`true`的话。在`C`中，所有非`0`的值都是`true`。

```c
while (1) {
   /* do something */
}
```

### 循环的控制

有两种循环的控制方法，`break`和`continue`。

`break`将结束整个循环，即使循环没有完全地执行指定的次数。

```c
int n = 0;
while (1) {
    n++;
    if (n == 10) {
        break;
    }
}
```

`continue`语句用来跳过当次循环。对于一次迭代，`continue`以下的语句将不会被执行。

下面的例子，`continue`导致了某些情况下`printf`函数将被忽略，只有偶数才被输出。

```c
int n = 0;
while (n < 10) {
    n++;

    /* check that n is odd */
    if (n % 2 == 1) {
        /* go back to the start of the while block */
        continue;
    }

    /* we reach this code only if n is even */
    printf("The number %d is even.\n", n);
}
```

## 函数

`C`函数是简单的，但由于`C`的工作方式，函数的功能有一些限制。

- 函数接收固定或者不定数量的参数
- 函数**只能返回一个值，或不返回任何值**。

在`C`中，实际参数按值的方式复制，不能改变在函数外部的变量的值。

```c
int foo(int bar) {
    /* do something */
    return bar * 2;
}

int main() {
    foo(1);
}
```

函数`foo`接收一个整型参数`bar`，将其乘以2，然后返回。

调用函数`foo`采用以下语法，传入参数`bar`的值是1。

```c
foo(1);
```

在`C`中，函数必须先声明再使用。函数可以先在程序开始处或者头文件中声明，然后再实现。

使用函数的正确方法如下：

```c
/* function declaration */
int foo(int bar);

int main() {
    /* calling foo from main */
    printf("The value of foo is %d", foo(1));
}

int foo(int bar) {
    return bar + 1;
}
```

也可以创建一个没有返回值的函数，使用关键字`void`：

```c
void moo() {
    /* do something and don't return a value */
}

int main() {
    moo();
}
```

## Static

`static`是C语言关键字，可以用来修饰变量和函数。

### 什么是静态变量？

默认地，变量在定义的时候作用域都是局部的。将变量定义为`static`可以将其作用域提升到整个文件；结果，这些`static`变量可以在文件内部访问。

考虑下面的程序，计算所参与的跑步者的数量。

```c
#include<stdio.h>
int runner() {
    int count = 0;
    count++;
    return count;
}

int main()
{
    printf("%d ", runner());
    printf("%d ", runner());
    return 0;
}
```

`count`**不会被更新**，因为当函数完成的时候，变量`count`会从内存中移除。但是，当使用`static`变量时，情况将会不一样，整个文件使用同一个`count`，更改也会被任何函数发现。

```c
#include<stdio.h>
int runner()
{
    static int count = 0;
    count++;
    return count;
}

int main()
{
    printf("%d ", runner());
    printf("%d ", runner());
    return 0;
}
```

### 什么是静态函数？

在`C`中，函数默认都是全局的。如果将函数定义为`static`，它的作用域将降低到包含该函数的文件。静态函数定义如下：

```c
static void fun(void) {
   printf("I am a static function.");
}
```

### 静态 v.s. 全局

静态变量只在包含它的文件内部有效，而全局的变量可以在文件外部访问。

## 指针

### C中的地址

假设`var`是程序中的一个变量，那么`&var`给出的是变量在内存中的地址。其中，`&`是引用操作符。`scanf`、`printf`函数就是一个例子。

```c
#include <stdio.h>
int main() {  
	int var = 5;  
    //scanf("%d", &var);
    printf("Value: %d\n", var);  
    printf("Address: %u", &var);  /*Notice, the ampersand(&) before var.*/  	return 0;
}
```

输出：

```bash
Value: 5 
Address: 2686778
```

可见，值`5`被存储在内存地址为`2686778`的地方。`var`只是那个地址的名字。

### 什么是指针？

`C`指针是一种有别于像`Java`、`Python`等其他编程语言的功能强大的特性，用来访问内存和操纵地址。指针可以说是一种简单的整型变量，存储着某个值的内存地址，而不是存储值本身。

### 字符串指针

下面这行代码：

```c
char* name = "John";
```

做了两件事情：

- 分配一个叫`name`的局部变量，`name`是一个指针，指向字符串`John`的第一个字符；也就是说，`name`变量存储的是字符`J`的内存地址。
- 将`John`分配到程序内存的某个地方(当然，在需要编译和执行之后)。

如果将`name`作为数组来访问也是可以的，由于它存放着字符串`John`的首个字符的地址，`name`的值就是字符`J`。实际上，可以像数组一样来访问`string`的每一个字符，直到到达最后一个终止字符`\0`。

### 解引用

**解引用是指可以获取一个指针变量的实际值，而不只是其内存地址。**比如，中括号`[0]`操作符，访问数组中的第一个元素。并且，由于数组实际上是指针，访问第一个元素相当于解引用。相应地，数组的名字存放的是数组的第一个元素的内存地址。可以使用星号(`*`)来解引用。

创建一个数组指向存放在内存中的不同元素：

```c
/* define a local variable a */
int a = 1;

/* define a pointer variable, and point it to a using the & operator */
int * pointer_to_a = &a;

printf("The value a is %d\n", a);
printf("The value of a is also %d\n", *pointer_to_a);
```

注意：使用`&`操作符来指向变量`a`。

然后使用解引用操作符来获取指针变量的内容；也可以改变所引用的内容。

```c
int a = 1;
int * pointer_to_a = &a;

/* let's change the variable a */
a += 1;

/* we just changed the variable again! */
*pointer_to_a += 1;

/* will print out 3 */
printf("The value of a is now %d\n", a);
```

## 结构体

### 使用

C中的结构体是一种特殊的的变量，可以存储不同类型的各种变量。

结构体用来：

- 序列化数据
- 在函数中通过一个参数传递多个值
- 构建数据结构，比如链表、二叉树等

结构体中最简单的例子就是`point`了，`point`包含了两个变量`x`和`y`，分别是点的横纵坐标。`point`将地定义为：

```c
struct point {
    int x;
    int y;
};
```

假设函数`draw`接收一个点的`x`和`y`坐标，并将其显示在屏幕上。如果没有`struct`，就必须指定两个参数，每个坐标需要一个：

```c
/* draws a point at 10, 5 */
int x = 10;
int y = 5;
draw(x, y);
```

使用`struct`，可以使用指针来传递参数：

```c
/* draws a point at 10, 5 */
struct point p;
p.x = 10;
p.y = 5;
draw(p);
```

注意：使用`.`操作符访问`point`的内部变量。

### typedef

`typedef`允许将类型重命名，以摆脱很长的`struct`类型的定义。`typedef`定义结构体示例如下：

```c
typedef struct {
    int x;
    int y;
} point;
```

这允许在程序中像这样定义一个`point`：

```c
point p;
```

结构体内部也可以包含指针，也就能够包含`string`，或者包含其他结构体，这就是`struct`的魅力所在。考虑下面定义的`vehicle`结构体。

```c
typedef struct {
    char* brand;
    int model;
} vehicle;
```

因为`brand`是一个字符指针，`vehicle`类型可以包含`string`。

```c
vehicle mycar;
mycar.brand = "Ford";
mycar.model = 2007;
```

## 函数按照引用传递

### 含义

如果函数按值传递，那么实际参数只是原来的值的一份拷贝。函数不会对原值做任何更改。

函数`addone`，将一个值加一，按照以下方式写则不会生效。

```c
void addone(int n) {
    n++;
}

int n;
printf("Before: %d\n", n);
addone(n);
printf("After: %d\n", n);
```

然而，这样却是可行的。

```c
void addone(int * n) {
    (*n)++;
}

int n;
printf("Before: %d\n", n);
addone(&n);
printf("After: %d\n", n);
```

第二个版本的`addone`不同之处在于，它接收了一个指针变量`n`作为参数，然后直接操纵它，因为它知道`n`在内存中的地址，会直接改变`n`指代的变量。

注意：当调用`addone`函数时，**必须传递一个引用的值，而不是变量本身**。

### 指向结构体的指针

先创建一个叫`call`的函数，它使`x`和`y`坐标都向正方向移动。注意，函数的参数不是两个坐标，而只是一个结构体的指针。

```c
void move(point* p) {
    (*p).x++;
    (*p).y++;
}
```

然而，如果希望解引用结构体并访问其内部的成员时，有一种更加简明的语法，这种方法在结构体中很常用。可以像下面那样重写函数，结果是等价的。

```c
void move(point* p) {
    p->x++;
    p->y++;
}
```

## 动态内存分配

动态内存分配是`C`的一个重要主题。它允许创建诸如链表、二叉树之类的复杂数据结构。动态分配内存无需在程序中指定变量的初始存储空间。

为了动态地分配一块内存，需要指定一个指针来存放**新分配的内存的首地址**。这样，就可以通过这个指针来访问这块内存，并且一旦不再需要这块内存，可以使用同样的指针来释放它。

假设现在要分配一段`person`结构体的内存。`person`定义如下：

```c
typedef struct {
    char* name;
    int age;
} person;
```

使用下面的语法，来分配一个新的`person`实例，并将地址传递给`myperson`变量。

```c
person* myperson = malloc(sizeof(person));
```

使用`->`表示法来访问`person`的成员。

```c
myperson->name = "John";
myperson->age = 27;
```

在使用完所分配的`struct`变量之后，可以使用`free`函数来释放那段内存。

```c
free(myperson);
```

注意：`free`并不删除`myperson`本身，它删除的是`myperson`指向的变量所在的内存占用。`myperson`变量仍然指向那段内存，应该确保程序不会再使用那段内存区域，不然可能会出现问题，因为那段区域可能已经是其他数据了。

## 数组和指针

在下面的代码中，`pc`存放字符变量`c`的地址。

```c
char c = 'A';
char* pc = &c;
```

这里，`c`是一个基本变量，可以存放单一的值。然而，由于数组可以存放多个相同类型的值，并且在连续分配的内存空间中。所以，能不能使用指针来完成？可以。

先从简单的代码开始，看它输出什么。

```c
char vowels[] = {'A', 'E', 'I', 'O', 'U'};
char *pvowels = &vowels;
int i;

// Print the addresses
for (i = 0; i < 5; i++) {
    printf("&vowels[%d]: %u, pvowels + %d: %u, vowels + %d: %u\n", i, &vowels[i], i, pvowels + i, i, vowels + i);
}

// Print the values
for (i = 0; i < 5; i++) {
    printf("vowels[%d]: %c, *(pvowels + %d): %c, *(vowels + %d): %c\n", i, vowels[i], i, *(pvowels + i), i, *(vowels + i));
}
```

上面代码的典型输出如下：（不同机器会存在差别）

```text
&vowels[0]: 4287605531, pvowels + 0: 4287605531, vowels + 0: 4287605531
&vowels[1]: 4287605532, pvowels + 1: 4287605532, vowels + 1: 4287605532
&vowels[2]: 4287605533, pvowels + 2: 4287605533, vowels + 2: 4287605533
&vowels[3]: 4287605534, pvowels + 3: 4287605534, vowels + 3: 4287605534
&vowels[4]: 4287605535, pvowels + 4: 4287605535, vowels + 4: 4287605535
vowels[0]: A, *(pvowels + 0): A, *(vowels + 0): A
vowels[1]: E, *(pvowels + 1): E, *(vowels + 1): E
vowels[2]: I, *(pvowels + 2): I, *(vowels + 2): I
vowels[3]: O, *(pvowels + 3): O, *(vowels + 3): O
vowels[4]: U, *(pvowels + 4): U, *(vowels + 4): U
```


可见，`&vowels[i]`给出了`vowels`数组中的第`i+1`个元素；并且，因为是一个字符数组，每一个元素占一个字节，所以其内存区域就简单地以字节分隔。此后，将数组的地址赋给指针`pvowels`。`pvowels+i`是一个合法的操作，虽然在正常的情况下这看起来没什么意义。特别地，在上面的输出中，`&vowels[i]`和`pvowels+i`是相同的。所以在使用这种方法输出上不要有疑虑。

其实，还可以用另外一种更加奇怪的表示方法：`vowels+i`，这也是一样的。另一方面，`pvowels+i`和`vowels+i`同样是返回数组`vowels`的第`i+1`个元素。为什么会这样？

这是因为数组本身也是一个常指针，指向数组的第一个元素。换句话说，`vowels`、`&vowels[0]`、和`vowels+0`都指向同样的位置。

### 数组的动态内存分配

指针可以用来访问数组，也可以用来动态分配内存。这两个方面还可以结合在一起。

```c
// Allocate memory to store five characters
int n = 5;
char *pvowels = (char *) malloc(n * sizeof(char));
int i;

pvowels[0] = 'A';
pvowels[1] = 'E';
*(pvowels + 2) = 'I';
pvowels[3] = 'O';
*(pvowels + 4) = 'U';

for (i = 0; i < n; i++) {
    printf("%c ", pvowels[i]);
}

printf("\n");

free(pvowels);
```

在上面的代码中，程序分配了连续的5个字节来存储字符数据。当使用数组下标遍历存储空间时，仿佛`pvowels`就是一个数组。

注意：`pvowels`实际上是一个指针，数组和指针实际上都是一样的事情。

所以这有什么用？当定义数组的时候，数组的大小是必须确定的。但是，在一些场景下，程序并不知道数组的所需要的实际长度，`programmer`可能就尽可能大地定义数组，这样会造成内存浪费。然而，使用动态分配内存的时候，就可以使程序的内存按需分配。并且，不再使用的内存也可以使用`free`函数来释放。在不好的方面，如果使用了动态内存分配，`programmer`就必须显式地调用`free`函数来释放内存，否则将有可能发生内存泄漏。

可以按照同样的方式为二维数组动态分配内存，这也可以拓展到多维数组。

```c
int nrows = 2;
int ncols = 5;
int i, j;

// Allocate memory for nrows pointers
char **pvowels = (char **) malloc(nrows * sizeof(char *));

// For each row, allocate memory for ncols elements
pvowels[0] = (char *) malloc(ncols * sizeof(char));
pvowels[1] = (char *) malloc(ncols * sizeof(char));

pvowels[0][0] = 'A';
pvowels[0][1] = 'E';
pvowels[0][2] = 'I';
pvowels[0][3] = 'O';
pvowels[0][4] = 'U';

pvowels[1][0] = 'a';
pvowels[1][1] = 'e';
pvowels[1][2] = 'i';
pvowels[1][3] = 'o';
pvowels[1][4] = 'u';

for (i = 0; i < nrows; i++) {
    for(j = 0; j < ncols; j++) {
        printf("%c ", pvowels[i][j]);
    }

    printf("\n");
}

// Free individual rows
free(pvowels[0]);
free(pvowels[1]);

// Free the top-level pointer
free(pvowels);
```

## 递归

函数调用自身时将发生递归现象。递归可以使代码更加整洁、优雅。但当递归树较深时，内存占用将很严重，时间复杂度也不佳。

使用递归的情况：

- 遍历递归的数据结构，比如链表、二叉树等
- 探索某些可能存在的路径

递归总是可以分为两个部分的。终止部分指示了递归到什么程度为止，而递归部分则解决子问题。

比如，下面的例子将使用递归加法来实现乘法：

```c
#include <stdio.h>

unsigned int multiply(unsigned int x, unsigned int y) {
    if (x == 1) {
        /* Terminating case */
        return y;
    }
    else if (x > 1) {
        /* Recursive step */
        return y + multiply(x-1, y);
    }

    /* Catch scenario when x is zero */
    return 0;
}

int main() {
    printf("3 times 5 is %d", multiply(3, 5));
    return 0;
}
```

## 链表

### 简介

链表是最好最简单的动态数据结构的例子，使用指针来实现。

一般地，链表作为一种线性表，可以在链表的任何地方，按需增长和收缩。

- 数据项可以在链表的中间增加或者移除
- 没有必要定义初始化大小

然而，链表也有一些缺点：

- 不能随机访问，链表的迭代必须从第一个元素开始，按顺序存取
- 需要动态第分配内存，增加了代码的复杂度，处理不好可能会造成内存泄漏
- 链表比数组有更大的头部，因为每一个节点都必须包含一个指针域，增加了空间的使用

### 什么是链表？

链表是一系列动态分配的节点的集合，每个节点都包含至少一个值与一个指针。指针总是指向链表的下一个成员，最后一个成员指向为`NULL`。

通常使用一个局部的指针变量来指向链表的第一个元素，如果指针为`NULL`，那就认为链表为空。

![img](linked_list.jpeg)

可以使用下面的方式定义链表的节点：

```c
typedef struct node {
    int val;
    struct node * next;
} node_t;
```

注意：使用`struct`来实现数据上的递归，即`node`内部的`next`依然是`node`类型。

现在可以使用节点了。创建一个叫`head`的局部变量指向链表的第一个节点。

```c
node_t * head = NULL;
head = malloc(sizeof(node_t));
if (head == NULL) {
    return 1;
}

head->val = 1;
head->next = NULL;
```

上面的代码只是创建了链表的第一个节点，还必须设置它的值，并且如果链表只有这个元素的话，应该让其下一个节点为空，即`next`域为`NULL`。

注意：最好总是检查`malloc`分配内存是否成功。

在链表的尾部增加一个节点，只需更改`next`域指向这个新的节点。

```c
node_t * head = NULL;
head = malloc(sizeof(node_t));
head->val = 1;
head->next = malloc(sizeof(node_t));
head->next->val = 2;
head->next->next = NULL;
```

可以按照这样的方式不断地增加元素，直到最后一个元素的`next`域为`NULL`。

### 遍历链表

先创建一个打印链表所有节点的函数。使用`current`指针来追踪当前要打印的节点。在打印完一个节点之后，将`current`的指针域指向下一个节点，然后再次打印，直到到达链表的结尾。(`next`域为空)

```c
void print_list(node_t * head) {
    node_t * current = head;

    while (current != NULL) {
        printf("%d\n", current->val);
        current = current->next;
    }
}
```

### 在表尾增加元素

为了迭代整个链表，使用一个叫做`current`的指针。在每一次循环中都把它推向下一个节点，直到指针到达最后。然后就可以在`current`位置插入一个节点。

```c
void push(node_t * head, int val) {
    node_t * current = head;
    while (current->next != NULL) {
        current = current->next;
    }

    /* now we can add a new variable */
    current->next = malloc(sizeof(node_t));
    current->next->val = val;
    current->next->next = NULL;
}
```

### 在表头增加元素

要在表头添加元素，只需按照以下的步骤来做：

- 创建一个节点并赋值
- 将新节点的`next`域指向表头
- 修改表头指针

使用函数来完成这些操作。为了修改表头的指针域，程序还需要传递一个指针的指针，这样才能修改`next`域本身。

```c
void push(node_t ** head, int val) {
    node_t * new_node;
    new_node = malloc(sizeof(node_t));

    new_node->val = val;
    new_node->next = *head;
    *head = new_node;
}
```

### 移除表头元素

要移除变量，只需要进行反操作：

- 新建一个临时变量存放头指针的下一个元素的信息
- 释放头指针指向的节点
- 将临时节点设置为头节点

以下是代码。

```c
int pop(node_t ** head) {
    int retval = -1;
    node_t * next_node = NULL;

    if (*head == NULL) {
        return -1;
    }

    next_node = (*head)->next;
    retval = (*head)->val;
    free(*head);
    *head = next_node;

    return retval;
}
```

### 移除表尾元素

移除链表的最后一个元素需要遍历整个链表，找到最后一个元素的地址，然后直接释放其内存空间，将上一个节点的`next`域修改为`NULL`。

注意：要找到最后一个元素，还必须额外设置一个变量来检查是否到达倒数第二个，这是关键。

```c
int remove_last(node_t * head) {
    int retval = 0;
    /* if there is only one item in the list, remove it */
    if (head->next == NULL) {
        retval = head->val;
        free(head);
        return retval;
    }

    /* get to the second to last node in the list */
    node_t * current = head;
    while (current->next->next != NULL) {
        current = current->next;
    }

    /* now current points to the second to last item of the list, so let's remove current->next */
    retval = current->next->val;
    free(current->next);
    current->next = NULL;
    return retval;

}
```

### 移除特定的元素

无论是通过索引还是节点值的方式给出要删除的节点，都必须遍历整个链表，找到需要删除的元素，然后删除它。移除的过程只需要修改指针域。

这是删除的算法：

- 迭代链表，找到要删除元素的前一个元素，使用一个临时指针来指向它
- 使用一个额外指针指向待删除的节点
- 将临时指针指向的节点的`next`域修改为下一个节点的下一个节点的地址
- 释放待删除节点的地址

下面是一些边界案例，在删除的时候需要注意。

```c
int remove_by_index(node_t ** head, int n) {
    int i = 0;
    int retval = -1;
    node_t * current = *head;
    node_t * temp_node = NULL;

    if (n == 0) {
        return pop(head);
    }

    for (i = 0; i < n-1; i++) {
        if (current->next == NULL) {
            return -1;
        }
        current = current->next;
    }

    temp_node = current->next;
    retval = temp_node->val;
    current->next = temp_node->next;
    free(temp_node);

    return retval;

}
```

## 联合体

`C`中的联合体和结构体比较像，结构体中的每个变量都有自己的存储空间，但联合体是多个变量使用同一个内存空间，因此，联合体的大小取决于最大的变量占用空间的大小。

所以，如果需要以不同的方式读取一个变量，比如，读取一个整数的每一个字节，可以这样做：

```c
union intParts {
  int theInt;
  char bytes[sizeof(int)];
};
```

这样就可以看到一个`int`类型的每个字节的内容。

```c
union intParts parts;
parts.theInt = 5968145; // arbitrary number > 255 (1 byte)

printf("The int is %i\nThe bytes are [%i, %i, %i, %i]\n",
parts.theInt, parts.bytes[0], parts.bytes[1], parts.bytes[2], parts.bytes[3]);

// vs

int theInt = parts.theInt;
printf("The int is %i\nThe bytes are [%i, %i, %i, %i]\n",
theInt, *((char*)&theInt+0), *((char*)&theInt+1), *((char*)&theInt+2), *((char*)&theInt+3));

// or with array syntax which can be a tiny bit nicer sometimes

printf("The int is %i\nThe bytes are [%i, %i, %i, %i]\n",
    theInt, ((char*)&theInt)[0], ((char*)&theInt)[1], ((char*)&theInt)[2], ((char*)&theInt)[3]);
```

有时候，可能有一个`struct`类型的操作，但不想像下面那样使用：

```c
struct operator {
    int intNum;
    float floatNum;
    int type;
    double doubleNum;
};
```

因为程序可能还有很多这样的变量，这种存储方式显然会浪费内存空间。所以，可以这样做：

```c
struct operator {
    int type;
    union {
      int intNum;
      float floatNum;
      double doubleNum;
    } types;
};
```

`operator`这个结构体现在的大小是`type`的大小再加上`types`联合体的最大的变量(`double`)的大小。用法如下。

```c
operator op;
op.type = 0; // int, probably better as an enum or macro constant
op.types.intNum = 352;
```

而且，如果不指定联合体的名字，甚至可以直接通过结构体访问！

```c
struct operator {
    int type;
    union {
        int intNum;
        float floatNum;
        double doubleNum;
    }; // no name!
};

operator op;
op.type = 0; // int
// intNum is part of the union, but since it's not named you access it directly off the struct itself
op.intNum = 352;
```

另外，一种可能更加有用的特征是，当有很多相同类型的变量的时候，`programmer`想同时使用名称(可读性)和索引(迭代需要)，在这种情况下，可以这样做：

```c
union Coins {
    struct {
        int quarter;
        int dime;
        int nickel;
        int penny;
    }; /* anonymous struct acts the same way as an anonymous union, members are on the outer container */
    int coins[4];
};
```

联合体变量是共享内存的！

```c
union Coins change;
for(int i = 0; i < sizeof(change) / sizeof(int); ++i)
{
    scanf("%i", change.coins + i); // BAD code! input is always suspect!
}
printf("There are %i quarters, %i dimes, %i nickels, and %i pennies\n",
    change.quarter, change.dime, change.nickel, change.penny);
```

## 指针运算

如前所述，指针其实是一个整型的变量，因此，可以对其进行基本的运算。

### 指针自增

就像任何变量一样，`++`操作符对那个变量执行增加`1`的操作。在这里**指针变量的自增将会使指针指向下一个内存地址**。结合数组的操作，来看一下这是如何实现的。

```c
#include <stdio.h>

int main() {
    int intarray[5] = {10,20,30,40,50};

    int i;
    for(i = 0; i < 5; i++)
        printf("intarray[%d] has value %d - and address @ %x\n", i, intarray[i], &intarray[i]);

    int *intpointer = &intarray[3]; /*point to the 4th element in the array*/
     //print the address of the 4th element
    printf("address: %x - has value %d\n", intpointer, *intpointer);

    //now increase the pointer's address so it points to the 5th elemnt in the array
    intpointer++; 
    //print the address of the 5th element
    printf("address: %x - has value %d\n", intpointer, *intpointer); 

    return 0;
}
```

### 指针自减

`--`操作实现指针指向上一个内存地址。

```c
#include <stdio.h>

int main() {
    int intarray[5] = {10,20,30,40,50};

    int i;
    for(i = 0; i < 5; i++)
        printf("intarray[%d] has value %d - and address @ %x\n", i, intarray[i], &intarray[i]);

    int *intpointer = &intarray[4]; //point to the 5th element in the array
    printf("address: %x - has value %d\n", intpointer, *intpointer); //print the address of the 5th element

    intpointer--; //now decrease the point's address so it points to the 4th element in the array
    printf("address: %x - has value %d\n", intpointer, *intpointer); //print the address of the 4th element

    return 0;
}
```

### 指针加法

可以将一个整数值加到一个指针变量上，就像下面这样。

```c
#include <stdio.h>

int main() {
    int intarray[5] = {10,20,30,40,50};

    int i;
    for(i = 0; i < 5; i++)
        printf("intarray[%d] has value: %d - and address @ %x\n", i, intarray[i], &intarray[i]);

    int *intpointer = &intarray[1]; //point to the 2nd element in the array
    printf("address: %x - has value %d\n", intpointer, *intpointer); //print the address of the 2nd element

    intpointer += 2; //now shift by two the point's address so it points to the 4th element in the array
    printf("address: %x - has value %d\n", intpointer, *intpointer); //print the addres of the 4th element

    return 0;
}
```

注意：这里的内存地址一下子就改变了`8`。为什么会这样？答案很简单，因为指针是一个`int`类型的指针，而`int`类型的大小是`4`个字节，所以指针每增加`1`，内存地址就增加`4`，上面的指针执行加`2`的操作，内存地址也就增加了`8`。

### 指针减法

同样地，可以对指针进行减法操作。

```c
#include <stdio.h>

int main() {
    int intarray[5] = {10,20,30,40,50};

    int i;
    for(i = 0; i < 5; i++)
        printf("intarray[%d] has value: %d - and address @ %x\n", i, intarray[i], &intarray[i]);

    int *intpointer = &intarray[4]; //point to the 5th element in the array
    printf("address: %x - has value %d\n", intpointer, *intpointer); //print the address of the 5th element

    intpointer -= 2; //now shift by two the point's address so it points to the 3rd element in the array
    printf("address: %x - has value %d\n", intpointer, *intpointer); //print the address of the 3rd element

    return 0;
}
```

### 其他操作

还有更多的操作，比如指针比较`>`，`<`，`==`。这个和常规变量的比较基本相同，只不过在这里比较的是内存地址而已。

## 函数指针

### 为什么指向一个函数？

明明可以简单地通过函数名来调用函数，为什么要使用一个指针来指向一个函数？！好问题。现在想一下，`programmer`需要使用一个`sort`函数来对数组进行排序，有时想对其升序有时降序，这该如何选择？函数指针！

### 函数指针语法

```c
void (*pf)(int);
```

这样定义可能看起来有点复杂。**重新阅读代码，并且尝试去理解指针到底指向哪里。**从外面开始读，`*pf`是一个指向了一个函数。`void`是函数的返回值。最后，`int`是函数的参数。

现在向函数里面插入指针，然后再次读代码。

```c
char* (*pf)(int*)
```

再次，`*pf`是函数指针，`char*`是函数的返回值，`int*`是参数的类型。

理论就到此为止。看一个实际的例子。

```c
#include <stdio.h>

void someFunction(int arg) {
    printf("This is someFunction being called and arg is: %d\n", arg);
    printf("Whoops leaving the function now!\n");
}

int main() {
    void (*pf)(int);
    pf = &someFunction;
    printf("We're about to call someFunction() using a pointer!\n");
    (pf)(5);
    printf("Wow that was cool. Back to main now!\n\n");
}
```

现在再考虑实现`sort`函数。这次不是对集合进行升序排序，而是定义自己的排序规则。

```c
#include <stdio.h>
#include <stdlib.h> //for qsort()

int compare(const void* left, const void* right) {
    return (*(int*)right - *(int*)left);
    // go back to ref if this seems complicated: 
    //http://www.cplusplus.com/reference/cstdlib/qsort/
}

int main() {
    int (*cmp) (const void* , const void*);
    cmp = &compare;

    int iarray[] = {1,2,3,4,5,6,7,8,9};
    qsort(iarray, sizeof(iarray)/sizeof(*iarray), sizeof(*iarray), cmp);

    int c = 0;
    while (c < sizeof(iarray)/sizeof(*iarray)) {
        printf("%d \t", iarray[c]);
        c++;
    }
}
```

现在，反思一下，为什么要使用函数指针？因为灵活。

## 预处理

凡是以`#`开头的均为预处理命令。

### 宏(Macro)

宏定义简单点说就是查找替换。考虑下面的代码。

```c
#include <stdio.h>
#define PRINT(x) (x) 
int main() 
{ 
	printf("%s",PRINT(x)); 
	return 0; 
} 
```

编译可以通过吗？试一下。

```bash
➜  Buffer gcc macro.c 
macro.c: In function ‘main’:
macro.c:5:20: error: ‘x’ undeclared (first use in this function)
  printf("%s",PRINT(x));
                    ^
macro.c:2:19: note: in definition of macro ‘PRINT’
 #define PRINT(x) (x)
                   ^
macro.c:5:20: note: each undeclared identifier is reported only once for each function it appears in
  printf("%s",PRINT(x));
                    ^
macro.c:2:19: note: in definition of macro ‘PRINT’
 #define PRINT(x) (x)
                   ^
```

很明显，编译器并不知道`x`的类型，而`printf`函数试图打印`x`的值，失败是不可避免的。

问题似乎已经找出来了，现在只需要将`x`改为字符串。宏定义如下：

```c
#define PRINT(x) ("x")
```

其实，和还有一种更加简洁的方法 。在`C`中，有一个`＃`指令，也称为“字符串化运算符”，能将其参数转换为字符串。所以上面的宏定义可以修改如下。

```c
#define PRINT(x) (#x) 
```

#### 无参宏

不带参数的宏。

```c
#include <stdio.h>
#define M (x*3+x*5)
int main()
{
    int s = 0;
    int x = 0;
    printf("input a number:");
    scanf("%d", &x);
    s = 3*M; // 3*(x*3+x*5)
    printf("s = %d\n", s);
}
```

注意：在宏定义时，其字符串要用小括号括起来，否则会产生出错误的编译，在运行时，得不到想 要的结果。

#### 有参宏

宏定义中出现参数。

```c
#include <stdio.h>
#define SSSV(s1, s2, s3, v)  s1=l*w; s2=l*h; s3=w*h; v=l*w*h
int main()
{
    int l = 3, w = 4, h = 5, sa, sb, sc, vv;
    SSSV(sa, sb, sc, vv);
    printf("sa = %d\tsb = %d\tsc = %d\tvv = %d\n", sa, sb, sc, vv);
    return 0;
}
```

#### 变长参数宏

与函数一样，可以将可变长度参数传递给宏。 为此，将使用以下预处理器标识符。 要在宏中支持变长参数，必须在宏定义中包含省略号（...）。 `__VA_ARGS__`预处理标识符负责提供给宏的可变长度参数替换。 连接运算符`##`(又名粘贴运算符)用于连接变量参数。

看一下例子。 下面的宏采用可变长度参数，如`printf`函数。 此宏用于错误记录。 宏打印文件名后跟行号，最后打印信息/错误消息。 第一个参数`prio`确定消息的优先级，即它是信息消息还是错误，“流”可以是“标准输出”或“标准错误”。 它在`stdr`流上的`stdout`和`ERROR`消息上显示`INFO`消息。

```c
#include <stdio.h> 

#define INFO 1 
#define ERR 2 
#define STD_OUT stdout 
#define STD_ERR stderr 

#define LOG_MESSAGE(prio, stream, msg, ...) do {\ 
						char *str;\ 
						if (prio == INFO)\ 
							str = "INFO";\ 
						else if (prio == ERR)\ 
							str = "ERR";\ 
						fprintf(stream, "[%s] : %s : %d : "msg" \n", \ 
								str, __FILE__, __LINE__, ##__VA_ARGS__);\ 
					} while (0) 

int main(void) 
{ 
	char *s = "Hello"; 

	/* display normal message */
	LOG_MESSAGE(ERR, STD_ERR, "Failed to open file"); 

	/* provide string as argument */
	LOG_MESSAGE(INFO, STD_OUT, "%s Nick", s); 

	/* provide integer as arguments */
	LOG_MESSAGE(INFO, STD_OUT, "%d + %d = %d", 10, 20, (10 + 20)); 

	return 0; 
} 
```

输出：

```bash
➜  Buffer ./a.out 
[ERR] : macro.c : 15 : Failed to open file 
[INFO] : macro.c : 18 : Hello Nick 
[INFO] : macro.c : 21 : 10 + 20 = 30 
```

### 条件编译

#### #if、#else、#elif 和 #endif

```c
#if constant1
  //Statement sequence 1...
#elif constant2
  //Statement sequence 2...
#elif constant3
  //Statement sequence 3...
#else
  //Statement sequence 4...
#endif
```

如果`constant1`为`true`，执行`Statement sequence 1...`所在块代码，否则执行其他。与条件语句非常相似。

#### #ifdef 和 #ifndef

```c
#ifdef identifier /* #ifndef behaves the opposite*/
  //Statement sequence 1...
#else
  //Statement sequence 2...
#endif
```

对于`#ifdef`，如果标识符`identifier`已经定义，执行`Statement sequence 1...`所在块代码，否则执行其他。而`#ifndef`恰好相反。

## 文件处理

### 为什么需要文件？

- 当程序终止退出时，所有的数据将会随之消失。将数据保存在文件中则不会。
- 如果有很多的数据需要处理，自然不会采用手动输入的方式。然而，可以预先将数据存储在文件中，`C`提供了直接访问文件的函数，操作起来很方便。
- 可以将文件中的数据在不同计算机之间移植。

### 文件类型

#### 文本文件

通常以`.txt`为后缀，只包含平白文本内容，可以轻松地编辑和删除，维护代价小，易读，但安全性较低。

#### 二进制文件

存储的并不是文本，而是其二进制表示。这可以存更多数据，可读性不强，但安全性较高。

### 文件操作

#### 打开和关闭

不管是文本文件还是二进制文件，文件的主要操作都有一下几种：

- 创建新的文件
- 打开存在的文件
- 关闭文件
- 从文件中读取并向其中写入内容

```c
FILE *fptr;
```

打开文件的`fopen`函数需要包含`stdio.h`库。

```c
ptr = fopen("fileopen","mode")
```

比如：

```c
fopen("E:\\cprogram\\newprogram.txt","w");

fopen("E:\\cprogram\\oldprogram.bin","rb");
```

熟悉典型的文件打开模式会很有帮助。

| File Mode | Meaning of Mode                | During Inexistence of file                   |
| :-------- | :----------------------------- | :------------------------------------------- |
| `r`       | 只读                           | 如果文件不存在，`fopen`返回`NULL`            |
| `rb`      | 只读(二进制形式)               | 如果文件不存在，`fopen`返回`NULL`            |
| `w`       | 只写                           | 文件如果存在，其内容将会被覆盖；不存在则创建 |
| `wb`      | 只写(二进制形式)               | 文件如果存在，其内容将会被覆盖；不存在则创建 |
| `a`       | 在文件末尾添加内容             | 如果文件不存在则被创建                       |
| `ab`      | 在文件末尾添加内容(二进制形式) | 如果文件不存在则被创建                       |
| `r+`      | 读、写                         | 如果文件不存在，`fopen`返回`NULL`            |
| `rb+`     | 读、写(二进制形式)             | 如果文件不存在，`fopen`返回`NULL`            |
| `w+`      | 读、写                         | 文件如果存在，其内容将会被覆盖；不存在则创建 |
| `wb+`     | 读、写(二进制形式)             | 文件如果存在，其内容将会被覆盖；不存在则创建 |
| `a+`      | 读、写                         | 如果文件不存在则被创建                       |
| `ab+`     | 读、写(二进制形式)             | 如果文件不存在则被创建                       |

注意：所有文件使用完毕之后都应该关闭。

```c
fclose(fptr); //fptr is the file pointer associated with file to be closed.
```

#### 读和写

##### 对于文本文件，使用函数 fprintf 和 fscanf 进行读写。

**使用 fprintf 向文件中写内容**

```c
#include <stdio.h>
#include <stdlib.h>
int main()
{
   int num;
   FILE *fptr;
   fptr = fopen("C:\\program.txt","w");
   if(fptr == NULL)
   {
      printf("Error!");   
      exit(1);             
   }
   printf("Enter num: ");
   scanf("%d",&num);
   fprintf(fptr,"%d",num);
   fclose(fptr);
   return 0;
}
```

程序读取一个整数然后将其存到文件`program.txt`中。

**使用`fscanf`从文件中读取内容**

```c
#include <stdio.h>
#include <stdlib.h>
int main()
{
   int num;
   FILE *fptr;
   if ((fptr = fopen("C:\\program.txt","r")) == NULL){
       printf("Error! opening file");
       // Program exits if the file pointer returns NULL.
       exit(1);
   }
   fscanf(fptr,"%d", &num);
   printf("Value of n=%d", num);
   fclose(fptr); 
  
   return 0;
}
```

程序从文件读取一个整数并将其打印到标准输出流。

另一些函数，比如`fgetchar`、`fputc`都可以以相同的方式使用。

##### 对于二进制文件，使用函数 fread 和 fwrite 进行读写。

```c
fwrite(address_data,size_data,numbers_data,pointer_to_file);
fread(address_data,size_data,numbers_data,pointer_to_file);
```

**使用`fwrite`向文件中写内容**

```c
#include <stdio.h>
#include <stdlib.h>
struct threeNum
{
   int n1, n2, n3;
};
int main()
{
   int n;
   struct threeNum num;
   FILE *fptr;
   if ((fptr = fopen("C:\\program.bin","wb")) == NULL){
       printf("Error! opening file");
       // Program exits if the file pointer returns NULL.
       exit(1);
   }
   for(n = 1; n < 5; ++n)
   {
      num.n1 = n;
      num.n2 = 5*n;
      num.n3 = 5*n + 1;
      fwrite(&num, sizeof(struct threeNum), 1, fptr); 
   }
   fclose(fptr); 
  
   return 0;
}
```

**使用`fread`从文件中读取内容**

```c
#include <stdio.h>
#include <stdlib.h>
struct threeNum
{
   int n1, n2, n3;
};
int main()
{
   int n;
   struct threeNum num;
   FILE *fptr;
   if ((fptr = fopen("C:\\program.bin","rb")) == NULL){
       printf("Error! opening file");
       // Program exits if the file pointer returns NULL.
       exit(1);
   }
   for(n = 1; n < 5; ++n)
   {
      fread(&num, sizeof(struct threeNum), 1, fptr); 
      printf("n1: %d\tn2: %d\tn3: %d", num.n1, num.n2, num.n3);
   }
   fclose(fptr); 
  
   return 0;
}
```

##### 使用函数 fseek 获取数据

如果文件比较大但需要访问某一个位置的内容，这时候可能需要遍历整个文件。这显然浪费时间和内存。更简单的方式是使用`fseek`函数。

```c
fseek(FILE * stream, long int offset, int whence)
```

第一个参数是文件指针，第二个是要找的偏移量，第三个是计算偏移量的开始位置。

| Whence   | Meaning                      |
| :------- | :--------------------------- |
| SEEK_SET | 偏移量位于文件的开始         |
| SEEK_END | 偏移量位于文件的最后         |
| SEEK_CUR | 偏移量从当前的游标位置开始算 |

下面是一个例子。

```c
#include <stdio.h>
#include <stdlib.h>
struct threeNum
{
   int n1, n2, n3;
};
int main()
{
   int n;
   struct threeNum num;
   FILE *fptr;
   if ((fptr = fopen("C:\\program.bin","rb")) == NULL){
       printf("Error! opening file");
       // Program exits if the file pointer returns NULL.
       exit(1);
   }
   
   // Moves the cursor to the end of the file
   fseek(fptr, -sizeof(struct threeNum), SEEK_END);
   for(n = 1; n < 5; ++n)
   {
      fread(&num, sizeof(struct threeNum), 1, fptr); 
      printf("n1: %d\tn2: %d\tn3: %d\n", num.n1, num.n2, num.n3);
      fseek(fptr, -2*sizeof(struct threeNum), SEEK_CUR);
   }
   fclose(fptr); 
  
   return 0;
}
```

## I/O

### printf 和 scanf

这两个函数的返回值是什么？

- `printf`返回要打印字符的数量，或者当输出、编码错误时返回一个负数

  ```c
   int printf(const char *format, ...);
  ```

  ```c
  // C program to demonstrate return value 
  // of printf() 
  #include <stdio.h> 
  
  int main()  { 
  	char st[] = "CODING"; 
  
  	printf("While printing "); 
  	printf(", the value returned by printf() is : %d", 
  									printf("%s", st)); 
  
  	return 0; 
  } 
  ```

  输出：

  ```bash
  While printing CODING, the value returned by printf() is : 6
  ```

  对于字符串输出，使用`puts`更加高效。`printf`的底层实现比`puts`复杂很多，并且用户在终端输入字符串时会产生安全问题。另外，`puts`会自动换行，如果不想这样，使用`fputs(str,stdout)`。

- `scanf` 如果在接收第一个参数之前发生错误，将返回`EOF`，否则返回输入的数量。

  ```c
  // C program to demonstrate return value 
  // of printf() 
  
  #include <stdio.h> 

  int main()  { 
  	char a[100], b[100], c[100]; 
  
  	// scanf() with one input 
  	printf("\n First scanf() returns : %d", 
  							scanf("%s", a)); 
  
  	// scanf() with two inputs 
  	printf("\n Second scanf() returns : %d", 
  					scanf("%s%s", a, b)); 
  
  	// scanf() with three inputs 
  	printf("\n Third scanf() returns : %d", 
  				scanf("%s%s%s", a, b, c)); 
  
  	return 0; 
  } 
  ```

  输入：

  ```bash
  Hey!
  I am
  Pan Zhao Wang
  ```

  输出：

  ```bash
  First scanf() returns : 1
  Second scanf() returns : 2
  Third scanf() returns : 3
  ```

还有其他形式的输出。

`sprintf`输出字符到给定的字符数组。

```c
int sprintf(char *str, const char *string,...); 
#include<stdio.h> 
int main() 
{ 
	char buffer[50]; 
	int a = 10, b = 20, c; 
	c = a + b; 
	sprintf(buffer, "Sum of %d and %d is %d", a, b, c); 

	// The string "sum of 10 and 20 is 30" is stored 
	// into buffer instead of printing on stdout 
	printf("%s", buffer); 

	return 0; 
} 
```

输出：

```bash
Sum of 10 and 20 is 30
```

`fprintf`将内容输出到文件。

```c
int fprintf(FILE *fptr, const char *str, ...);
```

### getchar、fgetc 和 getc

在`C`中，`getchar`、`fgetc`和`getc`的返回值都是`int`，不是`char`。

`getc`从给定的输入流读取**单个**字符，成功时返回该字符对应的整数，通常是`ASCII`码；失败时返回`EOF`。

```c
int getc(FILE *stream); 
// Example for getc() in C 
#include <stdio.h> 
int main() 
{ 
	printf("%c", getc(stdin)); 
	return(0); 
} 
```

`getchar`只能从标准输入流读取一个字符，也就是说，`getchar`和`getc(stdin)`等价。

```c
int getchar(void); 
// Example for getchar() in C 
#include <stdio.h> 
int main() 
{ 
	printf("%c", getchar()); 
	return 0; 
} 
```

`getch`通常出现在`MS-DOS`使用的像`Turbo C`之类的编译器的`conio.h`头文件中，注意它不是`C`标准库或者`ISO C`的一部分，或者说`getch`没有被`POSIX`定义。`getch`从键盘读取一个字符但没有使用任何缓冲。与此相似的还有`getche`。

```c
int getch();
// Example for getch() in C 
#include <stdio.h> 
#include <conio.h> 
int main() 
{ 
	printf("%c", getch()); 
	return 0; 
}
```

### fflush

`fflush`通常用于输出流。`fflush`可以清空输出缓冲，并把缓冲区的数据移控制台(标准输出流)或者磁盘(文件流)。以下是语法。

```c
fflush(FILE *ostream);

ostream points to an output stream 
or an update stream in which the 
most recent operation was not input, 
the fflush function causes any 
unwritten data for that stream to 
be delivered to the host environment 
to be written to the file; otherwise, 
the behavior is undefined.
```

`fflush`可以用在输入流吗？`C`标准是不允许的。但有些编译器，如微软的`Visual Studio`却允许。下面给出用例。

```c
// C program to illustrate flush(stdin) 
// This program works as expected only 
// in certain compilers like Microsoft 
// visual studio. 
#include <stdio.h> 
#include<stdlib.h> 
int main() 
{ 
	char str[20]; 
	int i; 
	for (i = 0; i<2; i++) 
	{ 
		scanf("%[^\n]s", str); 
		printf("%s\n", str); 

		// used to clear the buffer 
		// and accept the next string 
		fflush(stdin); 
	} 
	return 0; 
} 
```

## Socket API

### 什么是 Socket 编程？

`socket`编程是连接`Internet`中两个节点(两台主机)并进行通信的一种方式。在典型的`C/S`模式中，服务端被动地等待连接，而客户端主动请求连接。两者通过`TCP/IP`协议栈实现进程间的通信。

![img](client_server_mode.png)

### 通信

`Client`和`Server`都要经历一定的阶段才能建立连接。流程图如下。

![img](c_s_stages.jpg)

### 对于 Server

- 建立

  ```c
  int sockfd = socket(domain, type, protocol)
  ```

  - `sockfd`：`socket`描述符，是一个整数
  - `domain`：连接域，也是一个整数，`AF_INET`表示`IPv4`协议，而`AF_INET6`表示`IPv6`协议
  - `type`：连接类型。`SOCK_STREAM`表示`TCP`，`SOCK_DGRAM`表示`UDP`
  - `protocol`：`IP`值

- `setsockopt`：

  ```c
  int setsockopt(int sockfd, int level, int optname,  
                     const void *optval, socklen_t optlen);
  ```

  这个是可选的。

- 绑定

  ```c
  int bind(int sockfd, const struct sockaddr *addr, 
                            socklen_t addrlen);
  ```

  在`socket`建立之后，`bind`函数将地址和端口绑定在`addr`这个结构中。

- 监听

  ```c
  int listen(int sockfd, int backlog);
  ```

  将`Server`的`socket`置于被动模式，等待`Client`的连接。`backlog`定义了`sockfd`能连接的最大数量。如果连接队列已满，`Client`可能会收到诸如`ECONNREFUSED`之类的提示消息。

- 接受

  ```c
  int new_socket= accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
  ```

  `Server`分析监听队列里的第一个请求，然后建立一个新的`socket`，并返回一个描述符。此时，连接已经建立，`Server`和`Client`能够彼此通信。

### 对于 Client 

- 建立，和`Server`一样

- 连接

  ```c
  int connect(int sockfd, const struct sockaddr *addr,  
                               socklen_t addrlen);
  ```

  `Server`的地址和端口信息都在`addr`结构中指定。`connect`系统调用将本地`sockfd`连接到`addr`所标识的`Server`。

### 实现

下面是一个例子。

**`Server`：**

```c
// Server side 
#include <unistd.h> 
#include <stdio.h> 
#include <sys/socket.h> 
#include <stdlib.h> 
#include <netinet/in.h> 
#include <string.h> 
#define PORT 8080 
int main(int argc, char const *argv[]) 
{ 
	int server_fd, new_socket, valread; 
	struct sockaddr_in address; 
	int opt = 1; 
	int addrlen = sizeof(address); 
	char buffer[1024] = {0}; 
	char *hello = "Hello from server"; 
	
	// Creating socket file descriptor 
	if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) 
	{ 
		perror("socket failed"); 
		exit(EXIT_FAILURE); 
	} 
	
	// Forcefully attaching socket to the port 8080 
	if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, 
												&opt, sizeof(opt))) 
	{ 
		perror("setsockopt"); 
		exit(EXIT_FAILURE); 
	} 
	address.sin_family = AF_INET; 
	address.sin_addr.s_addr = INADDR_ANY; 
	address.sin_port = htons( PORT ); 
	
	// Forcefully attaching socket to the port 8080 
	if (bind(server_fd, (struct sockaddr *)&address, 
								sizeof(address))<0) 
	{ 
		perror("bind failed"); 
		exit(EXIT_FAILURE); 
	} 
	if (listen(server_fd, 3) < 0) 
	{ 
		perror("listen"); 
		exit(EXIT_FAILURE); 
	} 
	if ((new_socket = accept(server_fd, (struct sockaddr *)&address, 
					(socklen_t*)&addrlen))<0) 
	{ 
		perror("accept"); 
		exit(EXIT_FAILURE); 
	} 
	valread = read( new_socket , buffer, 1024); 
	printf("%s\n",buffer ); 
	send(new_socket , hello , strlen(hello) , 0 ); 
	printf("Hello message sent\n"); 
	return 0; 
} 
```

**`Client`：**

```c
// Client side
#include <stdio.h> 
#include <sys/socket.h> 
#include <arpa/inet.h> 
#include <unistd.h> 
#include <string.h> 
#define PORT 8080 

int main(int argc, char const *argv[]) 
{ 
	int sock = 0, valread; 
	struct sockaddr_in serv_addr; 
	char *hello = "Hello from client"; 
	char buffer[1024] = {0}; 
	if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) 
	{ 
		printf("\n Socket creation error \n"); 
		return -1; 
	} 

	serv_addr.sin_family = AF_INET; 
	serv_addr.sin_port = htons(PORT); 
	
	// Convert IPv4 and IPv6 addresses from text to binary form 
	if(inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr)<=0) 
	{ 
		printf("\nInvalid address/ Address not supported \n"); 
		return -1; 
	} 

	if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) 
	{ 
		printf("\nConnection Failed \n"); 
		return -1; 
	} 
	send(sock , hello , strlen(hello) , 0 ); 
	printf("Hello message sent\n"); 
	valread = read( sock , buffer, 1024); 
	printf("%s\n",buffer ); 
	return 0; 
} 
```

编译：

```bash
gcc client.c -o client
gcc server.c -o server
```

输出：

```bash
Client:Hello message sent
Hello from server
Server:Hello from client
Hello message sent
```
