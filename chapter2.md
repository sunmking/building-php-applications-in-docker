# 第二章 . 在Docker中运行PHP脚本

在我们开始构建我们的应用程序之前，先了解一下Docker是如何在Docker中运行一个PHP脚本的。让我们从编写一个经典的Hello World开始! PHP脚本如下:

{title="hello.php", linenos=off, lang=php}
~~~~~~~
<?php
echo "Hello World!";
~~~~~~~

你可以在虚拟机或者笔记本的终端上运行这个脚本(假设你已经安装了PHP)。你应该看看输出Hello World !就像我们在上面输入的那样。

## Docker 镜像介绍

Docker在一个容器中运行每个进程。所有这些容器都在主机上运行，这是你在这本书里的电脑。一旦应用程序准备好进入生产环境，服务器(或多个服务器)将充当Docker主机。

每个运行的容器后面都有一个“镜像”。Docker镜像是由软件开发人员使用dockerfile创建和维护的。换句话说，如果您想从头创建自己的Docker镜像，那么首先要创建一个新的Dockerfile，然后“构建”一个镜像，然后在容器中 “运行” 该镜像。

{width=100%}
![图2:Dockerfiles、镜像、容器](images/diagram2.png)

幸运的是，我们通常不需要从头构建自己的镜像。大多数流行的软件平台(包括PHP)都有由软件开发人员或感兴趣的社区维护的镜像。您很少需要构建一个全新的镜像，但是稍后我们将会讲解到如何通过编写自己的Dockerfile来扩展现有镜像。

Docker映像可以构建并存储在主机上，也可以保存在远程"registry"中。除了维护核心的Docker平台之外，Docker团队还维护一个名为[Docker Hub](https://hub.docker.com/)镜像管理平台，在这里可以免费存储公共镜像。大多数开源软件团队在Docker Hub上托管官方镜像，包括[PHP](https://hub.docker.com/_/php/)。

## 获取PHP Docker 镜像

为了在容器中运行hello.php脚本，首先需要为*pull*一个PHP镜像，选择PHP的最新稳定版本。在你的终端中运行:

{linenos=off, lang=sh}
~~~~~~~
$ docker pull php:latest
~~~~~~~

你应该在你的终端看到类似这样的东西:

{linenos=off, lang=sh}
~~~~~~~
latest: Pulling from library/php
94ed0c431eb5: Pull complete
8d5410041793: Downloading   41.6MB/82.75MB
6ab2a160fa31: Download complete
247a60c07518: Download complete
b09bee244a51: Download complete
b808e084c9be: Downloading  7.222MB/9.858MB
1d362d99e847: Waiting
~~~~~~~

这表示Docker正在拉取PHP最新版镜像。当它完成时，Docker会显示一个如下的状态，表明它已经获取了最新的版本:

{linenos=off, lang=sh}
~~~~~~~
Status: Downloaded newer image for php:latest
~~~~~~~

注意:“latest”标记是大多数Docker映像用于其软件的最新版本的标准约定。不要不加区分地使用“latest”，因为它会自动检索到“latest”版本，即使有重大的版本更改。

因为hello.php脚本很简单，所以我们使用哪个版本的PHP并不重要，但是如果我们需要为一个现有项目运行一个旧版本的PHP呢?这是Docker真正的亮点，因为我们只需要在运行Docker pull时指定PHP版本。例如，要下载PHP 5.6镜像，只需运行如下代码：

{linenos=off, lang=sh}
~~~~~~~
$ docker pull php:5.6
~~~~~~~

我们也可以使用这个方法来获得更新的、未发布的PHP版本(假设[PHP registry's list](https://hub.docker.com/_/php/)中至少有一个Beta版本)。这使得在Docker中运行PHP对于需要经常使用多个PHP版本的开发人员非常有帮助。

## 将代码放入容器中

为了理解下一步，您必须稍微了解Docker如何访问主机系统上的文件。一个正在运行的容器不能直接向下读取或写入文件到您的计算机—该容器本质上它是独立的系统。相反，我们要运行的容器数据来自于主机中挂载的[volume](https://docs.docker.com/engine/admin/volumes/volumes/)或者在构建镜像时添加代码。

在本书的后面，我们将介绍如何从Dockerfiles构建Docker映像并以这种方式添加代码，但是对于这个简单的Hello World!例如，我们将安装hello.php 文件的磁盘挂载到我们需要运行的PHP容器中。

## 在Docker中运行Hello World脚本

现在我们已经从Docker Hub中获取了一些PHP镜像，并且对Docker如何使用卷有了一些了解，我们可以在终端的容器中运行我们的脚本:

{linenos=off, lang=sh}
~~~~~~~
$ docker run --rm -v $(pwd):/app php:latest php /app/hello.php
~~~~~~~

如果一切正确，你在命令行中应该看到输出Hello World!。您刚刚在Docker中运行了第一个PHP脚本!

### 它是如何运行的?

让我们回顾一下Docker命令以及它的含义:

* `docker run` - 这是Docker的命令 [在新容器中运行命令](https://docs.docker.com/engine/reference/run/).有很多选项可供您输入，但我们将从基础知识开始。

* `--rm` - 这告诉Docker在命令运行后“删除”容器。 或者，您可以保存容器以再次运行它，但如果您最终没有删除容器，它会占用空间，因此在大多数情况下最好设置删除选项。

* `-v $(pwd):/app` - 这是Docker的命令 [mount a volume](https://docs.docker.com/engine/tutorials/dockervolumes/). 通常，您将路径传递到主机系统、冒号、以及容器中文件夹的路径上。 卷是一个强大的工具，但是对于这个简单的示例，我们只是将当前目录(使用$(pwd))从终端挂载到新的Docker容器中的/app目录中。

* `php:latest` - 这表示我们为这个容器使用的镜像。您也可以指定其他的PHP镜像(例如:PHP:7.0或PHP:5.6)来使用该语言的特定版本。

* `php /app/hello.php` - 最后，这是Docker将在容器中运行的命令。由于我们将代码挂载在容器的/app目录中，因此必须从该目录运行脚本。

现在您已经对Docker有了基本的了解，并且可以在容器中运行PHP脚本，现在是时候构建一些更有用、更有趣的东西了。这也可能是一个休息的好时机，并阅读一些核心的Docker概念(在他们的文件中)(https://doc,docker.com/)当您准备好之后，请继续阅读本文，开始在Docker中构建PHP web应用程序。