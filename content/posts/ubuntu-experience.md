---
author: "Trayvon Pan"
title: "Ubuntu 体验之旅"
date: 2019-05-01T20:53:00+08:00
draft: false
description: ""
tags: ["Linux"]
toc: false
---


作为 Ubuntu 系统深度用户，一路走过来，仍然遇到了不少麻烦（~~踩了很多坑~~），无论是系统的安装，还是桌面的配置。所以，这里打算将 Ubuntu 的整个安装和配置过程做一个总结，以供参考。

<!--more-->

要想使用 `Ubuntu` 桌面系统，一般有两种方式，一种是直接在物理机器上安装，另一种是借助虚拟机技术。这两种方式我都试过，具体使用那种还是看需要吧，如果是日常玩耍，可以装虚拟机；如果是做一些专业性的工作，感觉还是物理机流畅。

## 在物理机安装

首先得下载系统镜像，可以选择自己想要的版本，截至目前，`Ubuntu` 官方已经推出了 `Ubuntu 20.04 LTS`，这又是一个长期支持的版本，不得不说，`Ubuntu` 桌面真的越来越完善了。访问<a class="link" href="https://ubuntu.com/download/desktop">这里</a>可以下载。

![](ubuntu_download.png)

接下来，需要制作 `USB` 启动机，这个也很简单，如果你正在使用 `Ubuntu` 系统， 打开 `Startup Disk Creator`，你只需要提供下载的 `Ubuntu ISO` 文件，连接 `USB` 驱动器，这个工具就会为你创建可引导的 `Ubuntu USB` 驱动器。

![](startup_disk_creator.png)

当然了，如果你使用的是 `Windows` 系统，可以使用[`Universal USB Installer`](https://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/)。这是个小巧玲珑的工具，只需要配置简单的信息，你一样可以轻松地创建启动器。

![](steps.png)

引导盘制作好之后，重启计算机，按下`F2`或者`F10`或者`ESC`等(不同的计算机会有差异)，引导计算机从`USB`启动，接下来按照引导程序的指示来做，这样，`Ubuntu` 就装在了你的物理机器上了。

## 安装虚拟机

注：这里以 `Ubuntu 18.04 LTS` 为例。

现在大多数计算机都非常强大，可以在自己的操作系统上运行另一个虚拟的操作系统，这意味着，如果你只有一台装着 `Windows` 操作系统的计算机，但凭借计算机的内存和处理能力，你依然可以在其中运行 `Chrome OS`、`Solaris`、`Ubuntu`、`macOS`等系统。

现在市场上也有很多虚拟机软件，比较受欢迎的是 `VMware` 和 `VirtualBox`。这里以 `VMware` 为例，其他软件大同小异。

如果不需要企业级的虚拟机支持，完全可以考虑只用免费版的 `VMware Player`，下载下来直接安装就行了。

完成之后，我们开始创建虚拟机。

打开 `VMware Player`，点击 `Create a New Virtual Machine`，进入创建新虚拟机向导，选择 `I will install the operating system later.`，点击 `Next`，在 `Guest Operating System`中选择 `Linux`，版本为 `Ubuntu 64 bit`，点击 `Next`，填写虚拟机名和存储位置，点击 `Next`，指定初始化磁盘空间大小，勾上 `Split virtual disk into multiple files`，点击 `Next`，点击 `Finish`。点击 `Edit virtual machine settings`，在 `CD/DVD(SATA)` 中选择 `Use ISO image`，并选择下载好的 `Ubuntu` 镜像的位置，保存。

这样，虚拟机就创建好了。

![](created_ubuntu.png)

开机试试看？

点击 `Power On`，将启动虚拟机，进入系统安装页面。选择 `Install Ubuntu`，按部就班跟着向导就可以了。安装成功之后，重启虚拟机。

![](gui_ubuntu_install.png)

开机之后，虚拟机并不能自适应宿主机屏幕的大小，需要调整。

在 `VMware Player` 中选择 `Virtual Machine` 下拉菜单，选择 `Install VMware Tools`，这将会在虚拟机上安装一个驱动。

<div class="note-primary">
 VMware Tools 是一套可以提高虚拟机客户机操作系统性能并改善虚拟机管理的实用工具。例如，在安装 VMware Tools 后，我们将可以：</br>
 1. 主机与客户机文件系统之间的共享文件夹</br>
 2. 在虚拟机与主机或客户端桌面之间复制并粘贴文本、图形和文件</br>
 3. 改进的鼠标性能</br>
 4. 虚拟机中的时钟与主机或客户端桌面上的时钟同步</br>
 5. 帮助自动执行客户机操作系统操作的脚本</br>
</div>

安装成功之后，打开 `VMware Tools`，可以看到 `manifest.txt`，`run_upgrader.sh`，`VMwareTools-xxx.tar.g`z（这个`xxx`是具体的版本号），`vmware-tools-upgrader-32`，`vmware-tools-upgrader-64` 这几个文件，将 `VMwareTools-xxx.tar.gz` 这个压缩包拷贝到某个目录（比如`Documents`）并解压，打开终端，进入 `VMwareTools-xxx/vmware-tools-distrib`，运行 `vmware-install.pl` 这个文件。

```bash
$ sudo ./vmware-tools.pl
```

这个脚本将会启动`VMware Tools`的安装，出现确认选项，一路选择默认即可。

如果最后看到下面的提示信息，说明已经安装成功了。

<div class="note-info">
Enjoy,

 –the VMware team
</div>

重启虚拟机，发现已经可以全屏了。

![](ubuntu_installed.png)

这样，我们就成功装上了`Ubuntu 18.04 LTS`，接着可以更新一下软件，做一些个性化的配置，我将在下一篇文章中分享我个人的方案。

*希望本文对你有帮助：）*
