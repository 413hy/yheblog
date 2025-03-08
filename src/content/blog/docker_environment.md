---
title: "docker environment"
description: "这边是配置docker环境的内容（哪怕你的电脑不支持hyper-v）"
pubDate: " Mar 2025"
image: /image/image3.png
categories:
  - tech
tags:
  - docker
  - environment
badge: Pin
---

这边是配置docker环境的内容（哪怕你的电脑不支持hyper-v）

## 1、前置条件

#### 在安装docker之前，请确保你的电脑：
首先要注意，想要运行 Docker Desktop，你的windows系统必须高于1904版本。
你可以进入 Windows设置→系统→关于，来查看当前的windows版本。例如我所使用的windows版本号为22h2。
不过请放心，一般来说只有超过5年以上从未更新过的电脑才会停留在1904那个比较古老的版本。

通常只有Windows专业版、企业版或工作站版才能够安装Hyper-V和WSL2。而这两个组件是在Windows上运行Docker Desktop的必备条件。
但是今天我们介绍的安装方法，同时支持Windows家庭版和专业版的安装。我们将会帮助大家绕过一部分Windows系统的限制。

## 2、安装 Hyper-V

首先我们需要运行一个批处理程序来启动Hyper-V组件。

通过将下方的代码复制到记事本中，并另存为`enable_hyper_v.cmd`，之后直接右击该脚本，通过“管理员模式”运行。

````markdown mockup-code

pushd "%~dp0"

dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt

for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"

del hyper-v.txt

Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL

````

如果你的电脑没有Hyper-V组件，系统将会从Windows Update服务器那里更新Windows的相关组件，这可能会花费一些时间。安装完成后，系统需要重启。

## 3、安装WSL升级包

然后你需要前往微软的官方网站下载WSL的离线升级包。
或者直接点击[ WSL离线升级包 ](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)这里下载升级包。（会自动弹出下载链接，等待几秒即可）

双击安装。这里我们可能需要再次重启电脑。


## 4、安装Docker Desktop

完成以上两步后，无论你使用的是Windows家庭版还是专业版，你就基本上可以无痛安装Docker。