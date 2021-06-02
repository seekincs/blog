---
author: "trayvonpan"
title: "使用 top 命令监视进程运行状态"
date: 2019-09-18T16:43:26+08:00
draft: false
description: ""
tags: ["Linux"]
---

在 Linux 中，top 是个很常用的命令，用来显示系统正在运行的进程，其输出的信息有时候对系统资源的监控很重要。一些重要的指标常常代表了不同的含义，理解这些指标才能更好地利用 top 提供的信息。

<!--more-->

![](top.png)

- 系统运行时间和平均负载(load average)，例如：

   <div class="note-default">
   top - 10:54:48 up 9 min,  1 user,  load average: 0.78, 0.57, 0.33
   </div>

  这行的字段分别显示了当前时间，系统已经运行的时间，当前登录的用户数和相应的 5、10、15 分钟的平均负载。


- 任务或者进程总结，例如：

  <div class="note-info">
  Tasks: 291 total, 1 running, 242 sleeping, 0 stopped, 0 zombie
  </div>

  这行显示了当前总的任务(进程)数，以及正在运行、睡眠、停止和僵尸(zombie)的进程数。

- CPU 状态，即不同模式下所占的 CPU 百分比，例如：

  <div class="note-primary">
  %Cpu(s): 10.3 us, 3.1 sy, 0.1 ni, 85.8 id, 0.2 wa, 0.0 hi, 0.6 si, 0.0 st
   </div>

  这行显示：

  - us => user：运行(未调整优先级)用户进程的 CPU 时间
  - sy => system，运行内核进程的 CPU 时间
  - ni => niced，运行已调整优先级的用户进程的 CPU 时间
  - wa => io wait，用于等待 IO 完成的 CPU 时间
  - hi => hardware interrupt：处理硬件中断的时间
  - si => software interrupt：处理软件中断的时间

- 内存使用情况

  <div class="note-default">
  KiB Mem : 8075192 total, 4250172 free, 1792168 used, 2032852 buff/cache
  KiB Swap: 2097148 total, 2097148 free, 0 used. 5424744 avail Mem
  </div>

  第一行显示物理内存使用情况，第二行是虚拟内存使用情况。大致都可以分为全部可使用内存、空闲内存、已使用内存、缓冲内存。

- 任务信息列，列出每个进程的一些信息。

  一般有以下属性：

  - PID，进程 ID，进程的唯一标识符
  - USER，进程所有者的实际用户名
  - PR，进程调度的优先级别，rt 表示运行在实时态
  - NI，进程的 nice 值(优先级)，越小的值代表越高的优先级
  - VIRT，进程使用的虚拟内存
  - RES，驻留内存大小
  - SHR，进程使用的共享内存
  - S，进程的状态，有以下值：
    - D，不可中断睡眠
    - R，运行态
    - S，睡眠态
    - T，被跟踪或已经停止
    - Z，僵尸态
  - %CPU，自上一次给你更新到现在任务所使用的 CPU 时间的百分比
  - %MEM，进程使用的可用物理内存百分比
  - TIME+，进程从启动到现在所使用的全部 CPU 时间
  - COMMAND，进程所使用的命令
