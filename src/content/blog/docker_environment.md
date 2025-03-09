---
title: "Docker environment"
description: "What is Docker and how to install it"
pubDate: " Mar 09 2025"
image: /image/image3.png
categories:
  - tech
tags:
  - docker
  - environment
badge: Pin
---

这边主要是配置Docker环境的内容（哪怕你的电脑不支持hyper-v，Dno't be intimidated,this article will guide you through the world of Docker.）

## 0、在配置docker前，让我们先了解一下Docker吧！
随着对大语言模型的深度使用，我们现在需要使用一些更加复杂的工具。这些工具并不是可以支持原生的Windows运行环境。为了能够在Windows上方便地使用这些软件或框架集，我们需要安装一套功能非常强大的运行环境，这个运行环境叫做Docker。

#### 0-1、什么是Docker呢？
那么什么是Docker呢？你可以查到很多与它相关的技术资料，但对于刚刚入门的朋友来说，可能会显得有些晦涩难懂。

简单来说，Docker是一种虚拟化技术，也就是在当前的操作系统下创建一个相对独立的特定操作系统环境，用来运行我们所需要的软件。Docker的虚拟化介于虚拟机和沙盒之间，是一种非常高效的虚拟化方式。
![blog placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2Ff7a04bd2-226f-413d-93b5-f8255a68d1c3%2FUntitled.png?table=block&id=143b1a9e-1c06-4715-8070-9f188e40b314&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=2000&userId=&cache=v2)


#### 0-2、Docker有什么优势呢？
每个运行的Docker容器都捆绑了这个容器所运行的软件以及最低程度所需求的系统环境。举个例子，如果我需要运行一个Docker中的文本编辑器，那么Docker不会给它提供超出文本编辑器之外的系统组件，例如处理音频、视频、网络通信等这一类的组件。因此，我们的Docker容器通常非常精简，运行效率极高。

如果你在Windows上运行一个Linux的虚拟机，则必须附带Linux系统的所有系统组件。即便我只想在这个虚拟机中运行一个简单的文本编辑器，它同样会加载音频、视频、网络通信等系统核心组件。
![blog placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2F875f76db-29c7-47b6-9b4f-467512aa7623%2FUntitled.png?table=block&id=8fa5453b-05d0-4a9f-8bde-a75d65322a2d&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=2000&userId=&cache=v2)

#### 0-3、Docker与Windows间的爱恨情仇
Docker在Windows上的适配经历了三个阶段：第一个阶段是完全不适配Windows，因为Docker本身是构建在Linux系统上的一个虚拟化技术；第二阶段是有限的基于虚拟机的适配；第三阶段是基于WSL2（Windows Subsystem for Linux 2）虚拟化技术的适配。
![blog placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2Fdfdf9238-9e58-4cf2-9e86-dbaa35c779aa%2FUntitled.png?table=block&id=0b67bb76-ec7e-4bdf-aa1e-4b30e0941dba&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=2000&userId=&cache=v2)


## 1、准备好安装Docker了吗？

#### 1-0、在安装docker之前，请确保你的电脑：
首先要注意，想要运行 Docker Desktop，你的windows系统必须高于1904版本。
你可以进入 Windows设置→系统→关于，来查看当前的windows版本。例如我所使用的windows版本号为22h2。
不过请放心，一般来说只有超过5年以上从未更新过的电脑才会停留在1904那个比较古老的版本。

通常只有Windows专业版、企业版或工作站版才能够安装Hyper-V和WSL2。而这两个组件是在Windows上运行Docker Desktop的必备条件。
但是今天我们介绍的安装方法，同时支持Windows家庭版和专业版的安装。我们将会帮助大家绕过一部分Windows系统的限制。

#### 1-1、安装 Hyper-V

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

#### 1-2、安装WSL升级包

然后你需要前往微软的官方网站下载WSL的离线升级包。
或者直接点击[ WSL离线升级包 ](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)这里下载升级包。（会自动弹出下载链接，等待几秒即可）

双击安装。这里我们可能需要再次重启电脑。

#### 1-3、安装Docker Desktop

完成以上两步后，无论你使用的是Windows家庭版还是专业版，你就基本上可以无痛安装Docker。



## 2、安装好docker后，让我们来试着操作一下吧！

#### 2-0、在操作前，我们先了解一下其中相关的以先概念
Docker与其他虚拟化技术最大的区别体现在它是基于应用程序的虚拟化。因此，Docker的官方仓库提供了大量的Docker应用程序包下载服务。每个程序包被称为镜像（Image）。通过一个镜像，我们可以创建一个或多个执行不同参数的应用程序。每个运行中的应用程序被称为容器（Container）。

这是Docker最重要的两个概念：镜像（Image）和容器（Container）。

我们可以下载别人提供的镜像，或者创建自己的镜像。对镜像设置好相关的参数后，启动容器，就实现了利用Docker运行这个应用程序。

还有另外一些概念，我们会在接下来的演示中为大家逐步介绍。

