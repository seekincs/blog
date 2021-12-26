---
author: "trayvonpan"
title: "如何编写整洁的代码"
date: 2020-11-25T13:40:48+08:00
draft: false
description: ""
tags: ["Code Design"]
toc: false
---

写代码犹如写文章。整洁的代码让人读起来舒服，也便于后期的维护。

最近在读《代码整洁之道》这本书，作者总结了不少关于编写整洁代码的操作实践，这些实践具有很高的借鉴价值。

“阅读本书有两种原因：第一，你是个程序员；第二，你想成为更好的程序员。”

<!--more-->

怎么衡量代码的质量？有一种简单实在的方式，就是看你阅读代码的时候每分钟说“WTF”的次数，次数越多，代码质量越低。

![wtf_p_min](wtf_p_min.jpg)

为什么要关注代码的整洁性？因为代码质量与其整洁度成正相关。和信奉“能跑就行”的程序员不同，优秀的程序员在使代码能跑的同时，还想办法让代码更加优美。`《C++Programming Language》`的作者`Bjarne Stroustrup`就是其中的典型代表。他说：“**我喜欢优雅和高效的代码。代码逻辑应当直截了当，叫缺陷难以隐藏；尽量减少依赖关系，使之便于维护；依据某种分层战略完善错误处理代码；性能调至最优，省得引诱别人做没规矩的优化，搞出一堆混乱来。整洁的代码只做好一件事。**”

想要写出整洁的代码，就要不断地训练。要不断地阅读大量的代码，而且要去琢磨某段代码好在什么地方或者坏在什么地方，以形成一种习惯。代码的整洁性应该从程序的各个方面着手，包括格式、命名、注释、类和错误处理等等。

### 0. 命名

首先，应该取有意义的变量名，应该让人一看到一个变量就能知道（猜测）“这是什么”。糟糕的变量名是让别人阅读你写的代码的第一道障碍。比如下面的例子。

```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
  if (x[0] == 4)
  list1.add(x);
  return list1;
}
```

看到这样的代码，第一感觉可能就是对代码中变量名的疑惑。

（1）theList 中是什么类型的东西？</br>
（2）theList 零下标条目的意义是什么？</br>
（3）值 4 的意义是什么？</br>
（4）我怎么使用返回的列表？


只要对其中的命名稍加改进，可读性就高很多了。

```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>();
  for (int[] cell : gameBoard)

  if (cell[STATUS_VALUE] == FLAGGED)
  flaggedCells.add(cell);
  return flaggedCells;
}
```

其次，不要起误导性的名称。“误导性名称真正可怕的例子，是用小写字母`l`和大写字母`O`作为变量名，尤其是在组合使用的时候。”

```java
int a = l;
if (O == l)
  a = O1;
else
  l = 01;
```

应该使用读得出来的名称，这样在讨论的时候会好一点。

```java
// 反例
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
    /* ... */
};

// 正例
class Customer {
  private Date generationTimestamp;
  private Date modificationTimestamp;;
  private final String recordId = "102";
  /* ... */
};
```

### 1. 类

“类名和对象名应该是名词或名词短语，如`Customer`、`WikiPage`、`Account`和`AddressParser`。避免使用`Manager`、`Processor`、`Data`或`Info`这样的类名。类名不应当是动词。而方法名应当是动词或动词短语，如`postPayment`、`deletePage`或`save`。”

“类的名称应当描述其权责。实际上，命名正是帮助判断类的长度的第一个手段。如果无法为某个类命以精确的名称，这个类大概就太长了。类名越含混，该类越有可能拥有过多权责。例如，如果类名中包括含义模糊的词，如`Processor`或`Manager`或`Super`，这种现象往往说明有不恰当的权责聚集情况存在。”

“类应该只有少量实体变量。类中的每个方法都应该操作一个或多个这种变量。通常而言，方法操作的变量越多，就越黏聚到类上。如果一个类中的每个变量都被每个方法所使用，则该类具有最大的内聚性。”

### 2. 函数

- “函数的第一规则是要短小。第二条规则是还要更短小。”

- “函数应该做一件事。做好这件事。只做这一件事。”

通常，函数作为一段逻辑过程的封装，应该尽可能做最少的事情。同时还要注意结合代码的阅读顺序。我们从上到下阅读代码，所以，应该让每个函数后面都跟着位于下一抽象层级的函数，这样一来，在查看函数列表时，就能偱抽象层级向下阅读了。这个叫做向下规则。

函数的参数也是值得关注的内容。

“最理想的参数数量是零（零参数函数），其次是一（单参数函数），再次是二（双参数函数），应尽量避免三（三参数函数）。”如果函数的参数大于三个，应该把它封装起来进行传递，比如`C`里的结构体、`OOP`里的类等等。

对于多个参数的函数，应该让这些参数更容易区分，不要设计那些意义相近的参数，这很容易使调用者混淆。“即便是如 `assertEquals(expected, actual)`这样的二元函数也有其问题。你有多少次会搞错`actual`和`expected`的位置呢？这两个参数没有自然的顺序。`expected`在前，`actual`在后，只是一种需要学习的约定罢了。”

