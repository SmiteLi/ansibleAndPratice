# Getting Started

JUMP TO SECTION

The Vagrant getting started guide will walk you through your first Vagrant project, and show off the basics of the major features Vagrant has to offer.

If you are curious what benefits Vagrant has to offer, you should also read the ["Why Vagrant?"](https://www.vagrantup.com/intro) page.

The getting started guide will use Vagrant with [VirtualBox](https://www.virtualbox.org/), since it is free, available on every major platform, and built-in to Vagrant. After reading the guide though, do not forget that Vagrant can work with [many other providers](https://www.vagrantup.com/intro/getting-started/providers).

Before diving into your first project, please [install the latest version of Vagrant](https://www.vagrantup.com/docs/installation/). And because we will be using [VirtualBox](https://www.virtualbox.org/) as our provider for the getting started guide, please install that as well.

《 Vagrant入门指南》将引导您完成第一个Vagrant项目，并展示Vagrant提供的主要功能的基础。



如果您想知道流浪汉有什么好处，您还应该阅读“为什么要流浪汉？” 页。



入门指南将Vagrant与VirtualBox一起使用，因为它是免费的，可在每个主要平台上使用，并且内置于Vagrant中。 在阅读了指南之后，请不要忘记Vagrant可以与许多其他提供商一起使用。



在进入第一个项目之前，请安装最新版本的Vagrant。 并且由于我们将使用VirtualBox作为入门指南的提供者，因此也请安装它。

**More of a book person?** If you prefer to read a physical book, you may be interested in [Vagrant: Up and Running](https://www.amazon.com/gp/product/1449335837/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1449335837&linkCode=as2&tag=vagrant-20), written by the author of Vagrant and published by O'Reilly.

- 安装最新的virtualBox：https://www.virtualbox.org/wiki/Downloads
- 安装最新版的Vagrant：https://www.vagrantup.com/downloads

## [»](https://www.vagrantup.com/intro/getting-started#up-and-running)Up and Running

```shell-session
$ vagrant init hashicorp/bionic64
$ vagrant up
vagrant ssh
vagrant destroy
```

After running the above two commands, you will have a fully running virtual machine in [VirtualBox](https://www.virtualbox.org/) running Ubuntu 18.04 LTS 64-bit. You can SSH into this machine with `vagrant ssh`, and when you are done playing around, you can terminate the virtual machine with `vagrant destroy`.

Now imagine every project you've ever worked on being this easy to set up! With Vagrant, `vagrant up` is all you need to work on any project, to install every dependency that project needs, and to set up any networking or synced folders, so you can continue working from the comfort of your own machine.

The rest of this guide will walk you through setting up a more complete project, covering more features of Vagrant.

运行以上两个命令后，您将在VirtualBox中拥有一个完全运行的虚拟机，该虚拟机运行64位Ubuntu 18.04 LTS。 您可以使用vagrant ssh SSH进入该计算机，并在完成游戏之后，可以使用vagrant destroy终止虚拟机。



现在，想象一下您曾经致力于的每个项目都可以如此轻松地设置！ 使用Vagrant，您可以在任何项目上工作，安装项目所需的每个依赖项以及设置任何网络或同步文件夹，而无所事事，因此您可以在自己的机器上舒适地继续工作。



本指南的其余部分将引导您完成一个更完整的项目，涵盖Vagrant的更多功能。

