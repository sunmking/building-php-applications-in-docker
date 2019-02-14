# 第一章. Docker 的介绍

自2013年首次发布以来，Docker已经成为许多开发人员首选的容器引擎，它可能很快就会取代您身边的虚拟机。这本书提供了一个循序渐进的指南，它将指导您使用Docker构建一个真正的PHP web应用程序，同时介绍该平台的基础知识。

我们在本书中构建的应用程序将完成PHP开发人员每天需要完成的许多关键工作，包括:

* 使用[Composer](https://getcomposer.org/)j进行依赖安装

* 使用web框架([SlimPHP](https://www.slimframework.com/))进行路由

* 从第三方API获取数据

* 保存数据到数据库

* 安全地设置环境变量

这本书是为经验丰富的PHP开发人员编写的，他们希望学习如何使用Docker开发web应用程序，了解Docker如何工作，并喜欢通过构建实际的、工作的应用程序来学习。一旦你完成了这本书，将为你学习更高级的Docker知识做好准备，并且我在最后也提供了一些资源来帮助你。

我在本书中包含的所有代码都是开源的，并且[可以在Github上免费获得](https://github.com/shiphp/weather-app)。如果您发现任何问题或您想提出改进建议，请随时提出pull request。最后，经常查看[www.shiphp.com](https://www.shiphp.com/)关于Docker和PHP的博客文章、培训和新书。

## Docker 是什么?

Docker是一个管理和运行容器的平台。容器类似于[虚拟机](https://en.wikipedia.org/wiki/Virtual_machine)，但它们实际上并不模拟整个操作系统。相反，您运行的所有容器与主机共享相同的底层内核，这意味着它们比虚拟机轻得多。因此，容器非常高效，并且大多数实际应用程序同时运行多个容器。Docker帮助您使用容器的[networks](https://docs.docker.com/engine/userguide/networking/)将这些容器链接在一起，并帮助您使用[Docker Compose](https://docs.docker.com/compose/)配置文件定义容器。虽然本书没有详细介绍这些主题，但它将帮助您入门，更深入的内容可以在[www.shiphp.com](https://www.shiphp.com/)上找到。

{width=100%}
![图1:Docker容器vs.虚拟机](images/diagram1.png)

## 为什么使用 Docker?

熟悉虚拟机或使用过[Vagrant](https://www.sitepoint.com/5-easily-ways-getting-startedphp.vagrant/)的PHP开发人员通过将容器与虚拟机进行比较，将最容易理解容器。在实践中，容器和虚拟机之间的重要区别是，您很少为整个应用程序使用*一个* Docker容器。相反，您将在一个容器中运行PHP代码，在另一个容器中运行web服务器，在第三个容器中运行数据库。我工作过的一些应用程序使用几十个链接的容器来操作!

如果您以前没有使用过虚拟机，Docker容器仍然可以帮助您改进开发工作流。在本地操作系统上运行PHP和Apache的团队中工作的开发人员经常遇到“在我的机器上工作”的问题。一个团队成员更新web应用程序，一切看起来都很正常，但是当应用程序部署到服务器上或由另一个团队成员运行时，它会神秘地中断。Docker通过允许开发人员设置和共享可复制的开发环境，然后使用容器在完全相同的状态下*测试和部署应用程序来解决这些问题。

此时，您可能需要阅读更多关于Docker的信息。尽管本书将介绍“构建和运行”在Docker上的PHP网络应用程序，但它不会覆盖Docker、容器或操作系统虚拟化的内部工作。你可以在[Docker的网站](https://www.docker.com/what-docker)上阅读更多关于Docker和这些相关主题的内容。

## 我能从这本书中学到什么?

虽然容器背后的计算机科学是一个有趣的话题，但这本书采用了实用的方法。许多PHP开发人员(包括我自己)都是实践型学习者，所以我写这本书是为了“帮助那些通过实践学习的人”。在这过程中，我还包括了一些图表用于视觉学习者，代码样本(以及Github的一个完整的代码repo[Github](https://github.com/shiphp/weather-app))，并链接到免费和付费的资源来了解更多关于每个主题的内容。

阅读[Docker的文档](https://doc,docker.com/)是个好主意，但我发现用一个有效的应用程序可以让我更加明确地了解这些文档，并且可以加深我对文档的理解。如果您是一名PHP开发人员，这本书将带您踏上构建您的第一个应用程序的旅程，并为您提供您需要的基础知识，以便真正理解Docker如何工作，以及如何使用它来构建更好的软件。

我们在本书中构建的应用程序是简单的[REST](https://stackoverflow.com/questions/671118/what-exactlyis-restful-programming) API，但是它涵盖了PHP开发人员在使用Docker时需要解决的大多数典型问题。在构建此应用程序的整个过程中，我将向您展示如何使用[Composer](https://getcomposer.org/)安装包，从第三方API获取数据，将数据保存到数据库，并在Docker中使用环境变量。在构建应用程序时，大多数细节将被揭示，而不是预先解释所有内容。

学习这本书最好的办法就是坐在你的电脑前，用一个下午的时间去完成它。希望到最后，你可以开始为你的下一个PHP项目使用Docker了。

这本书有几个先决条件。您应该了解什么是[PHP](http://php.net/manual/zh/)及其语法的基本知识，您应该知道如何打开计算机的终端，并从中运行PHP脚本，您应该在您的计算机上安装版本17(Docker(可用于Mac、Windows或Linux))[docker](https://www.docker.com/community-edition)。假设你有这些东西，让我们开始吧!