### 3. 注释

不少人会认为，写注释会让代码可读性更强。其实不一定是这样。

“注释的恰当用法是弥补我们在用代码表达意图时遭遇的失败。注意，我用了“失败”一词。我是说真的。注释总是一种失败。我们总无法找到不用注释就能表达自我的方法，所以总要有注释，这并不值得庆贺。”

“什么也比不上放置良好的注释来得有用。什么也不会比乱七八糟的注释更有本事搞乱一个模块。什么也不会比陈旧、提供错误信息的注释更有破坏性。”

“不准确的注释要比没注释坏得多。它们满口胡言。它们预期的东西永不能实现。它们设定了无需也不应再遵循的旧规则。真实只在一处地方有：代码。只有代码能忠实地告诉你它做的事。那是唯一真正准确的信息来源。所以，尽管有时也需要注释，我们也该多花心思尽量减少注释量。”

“带有少量注释的整洁而有表达力的代码，要比带有大量注释的零碎而复杂的代码像样得多。与其花时间编写解释你搞出的糟糕的代码的注释，不如花时间清洁那堆糟糕的代码。”

“`TODO`是一种程序员认为应该做，但由于某些原因目前还没做的工作。它可能是要提醒删除某个不必要的特性，或者要求他人注意某个问题。它可能是恳请别人取个好名字，或者提示对依赖于某个计划事件的修改。无论`TODO`的目的如何，它都不是在系统中留下糟糕的代码的借口。”

“如果只是因为你觉得应该或者因为过程需要就添加注释，那就是无谓之举。如果你决定写注释，就要花必要的时间确保写出最好的注释。”

“所谓每个函数都要有 `Javadoc` 或每个变量都要有注释的规矩全然是愚蠢可笑的。这类注释徒然让代码变得散乱，满口胡言，令人迷惑不解。”

“有时，程序员喜欢在源代码中标记某个特别位置。例如，最近我在程序中看到这样一行：

```java
// Actions //////////////////////////////////
```

把特定函数趸放在这种标记栏下面，多数时候实属无理。鸡零狗碎，理当删除——特别是尾部那一长串无用的斜杠。”

“直接把代码注释掉是讨厌的做法。别这么干！其他人不敢删除注释掉的代码。他们会想，代码依然放在那儿，一定有其原因，而且这段代码很重要，不能删除。注释掉的代码堆积在一起，就像破酒瓶底的渣滓一般。”

“源代码注释中的`HTML`标记是一种厌物。编辑器或者`IDE`中的代码本来易于阅读，却因为`HTML` 注释的存在而变得难以卒读。如果注释将由某种工具（例如`Javadoc`）抽取出来，呈现到网页，那么该是工具而非程序员来负责给注释加上合适的`HTML`标签。”

### 4. 格式

代码的布局也很重要。现在的编辑器一般都能对代码进行格式化，善于利用这种格式化，可以迅速增加代码的整体美感。

对于循环语句，如果循环体只有一条语句或者为空，可能不少人会省略循环体的大括号。尽量不要这么干。

“有时，while或for语句的语句体为空。我不喜欢这种结构，尽量不使用。如果无法避免，就确保空范围体的缩进，用括号包围起来。我无法告诉你，我曾经多少次被静静安坐在与 while 循环语句同一行末尾的分号所欺骗。除非你把那个分号放到另一行再加以缩进，否则就很难看到它。”

```java
while (dis.read(buf, 0, readBufferSize) != -1)
;
```

### 5. 错误处理

“我认为，要讨论错误处理，就一定要提及那些容易引发错误的做法。第一项就是返回`null`值。我不想去计算曾经见过多少几乎每行代码都在检查`null`值的应用程序。”

“返回`null`值，基本上是在给自己增加工作量，也是在给调用者添乱。只要有一处没检查null值，应用程序就会失控。”

“在方法中返回`null`值是糟糕的做法，但将`null`值传递给其他方法就更糟糕了。除非`API`要求你向它传递`null`值，否则就要尽可能避免传递`null`值。”

### 6. 单元测试

“谁都知道`TDD`要求我们在编写生产代码前先编写单元测试。但这条规则只是冰山一角。看看下列三定律：</br>
**定律一 在编写不能通过的单元测试前，不可编写生产代码。** </br>
**定律二 只可编写刚好无法通过的单元测试，不能编译也算不通过。** </br>
**定律三 只可编写刚好足以通过当前失败测试的生产代码。**”

“测试与生产代码一起编写，测试只比生产代码早写几秒钟。”

### 7. 逐步改进

没有人能够一次就写出整洁、漂亮的代码。编写整洁的代码是一个需要逐步改进的过程。

“**要编写整洁代码，必须先写肮脏代码，然后再清理它。**”

“多数新手程序员（就像多数小学生一样）没有特别认真地遵循这个建议。他们相信，首要任务是写出能工作的程序。只要程序“能工作”，就转移到下一个任务上，而那个“能工作”的程序就留在了最后那个所谓“能工作”的状态。多数老手程序员都知道，这是一种自毁行为。”

注：文中部分观点、代码、图片来自原书，版权归作者`Robert C. Martin`所有。
