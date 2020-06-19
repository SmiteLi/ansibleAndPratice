# [»](https://www.vagrantup.com/intro/getting-started/project_setup#project-setup)Project Setup

JUMP TO SECTION

The first step in configuring any Vagrant project is to create a [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile/). The purpose of the Vagrantfile is twofold:

1. Mark the root directory of your project. Many of the configuration options in Vagrant are relative to this root directory.
2. Describe the kind of machine and resources you need to run your project, as well as what software to install and how you want to access it.

Vagrant has a built-in command for initializing a directory for usage with Vagrant: `vagrant init`. For the purpose of this getting started guide, please follow along in your terminal:

配置任何Vagrant项目的第一步是创建一个Vagrantfile。 Vagrantfile的目的是双重的：



标记项目的根目录。 Vagrant中的许多配置选项都与此根目录相关。



描述运行项目所需的机器和资源种类，以及要安装的软件以及访问方式。



Vagrant具有内置命令，用于初始化目录以供Vagrant使用：vagrant init。 出于本入门指南的目的，请在您的终端中进行以下操作：

```shell-session
$ mkdir vagrant_getting_started
$ cd vagrant_getting_started
$ vagrant init hashicorp/bionic64
```

This will place a `Vagrantfile` in your current directory. You can take a look at the Vagrantfile if you want, it is filled with comments and examples. Do not be afraid if it looks intimidating, we will modify it soon enough.

You can also run `vagrant init` in a pre-existing directory to set up Vagrant for an existing project.

The Vagrantfile is meant to be committed to version control with your project, if you use version control. This way, every person working with that project can benefit from Vagrant without any upfront work.

这会将Vagrantfile放在您的当前目录中。 您可以根据需要查看Vagrantfile，该文件中包含注释和示例。 不要害怕它看起来令人生畏，我们将尽快对其进行修改。



您也可以在预先存在的目录中运行vagrant init来为现有项目设置Vagrant。



如果使用版本控制，则Vagrantfile旨在用于项目的版本控制。 这样，每个从事该项目的人都可以从Vagrant中受益，而无需任何前期工作。

## [»](https://www.vagrantup.com/intro/getting-started/project_setup#next-steps)Next Steps

You have successfully created your first project environment. Read on to learn more about [Vagrant boxes](https://www.vagrantup.com/intro/getting-started/boxes).