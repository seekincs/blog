---
author: "Trayvon Pan"
title: "Vim 简明手册"
date: 2019-08-30T17:49:29+08:00
draft: false
description: ""
tags: ["Linux"]
toc: false
---


### 移动游标

- 使用`↑`、`↓`、`←`、`→`箭头即可。
- `2w`：光标向前移动两个单词，并停在单词开始处，其中`2`可以是其他数字，表示移动的距离。同理，`2e`也是移动两个单词的距离，但光标停留在单词的末尾。
- `0`将光标移动到行首。
- 按下`G`将光标移动到文件末尾，`gg`将光标移动到开头。
- `5G`：将光标移动到第一次按下`CTRL+G`开始的第五行，`5`可以换成其他数字。
- 将光标置于括号处，按下`%`，光标会移动到匹配的另一个括号处。


<!--more-->
注意：如果不确定输入了什么指令，按下`ESC`回到正常模式。
### 退出`Vim`

- 按下`ESC`
- 输入：`:q!`，回车。这样会放弃所有更改并退出`Vim`。
- 输入：`:qw`，保存文件内容并退出。

### 删除文本

- 在正常模式下，按下`x`以删除光标处的字符。
- 将光标置于单词开始处，按下`dw`会删除这个单词，此后光标会移动到下一个单词开始的地方。`d2w`删除两个单词，其中`2`表示要删除的单词数。
- `d$`删除从光标开始到行末的文本。
- `de`删除从光标开始的单词，此后光标会移动到该单词的最后一个字符的下一个字符。
- `dd`删除整行。

### 插入

- 按下`i`以进入插入模式。
- 使用`dd`删除一行后，按下`p`会将删除的一行添加到光标所在行的下面。

### 追加

按下`A`在行末添加文本。

### 撤销

- `u`撤销一次操作，`U`撤销当前光标所在行的操作。

### 文件状态

- `CTRL+G`在终端显示文件位置、当前所在行数、总行数、占比等。

### 搜索

- 输入`/`以及要搜索的命令，回车。要继续往下搜索，按下`n`；往上搜索，按下`N`。`CTRL+O`回到搜索前光标开始处。

### 执行外部命令

- 输入`:!`，以及外部命令即可。

### 复制和粘贴

- 将光标置于要复制的文本开始或者结束处，按下`v`，进入`Visual`模式，移动光标选择要复制的文本，随后按下`y`复制；在要粘贴的地方，按下`p`粘贴。

### 设置选项

- 在`Normal`模式下，按下`:`，输入设置的内容。比如，`set number`将显示行号。

