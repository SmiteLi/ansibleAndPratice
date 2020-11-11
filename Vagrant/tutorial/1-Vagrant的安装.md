> 本次教程vagrant版本是2.2.12，virtualbox版本是6.1.16，OS为windo 10系统

## 一、vagrant是什么？

Vagrant是用于在单个工作流程中构建和管理虚拟机环境的工具，可与virtualbox、VMware等配合使用。

## 二、安装vagrant

vagrant默认以virtualbox作为基础创建虚拟机，所以使用vagrant前需要安装virtualbox。

1. 安装vagrant：

vagrant的下载地址：https://www.vagrantup.com/downloads

2. 安装virtualbox

virtualbox下载地址：https://www.virtualbox.org/wiki/Downloads

vagrant和virtualbox请安装最新的版本。

安装完vagrant后，重启电脑，打开powershell验证是否安装成功：

```shell-session
$ vagrant
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

...
```

## 三、vagrant快速入门

1.  打开powershell，在D盘下创建vagrant目录，并且进入vagrant目录

```
d:
mkdir vagrant
cd vagrant
```

2. 初始化目录：

```
vagrant init hashicorp/bionic64
```

本操作在vagrant目录内生成Vagrantfile文件，该文件的配置告诉vagrant如何生成一个虚拟机。

3. 启动虚拟机：

```
vagrant up
```
执行上面的操作，会拉取一个box（理解为虚拟机用到的操作系统的iso镜像），然后以该box启动一个虚拟机。但是拉取box的速度可能会很慢。可以在本教程末尾找到百度云的链接下载，再导入。
4. 连接虚拟机

```
vagrant ssh
```

5. 彻底删除虚拟机

logout虚拟机后，如果不需要了，可以彻底删除虚拟机

```
vagrant destroy
```

## 四、本教程用到的软件包下载