#### 2-1、以Memos举个栗子🌰吧
这是一个小巧的电子备忘录程序，可以创建一个备忘录网站，供我们记录一些碎片化的信息。
![blog placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2F9ac76dbe-4eda-4479-ae15-ada3d15d2557%2FUntitled.png?table=block&id=b3766305-5225-4353-b654-03dd0e755eb6&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=2000&userId=&cache=v2)
###### step1、查找Memos镜像
通过Docker Hub，我们可以查询到Memos的相关信息。Docker Hub使用Tags标签来标注应用程序包的不同版本。这个参数通常以冒号+版本号（或者latest）的形式出现在应用程序包名称的最后。对于一些简单的应用，我们通常选择冒号latest来下载最新版本就可以了。

###### step2、下载Memos镜像
我们有两种方式下载Docker提供的镜像。第一种是通过图形界面搜索，点击选择相应的版本号就可以开始下载；另一个方法是采用命令行的模式输入`docker pull memos:latest`来进行下载。
![blog placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2F28e86364-466c-4367-9303-471220a36282%2FUntitled.png?table=block&id=59704f3c-2279-4a90-86be-60fa7663f412&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=2000&userId=&cache=v2)
通常命令行更适合服务器环境的配置。在日常使用中，我们使用图形界面进行下载就可以了。
下载完成后，我们可以在Images选项卡中看到我们已经下载好的镜像。Memos的镜像很小，只有数十兆。
选择镜像，点击“Run（运行）”，就可以通过这个镜像创建一个Memos应用程序容器。根据不同的需求，我们可以通过一个镜像创建很多个容器。

###### step3、启动Memos容器
每个新建的容器会独立存储它运行过程中产生的新文件，例如我们输入的备忘录信息。然而程序本身仍然依赖于镜像中的文件进行运行，也就是说多个容器采用的是增量存储的方式来处理我们产生的数据。
换句话说，只要存在由某个镜像创建的容器，就必须保持这个镜像一直存储在你的系统中。

## 3、Memos为栗子，了解docker的一些机制吧！

#### 3-1、存储机制
接下来我们要配置Memos的一些系统参数，例如数据的存放位置。我们有两种选择：一种是让Docker自己创建一个封闭的虚拟磁盘用来存储Memos的数据；另一种是绑定本机的特定目录或特定文件。
![docker placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2F9338f4c6-3ad7-452b-ad52-43e16c0b9bd5%2FUntitled.png?table=block&id=9403ee55-4d3b-4d8c-bf89-eea78a0d21c2&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=1000&userId=&cache=v2)

虚拟磁盘在Docker中被称为Volume或者卷。绑定目录被称为Bind绑定或者挂载。这也是Docker中非常重要的概念之一。

这两种方式都会创造永久存储的数据，即使你删除容器和镜像，未来你也可以依靠这些数据重建你的Memos服务。区别在于虚拟磁盘无法直接读取或写入，但是它不存在权限问题；绑定目录或文件方便我们修改查看和修改相关数据，但可能存在权限问题。
![docker placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2Fd90f195f-4b68-482c-a2c4-a2348a9d1b04%2FUntitled.png?table=block&id=c19c5b97-3e9f-4bc4-9604-370ed8a903eb&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=1420&userId=&cache=v2)
这里我们选择绑定目录。我们在D盘新建一个Memos的数据存储目录，我们填写"C:\Users\Adam\Memos"。那么我们要绑定哪个目录呢？这里需要查阅Docker Hub上提供的相关信息。每个应用程序提供的存储位置都不相同。Memos的存储位置是"/var/opt/memos"。
![docker placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2Febf24428-8627-4320-875d-83b89f5d4d05%2FUntitled.png?table=block&id=b9c416f7-2da3-4c62-a27b-4069c28b4e5f&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=1420&userId=&cache=v2)

#### 3-2、端口映射
然后我们需要开放相应的端口。什么是端口呢？端口是计算机中的程序进行相互通信时设定的电子门牌号。通过特定的门牌号，我们就可以与指定的应用程序进行通讯。

Docker提供方便的端口号映射服务，可以将应用程序内的端口映射到当前操作系统的端口。这就好像是将大楼内部的门牌号转换成为街边的邮政信箱号码一样。

我们为Memos设定本地端口5555绑定容器内的端口5230。
![docker placeholder](https://martin123.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F9011b038-6d2b-4f78-b71e-929c63215d0d%2F07a1672a-463d-4cf7-9075-b9ed3dcffce3%2FUntitled.png?table=block&id=19629328-6f95-4fb3-9c41-c73b0e135628&spaceId=9011b038-6d2b-4f78-b71e-929c63215d0d&width=1760&userId=&cache=v2)
这意味着5555号邮政信箱对应的就是环球中心A座80楼80号办公室。

点击“启动”，我们就可以看到一个新的容器被创建了。我们来访问localhost的冒号5555，Memos已经成功运行了，让我们返回Docker的管理界面，点击“容器”可以查看Memos容器正在运行的状态。

这个界面打印的是容器的命令行界面。让我们打开刚才绑定的目录，可以看到Memos创建了一个新的文件，它是Memos的数据库文件。